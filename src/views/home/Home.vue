<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">星奇</div></nav-bar>
    <scroll class="scroll"  ref="scroll">
      <home-swiper :banners="banners" />
      <recommend-view :recommends="recommends" />
      <feature-view />
      <tab-control
        class="tab-control"
        :titles="['流行', '新款', '精选']"
        @tabClick="tabClick"
      />
      <goods-list :goods="showGoods" ></goods-list>
    </scroll>

    <back-top @click.native="backClick" />
  </div>
</template>

<script>
import { getHomeMultidata } from "network/home";
import { getHomeData } from "network/home";

import HomeSwiper from "./ChildComponets/HomeSwiper";
import RecommendView from "./ChildComponets/RecommendView";
import FeatureView from "./ChildComponets/FeatureView";

import NavBar from "components/common/navbar/NavBar";
import TabControl from "components/content/TabControl/TabControl";
import GoodsList from "components/content/goods/GoodsList";
import GoodsListItem from "components/content/goods/GoodsListItem";
import BackTop from "../../components/content/backTop/BackTop.vue";
import Scroll from "../../components/content/scroll/Scroll";

export default {
  name: "Home",
  components: {
    NavBar,
    TabControl,
    HomeSwiper,
    RecommendView,
    FeatureView,
    GoodsList,
    GoodsListItem,
    BackTop,
    Scroll,
  },
  data() {
    return {
      currentType: "pop",
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] },
      },
    };
  },
  created() {
    //请求多个数据
    this.getHomeMultidata();
    //请求商品数据
    this.getHomeData("pop");
    this.getHomeData("new");
    this.getHomeData("sell");
  },
  methods: {
    // 事件监听方法
    tabClick(index) {
      switch (index) {
        case 0:
          this.currentType = "pop";
          break;
        case 1:
          this.currentType = "new";
          break;
        case 2:
          this.currentType = "sell";
          break;
      }
    },

    backClick(){
      this.$refs.scroll.scrollTo(0,500)
    },

    //  网络请求方法
    //
    getHomeMultidata() {
      getHomeMultidata().then((res) => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeData(type) {
      const page = this.goods[type].page + 1;
      getHomeData(type, page).then((res) => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
      });
    },
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list;
    },
  },
};
</script>

<style scoped>
#home {
  height: 100vh;

  position: relative;
}

.home-nav {
  background-color: var(--color-tint);
  color: #fff;

  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9;
}

.scroll {
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
  /* 让鼠标滚轮不能滚 */
  overflow: hidden;
}

/* .tab-control {
} */
</style>