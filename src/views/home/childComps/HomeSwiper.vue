<template>
  <div>
    <Swiper>
      <swiper-item v-for="item in banners" :key="item.index">
        <a :href="item.link">
          <img :src="item.image" alt="" @load="imageLoad">
        </a>
      </swiper-item>
    </Swiper>
  </div>
</template>

<script>
import {Swiper, SwiperItem} from 'components/common/swiper'
export default {
  name: "HomeSwiper",
  components: {
    Swiper,
    SwiperItem
  },
  props: {
    banners: {
      Type: Array,
      default() {
        return []
      }
    }
  },
  data() {
    return {
      isLoad: false
    }
  },
  methods: {
    imageLoad() {
      if (!this.isLoad) {  // 减少次数 提高性能
        this.$emit('swiperImageLoad')
        this.isLoad = true
      }       
    }
  }

}
</script>

<style scoped>

</style>