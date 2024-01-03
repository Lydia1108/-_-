<script>
import axios from "axios"
export default {
    name: "cityNews",//此组件名
    components: {  },//挂载子组件
    props: {
        cityName4: String//获取父组件中的cityName值
    },
    data() {
      return {
        activetab:'first',
        economynews:[
        {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
        ],//经济
       
       tournews:[
       {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
       ],//文化
        newsData: [
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
          {name:'--',time:'--'},
       ]
      }
    },
    mounted() {
      this.findNews("北京")
    },
    
    methods:{
      findNews(x){
        axios//综合
            .get(`https://apis.tianapi.com/generalnews/index?key=48d347bfd966434d4796336ca2dbccdc&num=8&word=${x}`)
            .then((res) => {
              console.log(res.data.result.newslist);
              let i;
              for(i=0;i<8;i++){
                this.newsData[i].name=res.data.result.newslist[i].title.substring(0,18)+" ...";
                this.newsData[i].time=res.data.result.newslist[i].ctime.substring(0,10);
              }
            })
            .catch((error) => {
              console.error("API请求失败", error);
            });

            
          axios//经济
            .get(`https://apis.tianapi.com/allnews/index?key=48d347bfd966434d4796336ca2dbccdc&num=8&word=${x}&col=32`)
            .then((res) => {
              console.log(res.data.result.newslist);
              let i;
              for(i=0;i<8;i++){
                this.economynews[i].name=res.data.result.newslist[i].title.substring(0,18)+" ...";
                this.economynews[i].time=res.data.result.newslist[i].ctime.substring(0,10);
              }
            })
            .catch((error) => {
              console.error("API请求失败", error);
            });

           
          axios//文旅
            .get(`https://apis.tianapi.com/allnews/index?key=48d347bfd966434d4796336ca2dbccdc&num=8&word=${x}&col=18`)
            .then((res) => {
              console.log(res.data.result.newslist);
              let i;
              for(i=0;i<8;i++){
                this.tournews[i].name=res.data.result.newslist[i].title.substring(0,18)+" ...";
                this.tournews[i].time=res.data.result.newslist[i].ctime.substring(0,10);
              }
            })
            .catch((error) => {
              console.error("API请求失败", error);
            });
      },

      getNews(){
          this.findNews(this.cityName4);
      }
          
      },
    
    
}
</script>

<template>
  <el-card shadow="always" class="box-card">
    <!--卡片标题部分-->
    <template #header>
      <div class="card-header">
        <span class= "title-color" style="font-size: 18px">城市信息</span>
      </div>
    </template>

    <!--以下是卡片主体内容部分，新闻内容-->
    <div id="container6" style="margin:0"   >
      <el-tabs v-model="activetab" type="border-card" stretch="true" @tab-click="handleClick">

      <el-tab-pane label="综合" name="first">
        <el-table :data="newsData" stripe style="width: 100%">
          <el-table-column prop="name" label="新闻名称" width="300" />
          <el-table-column prop="time" label="发布时间" width="108" />
        </el-table>
      </el-tab-pane>
      
     

      <el-tab-pane label="经济资讯" name="second" >
        <el-table :data="economynews" stripe style="width: 100%">
          <el-table-column prop="name" label="新闻名称" width="300" />
          <el-table-column prop="time" label="发布时间" width="108" />
        </el-table>
      </el-tab-pane>

      <el-tab-pane label="文旅新闻" name="third" >
        <el-table :data="tournews" stripe style="width: 100%">
          <el-table-column prop="name" label="新闻名称" width="300" />
          <el-table-column prop="time" label="发布时间" width="108" />
        </el-table>
      </el-tab-pane>

      
      
  </el-tabs>
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
  .container6 {
  display: flex;
  justify-content: center;
  align-items: center;
  
}
</style>
