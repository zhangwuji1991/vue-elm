<template>
  <div class="star" :class="starType">
  	<span v-for="itemClass in itemClasses" :class="itemClass" class="star_item"></span>
  </div>
</template>

<script>
 const LENGTH = 5
 const CLS_ON = 'on'
 const CLS_OFF = 'of'
 const CLS_HALF = 'half'

 export default {
   props: {
     size: {
       type: Number
     },
     score: {
       type: Number
     }
   },
   computed: {
     starType () {
       return 'star_' + this.size
     },
     itemClasses () {
       let result = []
       let score = Math.floor(this.score * 2) / 2
       let hasDecimal = score % 1 !== 0
       let integer = Math.floor(score)
       for (let i = 0; i < integer; i++) {
         result.push(CLS_ON)
       }
       if (hasDecimal) {
         result.push(CLS_HALF)
       }
       while (result.length < LENGTH) {
         result.push(CLS_OFF)
       }
       return result
     }
   }
 }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss"">
	
		.star_48{
			.star_item{
				width: 20px;
				height: 20px;
				margin-right: 22px;
				background-size: 20px 20px;
				/*background-image: url(star48_half@2x.png);*/
				display: inline-block;
			    background-repeat: no-repeat;
				
			}
			.on{
				background-image: url(star48_on@2x.png);
			}
			.off{
				background-image: url(star48_off@2x.png);
			}
			.half{
				background-image: url(star48_half@2x.png);
			}
			.last-child{
				margin-right: 0;
			}
		}
		.star_36{
			.star_item{
				width: 15px;
				height: 15px;
				margin-right: 6px;
				background-size: 15px 15px;
				.on{
					background-image: url(star36_on@2x.png);
				}
				.off{
					background-image: url(star36_off@2x.png);
				}
				.half{
					background-image: url(star36_half@2x.png);
				}
				&.last-child{
					margin-right: 0;
				}
			}
		}
		.star_24{
			.star_item{
				width: 10px;
				height: 10px;
				margin-right: 3px;
				background-size: 10px 10px;
				.on{
					background-image: url(star24_on@2x.png);
				}
				.off{
					background-image: url(star24_off@2x.png);
				}
				.half{
					background-image: url(star24_half@2x.png);
				}
				&.last-child{
					margin-right: 0;
				}
			}
		}
	
</style>
