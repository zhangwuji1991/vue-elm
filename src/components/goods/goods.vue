<template>
  <div class="goods">
   <div class="menu_wrapper" ref="menu-Wrapper">
   	<ul>
   		<li v-for="(item,$index) in goods" class="menu_item" :class="{'current':currentIndex==$index}" @click="selectMenu($index,$event)">
   			<span class="text">
   				<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
   				{{item.name}}
   			</span>
   		</li>
   	</ul>
   </div>
   <div class="foods_wrapper"  ref="food-Wrapper">
   	<ul>
   		<li v-for="item in goods" class="food_list food-list-hock">
   			<h1 class="title">{{item.name}}</h1>
   			<ul>
   				<li v-for="food in item.foods" class="food_item">
   					<div class="icon">
   						<img width="57" height="57" :src="food.icon">
   					</div>
   					<div class="content">
   						<h2 class="name">{{food.name}}</h2>
   						<p class="desc">{{food.description}}</p>
   					</div>
   					<div class="extra">
   						<span class="count">月售{{food.sellCount}}</span>
   						<span>好评率{{food.rating}}%</span>
   					</div>
   					<div class="price">
   						<span class="now">￥{{food.price}}</span>
   						<span class="old"v-show="food.oldPrice"￥{{food.price}}</span>
   					</div>
            <div class="cartcontro_wrapper">
              <Cartcontro :food="food"></Cartcontro>
            </div>
   				</li>
   			</ul>
   		</li>
   	</ul>
   </div>
   <shopcart  :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice" :select-foods="selectFoods"></shopcart>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  import shopcart from '../shopcart/shopcart'
  import Cartcontro from '../cartcontro/cartcontro'
  
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0
      }
    },
    computed: {
      currentIndex () {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]
          let height2 = this.listHeight[i + 1]
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i
          }
        }
        return 0
      },
      selectFoods () {
        let foods = []
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food)
            }
          })
        })
        return foods
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
      this.$http.get('/api/goods')
        .then(response => {
          response = response.data
          if (response.errno === 0) {
            this.goods = response.data
            this.$nextTick(() => {
              this._initScroll()
              this._calculateHeight()
            })
          }
        })
    },
    components: {
      shopcart,
      Cartcontro
    },
    methods: {
      selectMenu (index, event) {
        if (!event._constructed) {
          return
        }
        let foodList = this.$refs['food-Wrapper'].getElementsByClassName('food-list-hock')
        let el = foodList[index]
        this.foodScroll.scrollToElement(el, 300)
      },
      _initScroll () {
        this.menuScroll = new BScroll(this.$refs['menu-Wrapper'], {
          click: true
        })
        this.foodScroll = new BScroll(this.$refs['food-Wrapper'], {
          click: true,
          probeType: 3
        })
        this.foodScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      _calculateHeight () {
        let foodList = this.$refs['food-Wrapper'].getElementsByClassName('food-list-hock')
        let height = 0
        this.listHeight.push(height)
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i]
          height += item.clientHeight
          this.listHeight.push(height)
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss"">
  .goods{
  	position: absolute;
  	top: 174px;
  	width:100%;
  	bottom: 46px;
  	display: flex;
  	overflow: hidden;
  	.menu_wrapper{
  		flex: 0 0 80px;
  		width: 80px;
  		background:#f3f5f7;
      .current{
          position: relative;
          z-index: 10;
          margin-top: -1px;
          font-weight: 700;
          background: #fff;
          .text{
            border-bottom: 1px solid red;
          }

        }
  		.menu_item{
  			display: table;;
  			height: 54px;
  			width: 56px;
  			line-height: 14px;
  			padding: 0 12px;
        
  			.icon{
  				display: inline-block;
				width: 12px;
				height: 12px;
				vertical-align: top;
				margin-right: 2px;
				background-size: 12px 12px;
				background-repeat: no-repeat;
  			}
  			.decrease{
	            background-image:url(decrease_3@2x.png);	
	        }
	        .guarantee{
	            background-image:url(guarantee_3@2x.png);	
	        }
	        .discount{
	            background-image:url(discount_3@2x.png);	
	        }
	        .invoice{
	            background-image:url(invoice_3@2x.png);	
	        }
	        .special{
	        	background-image:url(special_3@2x.png);	
	        }
	        .text{
	        	font-size: 12px;
	        	display: table-cell;
	        	width: 56px;
	        	vertical-align: middle;
	        	border-bottom: 1px solid rgba(7,17,27,0.2)
	        }
  		}
  	}
  	.foods_wrapper{
  		flex: 1;
  		.title{
  			padding-left: 14px;
  			height: 26px;
  			line-height: 26px;
  			border-left: 2px solid #d9dde1;
  			font-size: 12px;
  			color: rgb(147,153,159);
  			background: #f3f5f7;
  		}
  		.food_item{
  			/*display: flex;*/
        position: relative;
  			margin: 18px;
  			padding-bottom: 18px; 
  			border-top: 1px solid rgba(7,17,27,0.1)
  		}
  		.icon{
  			float: left;
  			margin-right: 10px;
  		}
  		.content{
  			flex: 1;
  		}
  		.name{
  			margin: 2px 0 8px 0;
  			height: 14px;
  			line-height: 14px;
  			font-size: 14px;
  			color: rgb(7,17,27);
  		}
  		.desc{
  			margin-bottom: 8px;
  			line-height: 12px;
  			font-size: 10px;
  			color: rgb(147,153,159);
  		}
  		.extra{
  			line-height: 10px;
  			color: rgb(147,153,159);
  			font-size: 10px;
  			.count{
  				margin-right: 12px;
  			}
  		}
  		.price{
  			font-weight: 700;
  			line-height: 24px;
  			.now{
  				margin-right: 18px;
  				font-size: 14px;
  				color: rgb(240,20,20);
  			}
  			.old{
  				text-decoration: line-through;
  				font-size: 10px;
  				color: rgb(147,153,159);
  			}
  		}
      .cartcontro_wrapper{
          position: absolute;
          right: 0;
          bottom: 12px;
      }
  	}
  }
</style>