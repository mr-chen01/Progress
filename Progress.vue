<template>
  <div class="progress"
    :class="[status?`is-${status}`:'']">
    <!-- 注意 -->
    <div class="progress_bar">
      <div class="progress_bar_outer" :style="{height:strokeWidth+'px'}">
        <div class="progress_bar_inner" :style="barStyle">
          <!-- 判断是否内显 -->
          <div class="progress_bar_innerText" v-if="text_inside">{{ percentage }}%</div>
        </div>
      </div>
    </div>
    <div class="progress_text" 
  :style="{fontSize:processTextSize+'px'}"
    v-if="!text_inside "
    >
      <template v-if="!status">{{ percentage }}%</template>
      <i v-else class="icon" :class='iconClass'></i>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    // 要从外界接受的值
    strokeWidth: {
      type: Number,
      default: 6
    },
    percentage: {
      type: Number,
      default: 0,
      required: true,
      validator(value) {
        return value <= 100 && value >= 0;
      }
    },

    status: {
      //进度条当前状态
      type: String
    },
    type: {
      type: String,
      default: "line",
      validator: val => ["circle", "line"].includes(val)
    },
    text_inside:{
      type:Boolean,
      default:false
    }
  },
  computed: {
    //计算属性
    processTextSize() {
      return 12 + this.strokeWidth * 0.4;
    },
    stroke() {
      //进度条填充的颜色
      let color;
      switch (this.status) {
        case "success":
          color = "#13ce66";
          break;
        case "exeption":
          color = "#ff4949";
          break;
        default:
          color = "#20a0ff";
      }
      return color;
    },
    barStyle() {
      return {
        width: this.percentage + "%", //在计算属性中要时刻注意this
        backgroundColor: this.stroke
      };
    },
    iconClass() {
      if (this.type === "line") {
        return this.status === "success"
          ? "icon-circle-check"
          : "icon-circle-close"  //不用加分号
      } else {
        return this.status === "success"
          ? "icon-check"
          : "icon-close"
      }
    }
  }
};
</script>

<style>
@font-face {
  font-family: "iconfont"; /* project id 1651751 */
  src: url("./font/iconfont.eot");
  src: url("./font/iconfont.eot?#iefix") format("embedded-opentype"),
    url("./font/iconfont.woff2") format("woff2"),
    url("./font/iconfont.woff") format("woff"),
    url("./font/iconfont.ttf") format("truetype"),
    url("./font/iconfont.svg#iconfont") format("svg");
}
.icon {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
}
/* 代码中把x去了加上转义\ */
.icon-circle-check::before {
  content: "\e615";
}
.icon-circle-close::before {
  content: "\e690";
}
.progress_bar {
  display: inline-block;
  /* css3特殊定义 */
  width: calc(100% - 50px);
  box-sizing: border-box;
}
.is-success .progress_text{
  color:green;
}
.is-fail .progress_text{
  color:red;
}
.progress_bar_outer {
  background-color: #ebeef5;
  border-radius: 100px;
}
.progress_bar_inner {
  height: 100%;
  text-align: right;
  border-radius: 100px;
}
.progress_text {
  display: inline-block;
  margin-left: 10px;
}
.progress_bar_innerText{
  color: #fff;
display: inline-block;
font-size: 15px;
padding-right: 6px;
}
</style>