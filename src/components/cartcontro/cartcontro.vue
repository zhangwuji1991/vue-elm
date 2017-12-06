<template>
  <div class="cartcontro">
   <div class="cart_decrease icon-remove_circle_outline" v-show="food.count>0" @click="decreaseCart"></div>
   <div class="cart_count" v-show="food.count>0">{{food.count}}</div>
   <div class="cart_add" @click="addCart"></div>
  </div>
</template>

<script  type="text/ecmascript-6">
  import Vue from 'vue'

  export default{
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart (event) {
        if (!event._constructed) {
          return
        }
        console.log(1)
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
      },
      decreaseCart (event) {
        if (!event._constructed) {
          return
        }
        if (this.food.count) {
          this.food.count--
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.cartcontro{
	.cart_decrease{
		display: inline-block;
		font-size: 24px;
		line-height: 24px;
		width: 30px;
		height: 30px;
		background: url(offline.png);
		background-size: 100% 100%;
	}
	.cart_count{
		display: inline-block;
		font-size: 10px;
		vertical-align: top;
		width: 12px;
		padding-top: 6px;
		text-align: center;
		line-height: 24px;
		color: red;
	}
	.cart_add{
		display: inline-block;
		font-size: 24px;
		line-height: 24px;
		width: 30px;
		height: 30px;
		background: url(addition.png);
		background-size: 100% 100%;
	}
	
	
}
</style>