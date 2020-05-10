<template>
  <div class="tab-bar">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "TabBar"
};
</script>

// scoped表示每个style都只在当前组件有用 不会对其他组件产生影响
<style scoped> 
.tab-bar {
  display: flex;
  background-color: #f6f6f6;

  position: fixed;
  left: 0;
  right: 0;
  /* 上面两行相当于 width: 100% */
  bottom: 0;
}
</style>