<template>
    <div class="ratings">
        <div class="ratings-content">
            <div class="overview">
                <div class="overview-left">
                    <h1 class="score">{{seller.score}}</h1>
                    <div class="title">综合评分</div>
                    <div class="rank">高于周边商家{{seller.rankRate}}%</div>
                </div>
                <div class="overview-right">
                    <div class="score-wrapper">
                        <span class="title">服务态度</span>
                        <star :size="36" :score="seller.serviceScore"></star>
                        <span class="score">{{seller.serviceScore}}</span>
                    </div>
                    <div class="score-wrapper">
                        <span class="title">商品评分</span>
                        <star :size="36" :score="seller.foodScore"></star>
                        <span class="score">{{seller.foodScore}}</span>
                    </div>
                    <div class="delivery-wrapper">
                        <span class="title">送达时间</span>
                        <span class="delivery">{{seller.deliveryTime}}</span>
                    </div>
                </div>
            </div>
        </div>
        <split></split>
        <div class="ratings-title">
            <div class="all">全部0</div>
            <div class="statisfied">满意0</div>
            <div class="notstatisfied">不满意0</div>
        </div>
    </div>
</template>
<script>
    import star from '../star/star';
    import split from '../split/split';
export default {
    props:{
        seller:{
            type:Object
        }
    },
    components:{
        star,
        split
    },
    data(){
        return{}
    }
}
</script>
<style lang="stylus" rel='stylesheet/stylus'>
    .ratings
        position: absolute
        top: 174px
        bottom: 0
        left: 0
        width: 100%
        overflow: hidden
        .overview
            display: flex
            padding: 18px 0
            .overview-left
                padding-bottom: 6px 0
                flex: 0 0 137px
                width: 137px
                border-right: 1px solid rgba(7,17,27,0.5)
                text-align:center
                .score
                    line-height: 28px
                    margin-bottom: 6px
                    font-size: 24px
                    color: rgb(255,153,0)
                .title
                    margin-bottom: 8px
                    line-height: 12px
                    font-size: 12px
                    color: rga(7,17,27)
                .rank
                    line-height: 10px
                    font-size: 10px
                    color: rgb(147,153,159)
            .overview-right
                flex:1
                padding-left: 24px
                .score-wrapper
                    margin-bottom: 8px
                    font-size: 0
                    .title
                        line-height: 18px
                        display: inline-block
                        vertical-align: top
                        font-size: 12px
                        color: rgb(7,17,27)
                    .star
                        display: inline-block
                        vertical-align: top
                        margin: 0 12px 
                    .score  
                        display: inline-block
                        line-height: 18px
                        vertical-align: top
                        font-size: 12px
                        color: rgb(255,153,0)
                .delivery-wrapper
                    font-size:0
                    .title
                        line-height: 18px
                        font-size: 12px
                        color: rgb(7,17,27)
                    .delivery
                        margin-left: 12px
                        font-size: 12px
                        color: rgb(147,153,159)
        .ratings-middle
            width: 100%
            height: 20px;
            border-top: 1px solid rgba(7,17,27,0.1)
            border-bottom 1px solid rgba(7,17,27,0.1)
            background: #f3f5f7   
        .ratings-title
            display: flex
            justify-content: flex-start
            margin-top: 25px
            .all
                width: 55px
                height: 30px
                line-height: 30px
                margin-right: 8px
                font-size:12px;
                color:#fff;
                background: #00a0dc
                text-align: center
            .statisfied
                width: 55px
                height: 30px
                line-height: 30px
                font-size: 12px
                text-align: center
                margin-right: 8px
                background: rgba(0,160,220,0.2)
            .notstatisfied
                width: 55px
                height: 30px
                line-height: 30px
                font-size: 12px
                text-align: center
                background: rgba(77,85,93,0.2)
            
</style>
