<template>
  <div class="bottom-bar">
    <div class="checkcontent">
      <check-button class="checkbtn" :is-checked="isSelectAll" @click.native="checkClick"></check-button>
      <span>全选</span>
    </div>

    <div class="price">
      合计:{{totalPrice}}
    </div>

    <div class="calculate">
      去计算({{checkLength}})
    </div>
  </div>
</template>

<script>
import CheckButton from 'components/content/checkButton/CheckButton'
export default {
  name: "CartBottomBar",
  components: {
    CheckButton
  },
  computed: {
    totalPrice() {
      return '￥' + this.$store.state.cartList.filter(item => {
        return item.checked
      }).reduce((preValue, item) => {
        return preValue + item.price * item.count
      }, 0).toFixed(2)
    },
    checkLength() {
      return (this.$store.state.cartList.filter(item => item.checked).length)      
    },
    isSelectAll() {
      if (this.$store.state.cartList.length === 0) return false
      // return !(this.$store.state.cartList.filter(item => !item.checked).length)     
      return !(this.$store.state.cartList.find(item => !item.checked))     
    }
  },
  methods: {
    checkClick() {
      if (this.isSelectAll) {
        this.$store.state.cartList.forEach(item => item.checked = false);
      } else {
        this.$store.state.cartList.forEach(item => item.checked = true);
      }
    }
  }
}
</script>

<style scoped>
.bottom-bar {
  height: 40px;
  background-color: #eee;
  position: relative;
  line-height: 40px;
  display: flex;
}
.checkcontent {
  display: flex;
  align-items: center;
  margin-left: 10px;
  width: 60px;
}
.checkbtn {
  width: 20px;
  height: 20px;
  line-height: 20px;
  margin-right: 5px;
}
.price {
  flex: 1;
  margin-left: 45px;
}
.calculate {
  width: 80px;
}


</style>