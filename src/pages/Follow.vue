<!--
 * @Author: your name
 * @Date: 2020-03-13 22:33:30
 * @LastEditTime: 2020-03-23 15:18:02
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \SellingPlat_APP\src\pages\Follow.vue
 -->
<template>
    <div style="height:100vh;overflow:auto;background:#eee">
        <headerBar title="我的关注"></headerBar>
        <div class="no-data" v-if="foollowList.length === 0">
            <img src="../assets/img/yutang.png" alt="">
            <p>
                一个人都没有，快去关注吧 ! ! !
            </p>
        </div>
        <div class="follow-wrapper" v-else>
            <userlist v-for="item in foollowList" :key="item.uid" :userValue="item"></userlist>
        </div>
        
    </div>
</template>

<script>
import headerBar from '../components/headerBar'
import { Loadmore,Indicator } from 'mint-ui';
import userlist from '../components/userList'
import axios from '../utils/request'
export default {
    components: {
        headerBar,
        userlist
    },
    data () {
        return {
            allLoaded:true,
            foollowList:[]
        }
    },
    mounted () {
        const followId = this.$route.params.followId
        Indicator.open({
            text: '加载中...',
            spinnerType: 'fading-circle'
        });
        axios.get(`/api/Graph/${followId}/followList`).then(res=>{
            Indicator.close();
            if(res.code !== 0 ){
                this.foollowList = [] ;
                return
            }
            this.foollowList = res.data
        })
    },
    methods: {
    }
}
</script>

<style>
.follow-wrapper{
    padding: 1px 5px 80px 5px
}
.no-data{
    height:100vh;
    text-align: center;
    line-height: 100%;
    font-size: 20px;
    max-width: 80%;
    margin:0 auto;
}
</style>