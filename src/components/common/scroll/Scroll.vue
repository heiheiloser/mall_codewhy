<!--
 * @Author: your Name
 * @Date: 2022-07-20 22:39:50
 * @LastEditors: your Name
 * @LastEditTime: 2022-08-20 14:38:17
 * @Description: 
-->
<template>
  <div class="scroll-wrapper" ref="wrapper">
    <div class="scroll-content">
      <slot />
    </div>
  </div>
</template>

<script>
import BScroll from "better-scroll";

export default {
  name: "Scroll",
  props: {
    probeType: {
      type: Number,
      default() {
        return 0;
      }
    },
    pullUpLoad: {
      type: Boolean,
      default() {
        return false;
      }
    }
  },
  data() {
    return {
      scroll: null
    };
  },
  mounted() {
    this.scroll = new BScroll(this.$refs.wrapper, {
      // 开启点击事件,默认是false
      click: true,
      // 0和1不监听滚动事件,2监听但是不监听手指松开后的滑动距离,3全部监听
      probeType: this.probeType,
      // 是否开启监听滚动到底部事件
      pullUpLoad: this.pullUpLoad,
      // mouseWheel: true, //开启鼠标滚轮
      // disableMouse: false, //启用鼠标拖动
      // disableTouch: false //启用手指触摸
    });

    //监听滚动的位置
    if (this.probeType === 2 || this.probeType === 3) {
      this.scroll.on("scroll", position => {
        this.$emit("backTopScroll", position);
      });
    }

    // 监听scroll滚动到底部
    if (this.pullUpLoad) {
      this.scroll.on("pullingUp", () => {
        // console.log("滚动到底部了");
        this.$emit("pullingUp");
      });
    }
  },
  methods: {
    // 第一个参数x轴的距离,第二个参数y轴的距离,第三个参数是延迟时间
    scrollTo(x, y, time = 500) {
      this.scroll && this.scroll.scrollTo(x, y, time);
    },
    // 并且是为了保证this.scroll存在才执行
    refresh() {
      this.scroll && this.scroll.refresh();
    },
    finishPullUp() {
      this.scroll && this.scroll.finishPullUp();
    },
    // 获取滚动的纵向距离
    getScrollY() {
      return this.scroll ? this.scroll.y : 0;
    },
    // 滚动到指定元素
    scrollToElement(el, time) {
      this.scroll.scrollToElement(el, time);
    }
  }
};
</script>
