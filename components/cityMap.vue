<script>
import AMapLoader from '@amap/amap-jsapi-loader';
export default {
    name: "cityMap",//此组件名
    components: {  },//挂载子组件
    props: {
        cityName1: String//获取父组件中的cityName值
    },
    data() {
      return {
        AMap:null,
        map: null,
        
      }
    },
    mounted() {
      this.initMap();
    },
    methods:{
      //初始化地图
      async initMap() {
        try {
          this.AMap = await AMapLoader.load({
            key: "6216d35221c2b4eb7ae1d4058b289b0e",
            version: "2.0",
            plugins: [
              "AMap.MapType",
              "AMap.Scale",
              "AMap.Geocoder"
            ],
          });
          
          this.map = await new this.AMap.Map("container1", {
            viewMode: "3D",
            zoom: 11,
            center: [116.397428, 39.90923],
          });
        } catch(error) {
          console.log(error);
        }
        
      },
    //显示特定的城市地图
    getMap() {
      console.log(this.cityName1); // 通过控制台观察是否能成功获取输入的城市

      let geocoder = new this.AMap.Geocoder({
        city: "",
      });
      let marker = new this.AMap.Marker();
      let address = this.cityName1;
      
      geocoder.getLocation(address, (status, result) => {
        if (status === 'complete' && result.geocodes.length) {
          let lnglat = result.geocodes[0].location; // 经纬度信息
          marker.setPosition(lnglat);
          
          this.map.add(marker);
          this.map.setFitView(marker);
          this.map.setZoom(11);
        } else {
          console.log('根据地址查询位置失败');
        }
      });
    }  
    }
}

</script>

<template>
  <el-card shadow="always" class="box-card">
    <!--卡片标题部分-->
    <template #header>
      <div class="card-header">
        <span class= "title-color" style="font-size: 18px">城市地图</span>
      </div>
    </template>

    <!--以下是卡片主体内容部分，内容为地图-->
    <div id="container1">
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

#container1 {
  width: 80%;
  height: 400px;
}

.title-color {
    color: rgba(59, 54, 88, 0.925);
  }
</style>
