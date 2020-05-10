<template>
  <div class="tab-bar-item" @click="itemClick">
    <!-- 由于插槽会被替换 如果把v-if之类的属性写到插槽 可能不会生效 所以我们最好加个div -->
    <div v-if="!isActive"><slot name="item-icon"></slot></div>
    <div v-else><slot name="item-active"></slot></div>   
    <!-- <div :class="{active: isActive}"><slot name="item-text"></slot></div>   -->
    <div :style="activeStyle"><slot name="item-text"></slot></div>
  </div>
</template>

<script>
export default {
  name: 'TabBarItem',
  props: {
    path: String,
    activeColor: {
      type: String,
      default: 'purple'  // 当item里面没有设置activeColor的时候默认显示紫色
    }
  },
  data() {
    return {
      // isActive: true
    }
  },
  computed: {
    isActive() {
      return this.$route.path.indexOf(this.path) !== -1
    },
    activeStyle() {
      return this.isActive ? {color: this.activeColor} : {}
    }
  },
  methods: {
    itemClick() {
      return this.$router.push(this.path)
    }
  }
}
</script>

<style scoped>
.tab-bar-item {
  flex: 1;
  text-align: center;
  height: 49px;
  font-size: 14px;
  
}
.tab-bar-item img {
  width: 24px;
  margin-top: 3px;
  vertical-align: middle; /* 可以消除图片默认的下边距3px middle也可以改为top*/
  margin-bottom: 2px;
}
.active {
  color: red;
}
</style>