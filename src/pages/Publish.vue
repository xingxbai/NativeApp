<!--
 * @Author: your name
 * @Date: 2020-03-08 12:20:00
 * @LastEditTime: 2020-05-03 16:55:16
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \SellingPlat_APP\src\pages\notFound.vue
 -->
<template>
  <div>
      <headerBar title="发布"></headerBar>
      <div class="publish">
          <mt-field label="商品标题" placeholder="品类品牌类型都是买家喜欢搜索的" v-model="goods.upload.productName"></mt-field>
          <mt-field label="商品描述" placeholder="描述一下你的闲置"  v-model="goods.upload.productContent"></mt-field>
          <div class="goods-upload">
            <!-- <input type="file">
            <img src="../assets/img/相机.png" alt="" style="widht:50px;height:50px;margin:auto;margin-top:10px;margin-bottom:10px"> -->
            <input type="file"  
              accept="image/jpeg,image/jpg,image/png"
              name="file" id="fileUpload"
              @change ="tirggerFile($event)"
              style="width:50%;height:100%;opacity:0" 
              capture="camera">
            <!-- <img src="../assets/img/相机.png" alt="" 
            style="widht:50px;height:50px;
                    position:absolute;
                    left:45%;top:10px;"> -->
            <p class="goods-upload-desc">请选择拍照片或者上传图片</p>
          </div>
          <div class="upload-img-list">
              <!-- <img :src="item" alt="" style="width:100px;height:100px" v-for="(item,index) in imgUrl" :key="index"> -->
              <img :src="item" alt="" style="width:100px;height:100px" v-for="(item,index) in goods.upload.productPic" :key="index" @click="handleTabBar(index)">
          </div>

          <div class="price">
            <mt-button type="default" style="width:90%;margin-left:5%;border-radius:20px;background:#ffda44;color:#fff">开个价</mt-button>
            <mt-field label="商品价格" placeholder="请输入商品价格" type="number" v-model="goods.upload.productPrice" style="margin-top:30px"></mt-field>
            <mt-field label="商品数量" placeholder="请输入商品数量" type="number" v-model="goods.upload.productNum"></mt-field>
            
            <div @click="goKinds">
              <mt-cell title="分类" label="选择类别" is-link
                      :value="goods.upload.productTag"
                      ></mt-cell>
            </div>
            <div class="publish-submit">
              <mt-button type="default" style="width:96%;font-size:14px;margin-left:2%;background:red;color:#fff" @click="handleclick">确定发布</mt-button>
            </div>
          </div>
      </div>
      <mt-actionsheet
          :actions="actions"
          cancelText=""
          v-model="sheetVisible">
      </mt-actionsheet>
      <tabBar></tabBar>
  </div>
</template>

<script>
import { Field,Button,Cell,Indicator,Actionsheet} from 'mint-ui';
import headerBar from '../components/headerBar'
import tabBar from '../components/tabBar'
import axios from '../utils/request'
import { mapState,mapMutations,mapActions } from 'vuex';
export default {
  components: {
      tabBar,
      Field,
      headerBar,
      Cell,
      Actionsheet
  },
  data () {
    return {
      title:'',
      desc:'',
      price:'',
      imgUrl:'',
      sheetVisible: false,
      imgIndex: '',
			actions: [{
				name: '删除',
		    method : this.delelteImg
			  },{
        name: '查看大图',
        method : this.watchBigImg
			}],
    }
  },
  computed:{
    ...mapState({
        'goods':'goods'
    })
  },
  methods: {
    ...mapActions({
      'submitPublish':'goods/submitPublish'
    }),
    ...mapMutations({
      'setPublishImg':'goods/setPublishImg',
      'delelteProductImg':'goods/delelteProductImg'
    }),
    //去分类页
    goKinds () {
      this.$router.push({
        path:'/kinds'
      })
    },
    //删除图片和查看大图tabbar
    handleTabBar(index) {
      this.sheetVisible = true
      this.imgIndex = index
    },
    // 删除图片
    delelteImg () {
      this.delelteProductImg(this.imgIndex)
    },
    //自动上传文件
    tirggerFile (event) {
        let self = this;
        let file = event.target.files[0]
        let param = new FormData() // 创建form对象
        param.append('file', file, file.name) // 通过append向form对象添加数据
        // param.append('type', '1') // 添加form表单中其他数据
        console.log(param.get('file')) // FormData私有类对象，访问不到，可以通过get判断值是否传进去
        let config = {
          headers: {'Content-Type': 'multipart/form-data'}
        }
        Indicator.open('上传图片中...');
        // 添加请求头
        axios.post('/fileSystem/upLoadImage', param, config)
          .then(response => {
            Indicator.close()
            //TODO 限制上传一张
            //this.imgUrl.push( response )
            // this.imgUrl = response
            this.setPublishImg(response.data)
          })
    },

    handleclick () {
      for(let key in this.goods.upload) {
        //循环判空，用户提示文案
        if ( this.goods.upload[key] !== 0 && !this.goods.upload[key] ) {
          Indicator.open(`${key}不能为空`);
          setTimeout(()=>{
            Indicator.close()
          },1000)
          return
        }
        if (this.goods.upload.productPic.length <= 0) {
          Indicator.open(`请选择上传图片`);
          setTimeout(()=>{
            Indicator.close()
          },1000)
          return
        }
      }
      this.submitPublish(this.$router)
    }
  }
}
</script>

<style>
.publish{
  /* margin-top:39px; */
  height: 87vh;
  padding-top: 11px;
  position: relative;
}
.goods-upload{
  height: 100px;
  background-color: #f8f8f8;
  text-align: center;
  position: relative;
  background-image: url('../assets/img/相机.png');
  background-repeat: no-repeat;
  background-position:center;
  background-size: 50px 50px;
  background-position: 50% 10px;
}
.goods-upload-desc{
  color: #888;
  font-size: 14px;
  position: absolute;
  left:30%;
  bottom: 10px;
}
.price{
  margin-top: 10px;
}
.publish-submit{

  position: absolute;
  left: 0;
  right: 0;
  bottom: 60px;
}
</style>