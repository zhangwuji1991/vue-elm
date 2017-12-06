<template>
  <div id="app">
      <headers :seller="seller"></headers>
   <div class="tab">
      <div class="tab-item">  
         <router-link tag="a" to="/goods">
          商品
          </router-link>
      </div>
      <div class="tab-item">
          <router-link tag="a" to="/ratings">
          评论
          </router-link>
      </div>
      <div class="tab-item">
          <router-link tag="a" to="/seller">
          商家
          </router-link>
      </div>
   </div>
    <router-view :seller="seller"></router-view>   
  </div>
</template>

<script>
  import headers from './components/header/header'
export default {
    data () {
      return {
        seller: {}
      }
    },
    components: {
      headers
    },
    created () {
      this.$http.get('/api/seller')
        .then(response => {
          response = response.data
          console.log(this.seller)
          if (response.errno === 0) {
            this.seller = response.data
          }
        })
    }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
/*@import "common/stylus/mixin.styl";*/
  
  .tab
    width: 100%;
    display: flex;
    height: 40px;
    line-height: 40px;
    /*border-1px(rgba(7,17,27,0.1));*/
    border:1px solid rgba(7,17,27,0.1);
    .tab-item
      flex: 1;
      text-align: center;
      a
        display: block;
        font-size: 14px;
        color: rgb(77,85,93);      
      .actives
          color:rgb(240,20,20) ;

</style>
