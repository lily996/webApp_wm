<template>
<div>
    <div class="goods">
        <div class="menu-wrapper" ref='menuWrapper'>
            <ul>
                <li v-for="(item,index) in goods" :key="index" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index,$event)">
                    <span class="text">
                        <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
                        {{item.name}}
                    </span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" ref='foodsWrapper'>
            <ul>
                <li v-for="(item,index) in goods" :key='index' class="food-list food-list-book">
                    <h1 class="title">{{item.name}}</h1>
                    <ul>
                        <li @click='selectFood(food,$event)' v-for="(food,index) in item.foods" :key="index"  class="food-item border-1px">
                            <div class="icon">
                                <img :src="food.icon" alt="" width="57" height="57">
                            </div>
                            <div class="content">
                                <h2 class="name">{{food.name}}</h2>
                                <p class="desc">{{food.description}}</p>
                                <div class="extra">
                                    <span class="count">月售{{food.sellCount}}份</span><span class="">好评率{{food.rating}}%</span>
                                </div>
                                <div class="price">
                                    <span class="now">￥{{food.price}}</span>
                                    <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                                </div>
                                <div class="cartcontrolWrapper">
                                    <cartcontrol :food="food"></cartcontrol>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <shopcart  :selectFoods='selectFoods' :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
    
    </div>
    <food :food='selectedFood' ref='food'></food>
</div>
  
</template>
<script>
import BScroll from 'better-scroll'
import shopcart from '../shopcart/shopcart'
import cartcontrol from '../cartcontrol/cartcontrol'
import food from '../food/food'
export default {
    props:{
        seller:{
            type:Object
        }
    },
    components:{
        shopcart,
        cartcontrol,
        food
    },
    events:{
        'cart.add'(target){
            this._drop(target)
        }
    },
    data(){
        return{
            goods:[],
            listHeight:[],
            scrollY:0,
            selectedFood:{}
        }
    },
    computed:{
        currentIndex(){
            for(let i = 0;i<this.listHeight.length;i++){
                let height1 = this.listHeight[i];
                let height2 = this.listHeight[i+1];
                if(!height2 || this.scrollY>=height1&&this.scrollY<height2){
                    return i;
                }
            }
            return 0;
        },
        selectFoods(){
            let foods = [];
            this.goods.forEach((good)=>{
                good.foods.forEach((food)=>{
                    if(food.count){
                        foods.push(food)
                    }
                })
            })
            return foods;
        }
    },
    created(){
        this.classMap = ['decrease','discount','special','invoice','guarantee']
        this.getdata();
    },
    mounted(){

    },
    methods:{
        selectFood(food,$event){
            if(!event._constructed){
                return;
            }
            this.selectedFood = food;
            this.$refs.food.show();
        },
        getdata() {
            var that = this;
            this.$http.get("/api/goods").then(
                res => {
                    that.goods = res.data;
                    this.$nextTick(()=>{
                       that._initScroll(); 
                       that._calculateHeight();
                    })
                    
                    console.log( that.goods);
                },
                err => {
                     console.log(err);
                }
            );
        },
        _initScroll(){
            this.meunScroll = new BScroll(this.$refs.menuWrapper,{
                click:true  //默认派发一次点击事件
            })
            this.foodsScroll = new BScroll(this.$refs.foodsWrapper,{
                click:true,
                probeType:3
            })
            this.foodsScroll.on('scroll',(pos)=>{
                this.scrollY = Math.abs(Math.round(pos.y));
            });
        },
        _calculateHeight(){
            let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-book');
            let height = 0;
            this.listHeight.push(height);
            for(let i = 0 ; i<foodList.length ; i++){
                let item = foodList[i];
                height += item.clientHeight;
                this.listHeight.push(height);
            }
        },
        selectMenu(index,event){
            if(!event._constructed){
                return;
            }
            let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-book');
            let el = foodList[index]
            this.foodsScroll.scrollToElement(el,300);
            console.log(index);
        },
        // _drop(target){
        //     this.$refs.shopcart
        // }

    }
}
</script>
<style lang="stylus" rel='stylesheet/stylus'>
    @import '../../common/stylus/mixin';
    .goods
        display flex
        position absolute
        top 178.4px
        bottom 46px
        width 100%
        overflow hidden
        .menu-wrapper
            flex 0 0 80px
            width 80px
            background #f3f5f7
            .menu-item
                display table
                width 56px
                height 54px
                padding 0 12px
                line-height 14px
                &.current
                   position relative
                   z-index 10 
                   margin-top -1px
                   background #fff
                   font-weight 700
                   .text
                       border-none()
                .text
                    display table-cell
                    width 56px
                    vertical-align middle
                    border-1px(rgba(7,17,27,.1))
                    font-size 12px
                .icon 
                    display inline-block
                    width 12px
                    height 12px
                    background-size 12px 12px
                    background-repeat no-repeat
                    vertical-align top 
                    &.decrease
                        bg-image('decrease_3')
                    &.discount
                        bg-image('discount_3')
                    &.guarantee
                        bg-image('guarantee_3')
                    &.invoice
                        bg-image('invoice_3')
                    &.special
                        bg-image('special_3')
        .foods-wrapper
            flex 1
            .title
                padding-left 14px
                height 26px
                line-height 26px
                border-left 2px solid #d9dde1
                font-size 12px
                color:rgb(14,153,159)
                background #f3f5f7
            .food-item
                display flex
                padding 18px 18px 0
                padding-bottom 18px
                border-1px(rgba(7,17,27,.1))
                &:last-child
                    border-none()
                    margin-bottom 0
                .icon
                    flex 0 0 57px
                    margin-right 10px
                .content
                    flex 1
                    .name
                        margin 2px 0 8px 0
                        height 14px
                        line-height 14px
                        font-weight bold
                        font-size 14px
                        color rgb(7,17,27)
                    .desc,.extra
                        font-size 10px
                        color rgb(147,153,159)
                        line-height 10px
                    .extra
                        margin-top 8px
                        .count
                            padding-right 12px
                    .price
                        margin-top 8px
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
                    .cartcontrolWrapper
                        position absolute
                        right 0
                        bottom 12px
                        
</style>
