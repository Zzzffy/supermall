<template>
  <div>
    <tab-bar>
      <tab-bar-item path="/home" activeColor="red">
      <!-- 由于我们在vue.config.js里面给assets起了别名 在dom中使用别名路径前面需要加上~ -->
        <img slot="item-icon" src="~assets/img/tabbar/home.svg" alt />
        <img slot="item-active" src="~assets/img/tabbar/home_active.svg" alt />
        <div slot="item-text">首页</div>
        <!-- 具名插槽需要添加 slot 与 name对应 -->
      </tab-bar-item>
      <tab-bar-item path="/category" activeColor="red">
        <img slot="item-icon" src="~assets/img/tabbar/category.svg" alt />
        <img slot="item-active" src="~assets/img/tabbar/category_active.svg" alt />
        <div slot="item-text">分类</div>
      </tab-bar-item>
      <tab-bar-item path="/cart" activeColor="red">
        <img slot="item-icon" src="~assets/img/tabbar/shopcart.svg" alt />
        <img slot="item-active" src="~assets/img/tabbar/shopcart_active.svg" alt />
        <div slot="item-text">购物车</div>
      </tab-bar-item>
      <tab-bar-item path="profile">
        <img slot="item-icon" src="~assets/img/tabbar/profile.svg" alt />
        <img slot="item-active" src="~assets/img/tabbar/profile_active.svg" alt />
        <div slot="item-text">我的</div>
      </tab-bar-item>
    </tab-bar>
  </div>
</template>

<script>
// 我们在base.js文件给components也起了别名 但是在import中不需要在它前面加上~符号
import TabBar from "components/common/tabbar/TabBar"; // 导入组件
import TabBarItem from "components/common/tabbar/TabBarItem";

export default {
  name: "MainTabBar",
  components: {
    TabBar,
    TabBarItem
  }
};
</script>

<style scoped>
</style>