<template>
  <div class="header">
   <div class="content_wrapper">
   	<div class="avatar">
   		<img :src="seller.avatar" width="64">
   	</div>
   	<div class="content">
   		<div class="title">
   			<span class="brand"></span>
   			<span class="name">{{seller.name}}</span>
   		</div>
   		<div class="description">
   			{{seller.description}}/{{seller.deliveryTime}}分钟送达
   		</div>
   		<div v-if="seller.supports" class="support">
   			<span class="icon" :class="classMap[seller.supports[0].type]"></span>
   			<span class="text">{{seller.supports[0].description}}</span>
   		</div>
   	</div>
   	<div v-if="seller.supports" class="support_count" @click="showDetail">
   		<span class="count">{{seller.supports.length}}个&nbsp></span>
   	</div>
   </div>
   <div class="bulletin_wrapper" @click="showDetail">
   	<span class="bulletin_title"></span><span class="bulletin_text">{{seller.bulletin}}</span>
   	<span class="bulletin_jt">></span>
   </div>
   <div class="backgrounds">
   	<img :src="seller.avatar" width="100%" height="100%">
   </div>
   <div class="detail" v-show="detailShow">
   	<div class="detail_wrapper clearfix">
   		<div class="detail_main">
   			<h1 class="name">{{seller.name}}</h1>
   			<div class="star_wrapper">
   			<star :size="48" :score="seller.score"></star>
   			</div>
   			<div class="title">
   				<div class="line"></div>
   				<div class="text">优惠信息</div>
   				<div class="line"></div>
   			</div>
   			<ul v-if="seller.supports" class="supports">
   				<li class="supports_item" v-for="(item,index) in seller.supports">
   					<span class="icon" :class="classMap[seller.supports[index].type]"></span>
   					<span class="text">{{seller.supports[index].description}}</span>
   				</li>
   			</ul>
   			<div class="title">
   				<div class="line"></div>
   				<div class="text">商家公告</div>
   				<div class="line"></div>
   			</div>
   			<div class="bulletin">
   				<p class="content">{{seller.bulletin}}</p>
   			</div>
   		</div>
   	</div>
   	<div class="detail_close">
   		<span class="detail_x" @click="closeDetail">x</span>
   	</div>
   </div>
  </div>
</template>

<script>
  import star from '../star/star'
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        detailShow: false
      }
    },
    methods: {
      showDetail () {
        this.detailShow = true
      },
      closeDetail () {
        this.detailShow = false
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
    },
    components: {
      star
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss"">
	.header{
		color:#fff;
		background: rgba(7,17,27,0.5);
		position: relative;
		overflow: hidden;
		.content_wrapper{
			padding: 24px 12px 18px 24px;
			position: relative;
			font-size: 0px;
			.avatar{
				display: inline-block;
				vertical-align: top;
				img{
					border-radius: 2px;
				}
			}	
			.content{
				display: inline-block;
				font-size: 14px;
				margin-left: 16px;
				.title{
					margin:2px 0 8px  0;
					.brand{
						width: 30px;
						height: 18px;
						display: inline-block;
						vertical-align: top;
						background-image: url(brand@2x.png);
						background-size: 30px 18px;
						background-repeat: no-repeat;
					}	
					.name{
						margin-left: 6px;
						font-size: 16px;
						line-height: 18px;
						font-weight: bold;
					}
				}	
				.description{
					margin-bottom: 10px;
					font-size: 12px;
					line-height: 12px;
				}	
				.support{
					.icon{
						display: inline-block;
						width: 12px;
						height: 12px;
						margin-right: 4px;
						background-size: 12px 12px;
						background-repeat: no-repeat;
						vertical-align: top;
					}					
					.decrease{
					    background-image:url(decrease_1@2x.png);	
					}
					.text{
						line-height: 12px;		
						font-size: 10px;
						vertical-align: top;
					}		
					
				}	
			}		
			.support_count{
				position: absolute;
				right:12px;
				bottom: 18px;
				padding: 0 8px;
				height: 24px;
				line-height: 24px;
				border-radius: 14px;
				text-align: center;
				background: rgba(0,0,0,0.2);
				.count{
					font-size: 10px;
					vertical-align: top;
				}
			}
		}
		.bulletin_wrapper{
			position: relative;
			height: 28px;
			line-height: 28px;
			padding: 0 22px 0 12px;
			white-space: nowrap;
			text-overflow: ellipsis;
			overflow: hidden;
			background: rgba(7,17,27,0.2);
			.bulletin_title{
				display: inline-block;
				width: 22px;
				height: 12px;
				background-image: url(bulletin@2x.png);
				background-size: 22px 12px;
				vertical-align: top;
				margin-top: 8px;
			}
			.bulletin_text{
				font-size: 10px;
				margin:0 10px;
				vertical-align: top;
			}
			.bulletin_jt{
				position: absolute;
				font-size: 10px;
				right: 12px;
				top: 1px;
				vertical-align: top;
			}
		}
		.backgrounds{
			position: absolute;
			top: 0;
			left: 0;
			width: 100%;
			height: 100%;
			z-index: -1;
			filter: blur(10px);
		}
		.detail{
			position: fixed;
			z-index: 100;
			top: 0;
			left: 0;
			width: 100%;
			height: 100%;
			overflow: auto;
			background: rgba(7,17,27,0.8);
			.detail_wrapper{
				min-height: 100%;
				width: 100%;
				.detail_main{
					padding-top: 64px;
					padding-bottom: 64px;
					.name{
						line-height: 16px;
						text-align: center;
						font-size: 16px;
						font-weight: 700;
					} 
					.star_wrapper{
						margin-top: 18px;
						padding: 2px 0;
						text-align: center;
					}
					.title{
						display: flex;
						width: 80%;
						margin: 28px auto 24px auto;
						.line{
							flex: 1;
							position: relative;
							top: -6px;
							border-bottom: 1px solid rgba(255,255,255,0.2);
						}
						.text{
							padding:  0 12px;
							font-size: 14px;
							font-weight: 700;
						}
					}
					.bulletin{
					        	width: 80%;
					        	margin: 0 auto;
					        	.content{
					        		padding: 0 12px;
					        		line-height: 24px;
					        		font-size: 12px;
					        	}
					        }
					.supports{
						width: 80%;
						margin: 0 auto;
						.supports_item{
							padding:  0 12px;
							margin-bottom: 12px;
							.icon{
								display: inline-block;
								width: 16px;
								height: 16px;
								vertical-align: top;
								margin-right: 6px;
								background-size: 16px;
								background-repeat: no-repeat;
							}
							.decrease{
					            background-image:url(decrease_2@2x.png);	
					        }
					        .guarantee{
					            background-image:url(guarantee_2@2x.png);	
					        }
					        .discount{
					            background-image:url(discount_2@2x.png);	
					        }
					        .invoice{
					            background-image:url(invoice_2@2x.png);	
					        }
					        .special{
					        	background-image:url(special_2@2x.png);	
					        }
					        .text{
					        	font-size: 12px;
					        	line-height: 16px;
					        }

						}
					}
				}
			}
			.detail_close{
				position: relative;
				width: 32px;
				height: 32px;
				margin: -64px auto 0 auto;
				clear: both;
				font-size: 32px;
			}

		}

    }
    .cleatfix{
    	display: inline-block;
    	&:affter{
    		display:block;
    		height: 0;
    		content: ".";
    		line-height: 0;
    		clear: both;
    		visibility: hidden;
    	}
    }
</style>
