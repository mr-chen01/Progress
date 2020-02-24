<template>
  <div>
    {{ msg }}
    <!-- prop传入数字、数组、布尔值、对象时都需要加v-bind，传入字符串时需要判断动态静态 -->
    <!-- nihao -->
    <x-process :stroke-width=20 :percentage=60 status="success" :text_inside=true />
    <x-process :percentage=40 status="success" :text_inside=false />
  </div>
</template>

<script>
import XProcess from "./Progress";
export default {
  components: {
    XProcess
  },
  data() {
    return {
      msg: ""
    };
  }
};
</script>