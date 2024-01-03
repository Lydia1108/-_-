<script>
import axios from "axios";
export default {
    name: "cityEnviron",//此组件名
    components: {  },//挂载子组件
    props: {
        cityName3: String,//获取父组件中的cityName值
    },

    data() {
      return {
        cloud: "--",//云量（百分比）
        feelsLike: "--",//体感温度
        humidity: "--",//相对湿度
        obsTime: "--",//时间
        windDir: "--",//风向
        pressure: "--",//气压（hpa)
        temp: "--",//温度
        text: "--",//天气
        windScale: "--",//风力等级
        
        aqi:"--",
        category:"--",
        pm10: "--",
        pm2p5: "--",
        Temp:"--",
        Humidity:"--",
        Feelslike:"--"
      }
    },
    mounted(){
      this.findWeather(101010100);
    },
    methods:{
      findWeather(x){
        axios
          .get(`https://geoapi.qweather.com/v2/city/lookup?location=${x}&key=f7b386377b494b8392f72542c1268630`)
          .then((res) => {
            let citycode=res.data.location[0].id;
            axios
            .get(`https://devapi.qweather.com/v7/weather/now?key=f7b386377b494b8392f72542c1268630&location=${citycode}`)
            .then((res) => {
              this.cloud=res.data.now.cloud;
              this.dew=res.data.now.dew;
              this.feelsLike=res.data.now.feelsLike;
              this.humidity=res.data.now.humidity;
              this.obsTime=res.data.now.obsTime;
              this.windDir=res.data.now.windDir;
              this.pressure=res.data.now.pressure;
              this.temp=res.data.now.temp;
              this.text=res.data.now.text;
              this.vis=res.data.now.vis;
              this.windDir=res.data.now.windDir;
              this.windScale=res.data.now.windScale;
              this.Temp=this.temp+"℃";
              this.Humidity=this.humidity+"％"
              this.Feelslike=this.feelsLike+"℃";
            })
            .catch((error) => {
              console.error("API请求失败：", error);
            });
            axios
            .get(`https://devapi.qweather.com/v7/air/now?key=f7b386377b494b8392f72542c1268630&location=${citycode}`)
            .then((response) => {
              this.aqi=response.data.now.aqi;
              this.category=response.data.now.category;
              this.pm10=response.data.now.pm10;
              this.pm2p5=response.data.now.pm2p5;
              console.log(response.data.now.category);
            })
            .catch((error) => {
              console.error("API请求失败：", error);
            });
          })
          .catch((error) => {
            console.error("API请求失败：", error);
          });
      },

      getWeather() {
        this.findWeather(this.cityName3);
      }
    }
}
</script>

<template>
  <el-card shadow="always" class="box-card">
    <!--卡片标题部分-->
    <template #header>
      <div class="card-header" >
        
        <span class= "title-color" style="font-size: 18px">环境状况</span>
      </div>
    </template>

    <!--以下是卡片主体内容部分-->
    
    <div id="container5" style="margin-bottom:5px border-bottom-style: solid" >
      <el-descriptions class="margin-top" :column="2" :size="size" border>

      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            天气
          </div>
        </template>
        &nbsp;&nbsp;{{ text }}
      </el-descriptions-item>

      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            气温
          </div>
        </template>
        &nbsp; {{ Temp }}
      </el-descriptions-item>

      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            湿度
          </div>
        </template>
        &nbsp; {{ Humidity }}
      </el-descriptions-item>

      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            体感温度
          </div>
        </template>
        &nbsp; {{ Feelslike }}
      </el-descriptions-item>

      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            气压(hpa)
          </div>
        </template>
        &nbsp; {{ pressure }}
      </el-descriptions-item>

      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            风向
          </div>
        </template>
        &nbsp; {{ windDir }}
      </el-descriptions-item>

      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            云量(％)
          </div>
        </template>
        &nbsp; {{ cloud }}
      </el-descriptions-item>

      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            风力等级
          </div>
        </template>
        &nbsp; {{ windScale }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            AQI
          </div>
        </template>
        &nbsp; {{ aqi }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            空气质量等级
          </div>
        </template>
        &nbsp; {{ category }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
          PM2.5
          </div>
        </template>
        &nbsp; {{ pm2p5 }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            PM10
          </div>
        </template>
        &nbsp; {{ pm10 }}
      </el-descriptions-item>
      
      <el-descriptions-item>
        <template #label>
          <div class="cell-item">
            &nbsp;
            时间
          </div>
        </template>
        &nbsp;{{ obsTime }}
      </el-descriptions-item>
    </el-descriptions>

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
</style>