<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import BScroll from "better-scroll";

export default {
  name: "Scroll",
  data() {
    return {
      scroll: null,
      scrollHandler: null,
    };
  },
  mounted() {
    this.scroll = new BScroll(this.$refs.wrapper, {
      probeType: 3,
      click: true,
      pullUpLoad: true,
      // 让滚动时不模糊
      useTransition: false,
    });
    this.scrollHandler = (position) => {
      this.$emit("scroll", position);
    };
    this.scroll.on("scroll",position => {
      this.$emit('scroll',position);
    })
    this.scroll.on("pullingUp", () => {
      this.$emit("pullingUp");
    });
  },
  methods: {
    scrollTo(position, delay) {
      this.scroll.scrollTo(position.x || 0, position.y || 0, delay);
    },
    refresh(param) {
      console.log(param);
      this.scroll.refresh();
    },
    finishPullUp(){
      this.scroll.finishPullUp();
    },
    onRefresh() {
      this.scroll.on("refresh", () => {
        this.$emit("refresh");
      });
    },
  },
};
</script>

<style scoped>
</style>