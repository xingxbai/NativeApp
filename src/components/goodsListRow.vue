<!--
 * @Author: your name
 * @Date: 2020-03-22 12:15:01
 * @LastEditTime: 2020-05-12 21:59:13
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \SellingPlat_APP\src\components\goodsListRow.vue
 -->

<template>
    <div class="wrapper">
        <div class="goods-item" v-for="item in goods" @click="goGoodsDetail(item)">
            <div class="img">
                <img class="img"  v-lazy="item.productPic" :src="item.productPic" alt="">
            </div>
            <div class="goods-content">
                <div class="desc">{{item.productContent}}</div>
            </div>
            <div class="goods-bottom">
                <div class="price">
                    ￥
                    <span>{{item.productPrice}}</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props:['goods','path'],
    methods: {
        goGoodsDetail (value) {
            if (this.path === 'UserDetail') {
                value.productId = value.id
            }
			this.$router.push({
				name: 'goods',
				params: {
					goodsId: value.productId
				}
			})
		},
    }
}
</script>

<style lang="scss" scoped>
.wrapper{
    background: #eee;
    display: flex;
    flex-wrap: wrap;
    min-height: 70vh;
    max-height: 90vh;
    padding-bottom: 45px;
    overflow-y: auto;
    .goods-item{
        width: 50%;
        box-sizing: border-box;
        padding: 5px 7px;
        font-size: 0;
        .img{
            border-radius: 9px 9px 0 0;
            border-bottom: 0;
            width: 100%;
            height: 200px;
        }
        .goods-content{
            background: #fff;
            font-size: 14px;
            padding: 5px 8px;
            box-sizing: border-box;
            border-bottom: 0;
            .desc{
                overflow: hidden;
                text-overflow: ellipsis;
                display: -webkit-box;
                border-bottom: 0;
                -webkit-line-clamp: 2; //行数
                -webkit-box-orient: vertical;
            }
        }
        .goods-bottom{
            background: #fff;
            font-size: 14px;
            box-sizing: border-box;
            padding: 5px 8px;
            padding-bottom: 11px;
            display: flex;
            border-radius:  0 0 9px 9px;
            align-items: center;
            .price{
                color: red;
                flex: 1;
                font-size: 16px;
                font-weight: 500;
            }
            .want{
                font-size: 12px;
                color: #aaa;
            }
        }
    }
}
</style>
