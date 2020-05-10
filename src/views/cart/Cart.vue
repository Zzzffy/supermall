<template>
  <div class="cart">
    <!-- 顶部导航部分 -->
    <nav-bar class="nav-bar">
      <div slot="center">购物车({{length}})</div>
    </nav-bar>

    <!-- 商品展示部分 -->
    <cart-list></cart-list>

    <!-- 底部汇总 -->
    <cart-bottom-bar></cart-bottom-bar>
  </div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'
import {mapGetters} from 'vuex'
import CartList from './childComps/CartList'
import CartBottomBar from './childComps/CartBottomBar'

export default {
  name: 'Cart',
  components: {
    NavBar,
    CartList,
    CartBottomBar
  },
  computed: {
    // ...mapGetters([
    //   'cartLength',
    //   'cartList'
    // ])
    // 如果想修改 getters中的函数名就传入对象
    ...mapGetters({
      length: 'cartLength',
      // list: 'cartList'
    })
  }
}
</script>

<style scoped>
.nav-bar {
  background-color: var(--color-tint);
  color: #fff;
}
.cart {
  overflow-wrap: break-word
}

</style>