<template>
  <div ref="chartDom"></div>
</template>

<script>
import echarts from "echarts/lib/echarts";
import "echarts/lib/chart/bar";
import "echarts/lib/component/title";
//resize-detector用来监听元素进行自适应高度
import { addListener, removeListener } from "resize-detector";
import debounce from "lodash/debounce";
export default {
  props: {
    option: {
      type: Object,
      default: () => {},
    },
  },
  watch: {
    option(val) {
      this.chart.setOption(val);
    },
    //深度监听，比较耗性能
    //option: {
    //  handler(val) {
    //    this.chart.setOption(val);
    //  },
    //  deep: true,
    //},
  },
  //进行防抖处理
  created() {
    this.resize = debounce(this.resize, 300);
  },
  mounted() {
    this.renderChart();
    addListener(this.$refs.chartDom, this.resize);
  },
  //使用之后销毁，防止占用内存
  beforeDestroy() {
    removeListener(this.$refs.chartDom, this.resize);
    this.chart.dispose();
    this.chart = null;
  },
  methods: {
    resize() {
      this.chart.resize();
    },
    renderChart() {
      this.chart = echarts.init(this.$refs.chartDom);
      this.chart.setOption(this.option);
    },
  },
};
</script>
