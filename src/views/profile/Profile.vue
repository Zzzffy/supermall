<template>
  <div>
    <scroll class="content">
      <ul>
        <li>列表1</li>
        <li>列表2</li>
        <li>列表3</li>
        <li>列表4</li>
        <li>列表5</li>
        <li>列表6</li>
        <li>列表7</li>
        <li>列表8</li>
        <li>列表9</li>
        <li>列表10</li>
      </ul>
    </scroll>
  </div>
</template>

<script>
import Scroll from 'components/common/scroll/Scroll'
export default {
  name: 'Profile',
  components: {
    Scroll
  }
}
</script>

<style scoped>
.content {
  height: 150px;
  background-color: pink;
}

</style>