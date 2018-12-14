<template>
    <div>
        <transition name="move">
            <div class="food" v-show="showFlag" ref='food'>
                <div class="image-header">
                    <img :src="food.image" alt="">
                    <div class="back" @click='hide'>
                        <i class="icon-arrow_lift"></i>
                    </div>
                </div>
                <div class="content">
                    <h1 class="title">{{food.name}}</h1>
                    <div class="detail">
                        <span class="sell-count">月售{{food.sellCount}}份</span>
                        <span class="rating">好评率{{food.rating}}</span>
                    </div>
                    <div class="price">
                        <span class="now">￥{{food.price}}</span>
                        <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                    </div>
                </div>
                <div class="cartcontrol-wrapper">
                    <cartcontrol :food='food'></cartcontrol>
                </div>
                <!-- <div @click='addFirst' class="buy" v-show="!food.count || food.count===0">加入购物车</div> -->
                <div class="quarantine"></div>
                <div class="introduce"> <!-- 商品介绍 -->
                    <p class="inttitle">商品介绍</p>
                    <p class="intp">{{food.info}}</p>
                </div>
                <div class="quarantine"></div>
                <div class="evaluate"> <!-- 商品评价 -->
                    <p class="etitle">商家评价</p>
                    <div class="einfo">   
                        <div class="eall"><span>全部</span></div>
                        <div class="recommend"><span>推荐</span></div>
                        <div class="complaints"><span>吐槽</span></div>
                    </div>
                    <div class="onlylook"><i class="icon-check_circle"></i><span>只看有内容的评价</span></div>
                    <div class="men">
                        <ul v-show='food.ratings && food.ratings.length'>
                            <li v-for="(rating,index) in food.ratings" :key='index' class="rating-item">
                                <div class="user">
                                    <span class="name">{{rating.username}}</span>
                                    <img class="avatar" width="12" height="12" :src="rating.avatar" alt="">
                                </div>
                                <div class="time">{{rating.rateTime}}</div>
                                <p class="text">
                                    <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>{{rating.text}}
                                </p>
                            </li>
                        </ul>
                    </div>
                </div>
                
                <div class="end"> </div>
            </div>
        </transition>
    </div>
     
</template>
<script>
import BScroll from 'better-scroll'
import Vue from 'vue'
import cartcontrol from '../cartcontrol/cartcontrol'
export default {
    props:{
        food:{
            type:Object
        }
    },
    components:{
        cartcontrol
    },
    data(){
        return{
            showFlag:false
        }
    },
    methods:{
        show(){
            this.showFlag = true;
            this.$nextTick(()=>{
                if(!this.scroll){
                    this.scroll = new BScroll(this.$refs.food,{
                        click:true
                    })
                }else{
                    this.scroll.refresh();
                }
            })
        },
        hide(){
            this.showFlag = false
        },
        addFirst(event){
            console.log('click')
            if(!event._constructed){
                return;
            }
            this.$dispatch('cart.add',event.target)
            Vue.set(this.food,'count',1);
        }
    }
}
</script>
<style lang="stylus" rel='stylesheet/stylus'>
    .food
        position absolute
        left 0
        top 0
        bottom 48px
        z-index 30
        width 100%
        padding-bottom 100px
        background #fff 
         
        &.move-enter-active,&.move-leave-active
            transition all 0.2 linear
             transform translate3d(0,0,0)
        &.move-enter,&.move-leave-active
            transform translate3d(100%,0,0)
        .image-header
            position relative
            width 100%
            height 0
            padding-top 100%
            img 
                position absolute 
                top 0
                left 0
                width 100%
                height 100%
            .back
                position absolute
                top 10px
                left 0
                .icon-arrow_lift
                    position fixed
                    display block
                    padding 10px
                    font-size 20px
                    color #fff
        .content
            padding 10px
            .title
                line-height 14px
                margin-bottom 8px
                font-size 14px
                font-weight 700
                color rgb(7,17,27)
            .detail
                margin-bottom 18px
                line-height 10px
                height 10px
                font-size 0
                .sell-count,.rating 
                    font-size 10px
                    color rgb(147,153,159)
                .sell-count
                    margin-right 12px
            .price
                font-weight 700
                line-height 24px
                .now
                    margin-right 8px
                    font-size 14px
                    color rgb(240,20,20)
                .old
                    text-decoration line-through
                    font-size 10px
                    color rgb(147,153,159) 
        .cartcontrol-wrapper
            position absolute
            right 12px
            bottom 180px
            
        .buy
            // position absolute 
            // right 18px
            // bottom 20px
            width 74px
            z-index 1000
            height 24px
            line-height 24px
            padding 0 12px
            box-sizing border-box
            border-radius 12px
            font-size 10px
            color #fff 
            background rgb(0,160,200)
        .introduce
            padding 18px
            .inttitle
                font-weight 500
            .intp
                padding 6px 8px 0
                font-size 12px
                font-weight 200
                color rgb(77,85,93)
                line-height 24px
        .evaluate
            padding 18px
            .etitle
                font-weight 500
            .einfo
                display flex
                padding 18px 0
                border-bottom 1px solid rgba(7,17,27,.1)
                .eall,.recommend,.complaints
                    padding 8px 12px
                    font-size 12px
                    line-height 16px
                .eall
                    background rgb(0,160,220)
                    margin-right 8px
                .recommend
                    background rgba(0,160,220,.2)
                    margin-right 8px
                .complaints
                    background rgba(77,85,93,.2)
            .onlylook
                // padding 12px 18px
                font-size 15px
                color rgb(147,153,159)
                line-height 48px
                border-bottom 1px solid rgb(147,153,159)
        .end
            width 100%
            height 50px
</style>

