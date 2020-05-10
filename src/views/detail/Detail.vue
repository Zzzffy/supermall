<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"></detail-nav-bar>

    <scroll class="content" ref="scroll" @scroll="contentScroll" :probe-type="3">
      <!-- <ul>
        <li v-for="item in $store.state.cartList" :key="item.index">
          {{item}}
        </li>
      </ul> -->
      <detail-swiper :top-images="topImages"></detail-swiper>
      <detail-base-info :goods="goods"></detail-base-info>
      <detail-shop-info :shop="shop"></detail-shop-info>
      <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"></detail-goods-info>
      <detail-param-info :paramInfo="GoodsParam" ref="param"></detail-param-info>
      <detail-comment-info :commentInfo="commentInfo" ref="comment"></detail-comment-info>
      <goods-list :goods="recommends" ref="recommend"></goods-list>
    </scroll>

    <detail-bottom-bar @addToCart="addToCart"></detail-bottom-bar>
    
    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>

    <toast :message="message" :show="show"></toast>

  </div>
</template>

<script>
import DetailNavBar from "./childComps/DetailNavBar";
import {
  getDetail,
  Goods,
  Shop,
  GoodsParam,
  getRecommend
} from "network/detail";
import DetailSwiper from "./childComps/DetailSwiper";
import DetailBaseInfo from "./childComps/DetailBaseInfo";
import DetailShopInfo from "./childComps/DetailShopInfo";
import Scroll from "components/common/scroll/Scroll";
import DetailGoodsInfo from "./childComps/DetailGoodsInfo";
import DetailParamInfo from "./childComps/DetailParamInfo";
import DetailCommentInfo from "./childComps/DetailCommentInfo";
import GoodsList from "components/content/goods/GoodsList";
import DetailBottomBar from './childComps/DetailBottomBar';
import BackTop from "components/content/backTop/BackTop";
import { mapActions } from 'vuex'
import Toast from 'components/common/toast/Toast'


export default {
  name: "Detail",
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    Scroll,
    DetailGoodsInfo,
    DetailParamInfo,
    DetailCommentInfo,
    GoodsList,
    DetailBottomBar,
    BackTop,
    Toast
  },
  data() {
    return {
      iid: null,
      topImages: [],
      goods: {},
      shop: {},
      detailInfo: {},
      GoodsParam: {},
      commentInfo: {},
      recommends: [],
      themeTopYs: [],
      currentIndex: 0,
      isShowBackTop: false,
      message: '',
      show: false
    };
  },
  created() {
    this.iid = this.$route.params.iid;
    // console.log(this.iid);
    getDetail(this.iid).then(res => {
      // 获取顶部的图片轮播数据
      // console.log(res);
      const data = res.result;
      this.topImages = data.itemInfo.topImages;
      // 获取商品信息
      this.goods = new Goods(
        data.itemInfo,
        data.columns,
        data.shopInfo.services
      );
      
      // 获取商家信息
      this.shop = new Shop(data.shopInfo);
      // 获取商品详细信息
      this.detailInfo = data.detailInfo;
      // 获取参数的信息
      this.GoodsParam = new GoodsParam(
        data.itemParams.info,
        data.itemParams.rule
      );
      // 获取评论信息
      if (data.rate.cRate !== 0) {
        this.commentInfo = data.rate.list[0];
      }
    }),
      getRecommend().then(res => {
        // console.log(res);
        this.recommends = res.data.list;
      });
  },
  methods: {
    ...mapActions(['addCart']),
    imageLoad() {
      this.$refs.scroll.refresh();

      this.themeTopYs = [];
      this.themeTopYs.push(0);
      this.themeTopYs.push(this.$refs.param.$el.offsetTop);
      this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
      this.themeTopYs.push(this.$refs.recommend.$el.offsetTop);
      this.themeTopYs.push(Number.MAX_VALUE) // 增加一个无穷大元素
      // console.log(this.themeTopYs);
    },
    titleClick(index) {
      // console.log(index);
      this.$refs.scroll.scrollTo(0, -this.themeTopYs[index] + 44, 500);
    },
    contentScroll(position) {
      const positionY = -position.y
      let length = this.themeTopYs.length
      for(let i = 0; i < length - 1; i++) {
        if (this.currentIndex !== i && (positionY >= this.themeTopYs[i] - 44  && positionY < this.themeTopYs[i+1] - 44)) {
          this.currentIndex = i;
          this.$refs.nav.currentIndex = this.currentIndex
        }       
      }
      // 是否显示回到顶部
      this.isShowBackTop = (-position.y) > 1000
    },
    backClick() {
      this.$refs.scroll.scrollTo(0, 0, 200)   
    }, 
    addToCart() {
      // 获取购物车界面商品展示所需要的数据
      const product = {}
      product.img = this.topImages[0]
      product.title = this.goods.title
      product.desc = this.goods.desc
      product.price = this.goods.realPrice
      product.iid = this.iid
      // 将商品添加到购物车
      // this.$store.commit('addCart', product) commit对应 mutations
      // this.$store.dispatch('addCart', product).then(res => {
      //   console.log(res);
      // })
      // 添加映射关系 导入 mapActions
      this.addCart(product).then(res => {
        this.message = res
        this.show = true

        setTimeout(() => {
          this.message = ''
          this.show = false
        }, 1500)
      })
    }  
  }
};
</script>

<style scoped>
.content {
  height: calc(100% - 44px - 49px);
}
#detail {
  height: 100vh;
}
.detail-nav {
  position: relative;
  z-index: 999;
  background-color: #fff;
}
</style>