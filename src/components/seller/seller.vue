<template>
    <div class="seller">
        <div class="seller-content">
            <div class="overview">
                <h1 class="title">{{seller.name}}</h1>
                <div class="desc border-1px">
                    <star :size="36" :score="seller.score"></star>
                    <span class="text">({{seller.ratingCount}})</span>
                    <span class="text">月售{{seller.sellCount}}单</span>
                </div>
                <ul class="remark">
                    <li class="block">
                        <h2>起送价</h2>
                        <div class="content">
                            <span class="stress">{{seller.minPrice}}</span>元
                        </div>
                    </li>
                     <li class="block">
                        <h2>商家配送</h2>
                        <div class="content">
                            <span class="stress">{{seller.deliveryPrice}}</span>元
                        </div>
                    </li>
                     <li class="block" style="border:none">
                        <h2>平均配送时间</h2>
                        <div class="content">
                            <span class="stress">{{seller.deliveryTime}}</span>分钟
                        </div>
                    </li>
                </ul>
                <!--<div class="favorite">
                    <span class="icon-favorite" :class="{{}}"></span>
                    <span clas="text">{{favoriteText}}</span>
                </div>-->
            </div>

        </div>
        <split></split>
        <div class="bulletin">
            <h1 class="title">公告与活动</h1>
            <div class="content-wrapper border-1px">
                <p class="content">{{seller.bulletin}}</p>
            </div>
        </div>
        
        <div class="activityall">
            <div v-for="(item,index) in seller.supports" :key="index" class="activity">
                <span class="floaticon" :class="classMap[item.type]"></span>
                <span class="floattext">{{item.description}}</span> 
            </div>
        </div>
        <split></split>
        <div class="pics" >
            <h1 class="title">商家实景</h1>
            <div class="pic-wrapper">
                <ul class="pic-list">
                    <li class="pic-item" v-for="pic in seller.pics">
                        <img :src="pic" width="120" height="90">
                    </li>
                </ul>
            </div>
        </div>
        <split></split>
        <div class="info">
            <h1 class="title border-1px">商家信息</h1>
            <ul>
                <li class="info-item" v-for="info in seller.infos">{{info}}</li>
            </ul>
        </div>
    </div>
</template>
<script>
import star from '../star/star';
import split from '../split/split';
import BScroll from 'better-scroll';
export default {
    props:{
        seller:{
            type:Object
        }
    },
    created(){
        this.classMap = ['decrease','descount','special','invoice','guarantee']
    },
    components:{
        star,
        split
    },
    data(){
        return{
           
        }
    },
    created() {
        this.classMap = ['decrease','discount','special','invoice','guarantee']
        //console.log(this.seller);
  }, 
  methods:{},
    

    
}
</script>
<style lang="stylus" rel='stylesheet/stylus'>
 @import '../../common/stylus/mixin';
    .seller
        position: absolute
        top: 174px
        bottom: 0
        left: 0
        width: 100%
        height:100%
        .overview
            padding: 18px 
            .title
                margin-bottom: 8px
                line-height: 14px 
                color: rgb(7,17,27)
                font-size: 14px
            .desc
                padding-bottom: 18px
                border-1px(raba(7,17,27,0.1))
                font-size: 0
                .star
                    display: inline-block
                    margin-right: 8px
                    vertical-align: top
                .text
                    display: inline-block
                    vertical-align: top
                    line-height: 18px
                    margin-right: 12px 
                    font-size: 10px
                    color:rgb(77,85,93)
            .remark
                display: flex
                margin-top: 18px 
                .block
                    flex: 1
                    text-align: center
                    border-right: 1px solid rgba(7,17,27,0.5)
                    &.last-child
                        border: none
                    h2
                        margin-bottom: 4px 
                        line-height: 10px
                        font-size: 10px
                        color: rgb(147,153,159)
                    .content
                        line-height: 24px
                        font-size: 10px
                        color: rgb(7,17,27)
                    .stress
                        font-size: 24px
        .bulletin
            padding: 18px 18px 0 18px 
            .title  
                margin-bottom: 8px
                line-height: 14px 
                color: rgb(7,17,27)
                font-size: 14px
            .content-wrapper
                padding: 0 12px 16px 12px 
                border-1px(raba(7,17,27,0.1))
                .content
                    line-height: 24px 
                    font-size: 12px 
                    color: rgb(240,20,20 )
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
                        bg-image('decrease_4')
                    &.discount
                        bg-image('discount_4')
                    &.guarantee
                        bg-image('guarantee_4')
                    &.invoice
                        bg-image('invoice_4')
                    &.special
                        bg-image('special_4')
                .floattext
                    margin-left 6px
                    font-size 12px
                    font-weight 200
                    color: rgb(7,17,27)
                   line-height 12px
        .pics
            padding: 18px 
            .title
                margin-bottom: 12px
                line-height: 14px 
                color: rgb(7,17,27)
                font-size: 14px
            .pic-wrapper
                width: 100%
                overflow: hidden
                white-space: nowrap
                .pic-list
                    font-size: 0
                    .pic-item
                        display: inline-block
                        margin-right: 6px
                        width:  120px 
                        height: 90px
                        &:last-child
                            margin: 0
        .info
            padding: 18px 18px 0 18px 
            color: rgb(7,17,27)
            .title
                padding-bottom: 12px 
                line-height: 14px
                font-size: 14px
            .info-item
                padding: 12px 12px
                line-height: 16px
                font-size: 14px

</style>

