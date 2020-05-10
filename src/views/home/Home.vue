<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>

    <tab-control 
        :tabcontrols="tabcontrols" 
        class="tabcontrol" 
        @tabClick="tabClick" 
        ref="tabControl1"
        v-show="isFixed"/>

    <Scroll class="content" 
            ref="scroll" 
            :probe-type="3" 
            @scroll="contentScroll"
            :pull-up-load="true"
            @pullingUp="loadMore">
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"></home-swiper>
      <home-recommend :recommends="recommends"></home-recommend>
      <Features></Features>
      <tab-control 
        :tabcontrols="tabcontrols" 
        class="tabcontrol" 
        @tabClick="tabClick" 
        ref="tabControl2"
        :class="{fixed: isFixed}"/>
      <goods-list :goods="showGoods"></goods-list>
    </Scroll>

    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
  </div>
</template>

<script>
import HomeSwiper from "./childComps/HomeSwiper";
import HomeRecommend from "./childComps/HomeRecommend";
import Features from "./childComps/Features";

import NavBar from "components/common/navbar/NavBar";
import TabControl from "components/content/tabControl/TabControl";
import GoodsList from "components/content/goods/GoodsList";
import GoodsListItem from "components/content/goods/GoodsListItem";
import Scroll from "components/common/scroll/Scroll";
import BackTop from "components/content/backTop/BackTop"

import { getHomeMultidata, getHomeGoods } from "network/home.js";
import { debounce } from "common/utils.js"
// import {backTopMixin} from 'common/mixin.js'

export default {
  name: "Home",
  components: {
    HomeSwiper,
    HomeRecommend,
    Features,
    NavBar,
    TabControl,
    GoodsList,
    GoodsListItem,
    Scroll,
    BackTop
  },
  data() {
    return {
      banners: [],
      recommends: [],
      tabcontrols: ["流行", "新款", "精选"],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] }
      },
      currentType: "pop",
      isShowBackTop: false,
      tabOffsetTop: 0,
      isFixed: false,
      saveY: 0
    };
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list;
    }
  },
  destroyed() {
    console.log('home destroyed');   
  },
  activated() {
    this.$refs.scroll.scrollTo(0, this.saveY, 0)
    this.$refs.scroll.refresh( )
  },
  deactivated() {
    this.saveY = this.$refs.scroll.scroll.y
  },
  created() {
    // 请求多个数据
    this.getHomeMultidata();
    // 请求商品数据
    this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");
  },
  mounted() {
    // this.$bus.$on('itemImageLoad', () => {
    //   this.$refs.scroll.refresh()
    // })
    const refresh = debounce(this.$refs.scroll.refresh, 50)
    this.$bus.$on('itemImageLoad', () => {
      refresh()
    })
  },
  methods: {   
    // 网络请求相关的方法
    getHomeMultidata() {
      getHomeMultidata().then(res => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
        
        this.$refs.scroll.finishPullUp()
      })     
    },
    // 事件监听相关的方法
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
      this.$refs.tabControl1.currentIndex = index;
      this.$refs.tabControl2.currentIndex = index;
    },
    backClick() {
      this.$refs.scroll.scrollTo(0, 0, 200)   
    },
    contentScroll(position) {
      this.isShowBackTop = (-position.y) > 1000 
      this.isFixed = (-position.y) > this.tabOffsetTop
    },
    loadMore() {
      // console.log('上拉加载更多');
      this.getHomeGoods(this.currentType)     
    },
    swiperImageLoad() {
      this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop;
      // console.log(this.$refs.tabControl.$el.offsetTop);      
    }
  }
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
  /* 在使用浏览器原生滚动时，为了让导航不跟随一起滚动 */
  /* position: fixed;
  left: 0;
  top: 0;
  right: 0;
  z-index: 999; */
}
.tabcontrol {
  position: sticky;
  top: 44px;
}
.content {
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
  overflow: hidden;
}
.fixed {
  position: fixed;
  left: 0;
  right: 0;
  top: 44px;
}
</style>