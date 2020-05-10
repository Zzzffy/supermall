<template>
  <div class="recommend">
    <div v-for="item in recommends" :key="item.index" class="recommend-item">
      <a :href="item.link">
        <img :src="item.image" alt="">
        <div>{{item.title}}</div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomeRecommend",
  props: {
    recommends: {
      Type: Array,
      default() {
        return []
      }
    }
  }
}
</script>

<style scoped>
.recommend {
  display: flex;
  text-align: center;
  margin-top: 10px;
}
.recommend-item {
  flex: 1
}
.recommend-item img {
  width: 65px;
}
.recommend-item div {
  font-size: 12px;
  margin-top: 5px;
  padding-bottom: 15px;
  border-bottom: 10px solid #eee;
}

</style>