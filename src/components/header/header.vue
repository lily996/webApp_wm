<template>
    <div id="head">
      <transition name="fade">
        <div class="detalistop" v-show="detailspan">
          <div class="detail-wrapper clearfix">
            <div class="detail-main">
                <div class="floattitle">{{seller.name}}</div>

                <div class="star-wrapper">
                  <star :size='48' :score='seller.score'></star>
                </div>

                <div class="floatflex">
                  <div class="line"></div>
                  <div class="text">优惠信息</div>
                  <div class="line"></div>
                </div>
                <div class="activityall">
                  <div v-for="(item,index) in seller.supports" :key="index" class="activity">
                    <span class="floaticon" :class="classMap[item.type]"></span>
                    <span class="floattext">{{item.description}}</span> 
                  </div>
                </div>

                <div class="floatflex">
                  <div class="line"></div>
                  <div class="text">商家公告</div>
                  <div class="line"></div>
                </div>
                <div class="acttext">
                  {{seller.bulletin}}
                </div>
            </div>
          </div>
          <div class="detail-close" @click="detailsbesttopno">
            <i class="icon-close"></i>
          </div>
        </div>
      </transition>
      
      <div class="bg">  <!--logo-->
        <img :src="seller.avatar" alt="" width="100%" height="100%"> 
      </div>
      <div class="top"> <!-- 上层 --> 
        <div class="logo">  <!--logo--> 
          <img :src="seller.avatar" alt=""> 
        </div>
        <div class="moddle_info">
          <div class="title"><span class="sone"></span><span class="stwo">{{seller.name}}</span></div> <!--  logo和标题 -->
          <div class="give_type">{{seller.description}}  /  {{seller.deliveryTime}}分钟送达</div>  <!-- 配送方式 -->
          <div class="discount" v-if='seller.supports'>
            <span class="icon" :class="classMap[seller.supports[0].type]"></span>
            <span class="text"> {{seller.supports[0].description}}</span> 
          </div>  <!-- 优惠 -->
        </div>
        <div class="right_info">
          <div class="detalis"  v-if='seller.supports' @click="detailsbesttop">
            <span>{{seller.supports.length}}个</span>
            <i class="icon-keyboard_arrow_right"></i>
          </div>
        </div>
      </div>
      <div class="bottom" @click="detailsbesttop">
        <span class="botimg"></span><span class="botinfo">{{seller.bulletin}}</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
      
    </div>
</template>
<script>
import star from '../star/star.vue'
export default {
  props:{
    seller:{
      type:Object
    }
  },
  data() {
    return {
      detailspan:false
    };
  },
  created() {
    this.classMap = ['decrease','discount','special','invoice','guarantee']
    console.log(this.seller);
  }, 
  components:{
    star
  },
  mounted() {

  },
  methods: {
    detailsbesttop(){
      this.detailspan = true
    },
    detailsbesttopno(){
      this.detailspan = false
    }
  }
};
</script>
<style lang='stylus' rel='stylesheet/stylus'>
@import '../../common/stylus/mixin';
  #head
    position relative
    width 100%
    .bg
      position absolute   
      width 100%
      height 100%
      background-size:100%;
      filter: blur(5px)
      z-index -1
    .top
      display flex
      width 100%
      padding 24px 0 18px
      background rgba(7,17,27,.5)
      .logo
        margin-top 2px
        padding-left 24px
        img
          width 64px
          height 64px
          border-radius 2px
        
      .moddle_info
        margin-left 16px
        font-size 0
        .title
          margin-top 4px
          .sone
            display inline-block
            vertical-align top
            width 30px
            height 18px
            bg-image('brand')
            background-size 30px 18px
            background-repeat no-repeat
          .stwo 
            padding-left 6px
            padding-top 10px
            font-size 16px
            color rgb(255,255,255)
            font-weight bold
            line-height 16px
            
        .give_type
          margin-top 8px
          margin-bottom 10px
          font-size 12px
          color rgb(255,255,255)
          font-weight 200
          line-height 12px
        .discount
          
          .icon
            display inline-block
            width 12px
            height 12px
            margin-right 4px
            background-size 12px 12px
            background-repeat no-repeat
            vertical-align top 
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.guarantee
              bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
             
          .text
            vertical-align top 
            font-size 10px
            color rgb(255,255,255)
            font-weight 200
            line-height 12px
      .right_info
        position relative
        .detalis
          position absolute
          width 36px
          bottom 0
          padding 7px 8px 7px 8px
          font-size 12px
          color rgb(255,255,255)
          font-weight 200
          line-height 12px
          background rgba(0,0,0,.2)
          border-radius 10px
    .bottom
      height 28px
      padding-right 16px
      line-height 28px
      color rgb(255,255,255)
      background rgba(7,17,27,.7)
      overflow hidden
      text-overflow ellipsis
      white-space nowrap
      .botimg 
        display inline-block
        width 22px
        height 12px
        margin-left 12px
        bg-image('bulletin')  
        background-size 22px 12px
        background-repeat no-repeat
        vertical-align middle
      .botinfo
        margin 0 4px
        padding-right 5px
        font-size 10px
      .icon-keyboard_arrow_right
        position absolute
        right 8px
        bottom 8px
        font-size 10px
    .detalistop
      position fixed
      z-index 100
      width 100%
      height 100%
      background rgba(7,17,27,0.8)
      font-size 0
      overflow auto 
      backdrop-filter blur(10px)
      transition all 0.4s
      &.fade-enter-active,&.fade-leave
        opacity 1
        background rgba(7,17,27,0.8)
      &.fade-enter,&.fade-leave-active
        opacity 0
        background rgba(7,17,27,0)

      .detail-wrapper
        width 100%
        min-height 100%
        .detail-main
          margin-top 64px
          padding-bottom 64px
          .floattitle 
            font-size 16px
            font-weight 700
            color rgb(255,255,255)
            line-height 16px
            text-align center
          .star-wrapper
            margin-top 16px
            padding:2px 0
            text-align center
          .floatflex
            display flex
            width 80%
            margin 28px auto 24px auto 
            .line
              flex 1  //等分
              position relative
              top -6px
              border-bottom 1px solid rgba(255,255,255,0.2)
            .text
              padding 0 12px
              font-size 14px
              font-weight 600
              color rgb(255,255,255)
          .activityall
            margin 0 auto
            width 80%
            .activity
              margin-left 12px
              margin-bottom 12px
              .floaticon
                display inline-block
                width 16px
                height 16px
                margin-right 4px
                background-size 16px 16px
                background-repeat no-repeat
                vertical-align top 
                &.decrease
                  bg-image('decrease_1')
                &.discount
                  bg-image('discount_1')
                &.guarantee
                  bg-image('guarantee_1')
                &.invoice
                  bg-image('invoice_1')
                &.special
                  bg-image('special_1')
              .floattext
                margin-left 6px
                font-size 12px
                font-weight 200
                color rgb(255,255,255)
                line-height 12px
          .acttext
            margin 0 auto
            width 80%
            font-size 12px
            font-weight 200
            color rgb(255,255,255)
            line-height 24px
      .detail-close
        position relative
        width 32px
        height 32px
        margin -64px auto 0 auto 
        clear both
        font-size 32px
        color rgba(255,255,255,.8)
        
          
        


      



</style>
