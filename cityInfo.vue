<script>
import axios from "axios";

export default {
  name: "cityInfo",
  components: {},
  props: {
    cityName2: String
  },
  data() {
    return {
      articles: "",
      Img_url:""
    };
  },
  mounted(){
    axios
          .get(`https://api.wer.plus/api/dub?t=北京`)
          .then((res) => {
            // 处理API响应数据
            console.log(res);//
            this.articles= res.data.data.text.substring(0,350);
            this.Img_url=res.data.data.img_url;
          })
          .catch((error) => {
            console.error("API请求失败：", error);
          });

          console.log(this.articles)//
  },
  methods: {
    async getInfo() {
        axios
          .get(`https://api.wer.plus/api/dub?t=${this.cityName2}`)
          .then((res) => {
            // 处理API响应数据
            console.log(res);//
            this.articles= res.data.data.text.substring(0,350);
            this.Img_url=res.data.data.img_url;
          })
          .catch((error) => {
            console.error("API请求失败：", error);
          });

          console.log(this.articles)//
    }
  }
}

</script>

<template>
  <el-card shadow="always" class="box-card">
    <!--卡片标题部分-->
    <template #header>
      <div class="card-header">
        <span class= "title-color" style="font-size: 18px">百科介绍</span>
      </div>
    </template>

    <!--以下是卡片主体内容部分，百科内容-->
    <div id="container2" style="margin-bottom:5px">
      <div class="common-layout">
         <el-container>
            <el-aside width="150px">
              <img :src= "Img_url" class="image">
            </el-aside>
          <el-main>
            <span style="font-size: 13px; line-height: 1.8;">
             {{ articles }}
             </span>
          </el-main>
         </el-container>
    </div>
      
    
    </div>
    
  </el-card>
</template>

<style>
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.text {
  font-size: 14px;
}

.item {
  margin-bottom: 18px;
}

.box-card {
  width: 480px;

}
.title-color {
    color: rgba(59, 54, 88, 0.925);
  }
.image{
  height: 400px;
}
</style>
