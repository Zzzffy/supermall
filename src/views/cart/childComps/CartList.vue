<template>
  <div class="cart-list">
    <scroll class="content">
      <cart-list-item v-for="(item, index) in cartList" :key="index" :item-info="item" />
    </scroll>   
  </div>
</template>

<script>
import {mapGetters} from 'vuex'
import Scroll from 'components/common/scroll/Scroll'
import CartListItem from './CartListItem'

export default {
  name: "CartList",
  components: {
    Scroll,
    CartListItem
  },
  computed: {
    ...mapGetters(['cartList'])
  }
}
</script>

<style scoped>

.content {
  height: calc(100vh - 44px - 49px - 40px);
}

</style>