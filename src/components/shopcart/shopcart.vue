<template>
	<div class="shopcart">
		<div class="content">
			<div class="content_left">
			    <div class="logo_wrapper">
				   <div class="logo" :class="{'highlight':totalCount>0}">				
				   </div>
				   <div class="num" v-show="totalCount>0">{{totalCount}}</div>
			    </div>
			   <div class="price" :class="{'highlight':totalPrice>0}">{{totalPrice}}</div>
			   <div class="desc">另需配送费￥{{deliveryPrice}}</div>
		    </div>
		    <div class="content_right">
		       <div class="pay" :class="payClass">
		       	{{payDesc}}
		       </div>
		   </div>
		</div>
		<div class="shopcart_list">
		  <div class="list_header">
		  	<h1 class="title">购物车</h1>
		  	<span class="empty">清空</span>
		  </div>
		  <div class="list_content">
		  	<ul>
		  		<li class="food" v-for="food in selectFoods">
		  			<span class="name">{{food.name}}</span>
		  			<div class="price">
		  				<span>￥{{food.price*food.count}}</span>
		  			</div>
		  			<div class="cartcontrol_wrapper">
		  				<Cartcontro></Cartcontro>
		  			</div>	
		  		</li>
		  	</ul>
		  </div>

		</div>

	</div>
</template>
<script type="text/ecmascript-6">
 import Cartcontro from '../cartcontro/cartcontro'

 export default {
   props: {
     selectFoods: {
       type: Array,
       default () {
         return [
           {
             price: 0,
             count: 0
           }
         ]
       }
     },
     deliveryPrice: {
       type: Number,
       default: 0
     },
     minPrice: {
       type: Number,
       default: 0
     }
   },
   computed: {
     totalPrice () {
       let total = 0
       this.selectFoods.forEach((food) => {
         total += food.price * food.count
       })
       return total
     },
     totalCount () {
       let count = 0
       this.selectFoods.forEach((food) => {
         count += food.count
       })
       return count
     },
     payDesc () {
       if (this.totalPrice === 0) {
         return `￥${this.minPrice}元起送`
       } else if (this.totalPrice < this.minPrice) {
         let diff = this.minPrice - this.totalPrice
         return `还差￥${diff}元起送`
       } else {
         return '去计算'
       }
     },
     payClass () {
       if (this.totalPrice < this.minPrice) {
         return 'not_enough'
       } else {
         return 'enough'
       }
     }
   },
   components: {
     Cartcontro
   }
 }
</script>>
<style lang="scss">
	.shopcart{
		position: fixed;
		left: 0;
		bottom: 0;
		z-index: 50;
		width: 100%;
		height: 48px;
		color: rgba(255,255,255,0.4);	
		.content{
			display: flex;
			background: #141d27;
			font-size: 0;
			.content_left{
				flex: 1;
				.logo_wrapper{
					display: inline-block;
					position: relative;
					top: -10px;
					margin: 0 12px;
					padding: 6px;
					width: 56px;
					height: 56px;
					box-sizing: border-box;
					vertical-align: top;
					border-radius: 50%;
					background: #141d27;
					.logo{
						width: 100%;
						height: 100%;
						border-radius: 50%;
						background: #2b343c;
						background: url(gwch.png);
						background-size: 100% 100%;
						background-repeat: no-repeat;
					}
					.highlight{
						background: url(gwcl.png);
						background-size: 100% 100%;
						background-repeat: no-repeat;
					}
					.num{
						position: absolute;
						top: 0;
						right: 0;
						width: 24px;
						height: 16px;
						text-align: center;
						font-size: 9px;
						font-weight: 700;
						background: red;
						color: #fff;
						border-radius: 16px;
						box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
					}
				}
				.price{
					display: inline-block;
					vertical-align: top;
					line-height: 24px;
					margin-top: 12px;
					box-sizing: border-box;
					border-bottom: 1px solid rgba(255,255,255,0.1);
					font-size:16px;
					font-weight: 700;
					color: rgba(255,255,255,0.4);

				}
				.highlight{
						color: white;
					}
				.desc{
					display: inline-block;
					vertical-align: top;
					line-height: 24px;
					margin: 12px 0 0 12px;
					font-size: 10px;
					
				}
			}
			.content_right{
				width: 105px;
				flex: 0 0 105px;
				.pay{
					height: 48px;
					line-height: 48px;
					text-align: center;
					font-size: 12px;
					background: #2b333b;
				}
				.not_enough{
					background: #2b333b;
				}
				.enough{
					background: #00b43c;
					color: #fff;
				}
			}
		}
	}
</style>