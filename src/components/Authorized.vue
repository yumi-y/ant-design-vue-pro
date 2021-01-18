<script>
//权限调 验组件
import { check } from "../utils/auth.js";
export default {
  functional: true,
  props: {
    //进行声明，为必须传递的属性
    authority: {
      type: Array,
      required: true,
    },
  },
  render(h, context) {
    const { props, scopedSlots } = context;
    //调研通过，返回插槽，不通过，返回null
    return check(props.authority) ? scopedSlots.default() : null;
  },
};
</script>
