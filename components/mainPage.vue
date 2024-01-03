<template>
  <div style="margin-bottom: 20px; margin-left:30px">
    <!--输入城市名，点击按钮执行search方法进行查询-->
    <span >
        <el-input v-model="cityName" class="w-50 m-2" placeholder="输入城市名称" :style="{ width: '300px' }" 
        @focus="showHistory = true" @blur="showHistory = false"/>
    </span>
    <span><el-button @click="search" type="info">查询</el-button></span>
    <!-- 搜索记录 -->
    <div v-if="showHistory && searchHistory.length > 0" class="search-history">
      <!--<span><el-button @click="clearSearchHistory" type="info" margin-left="5px">清除搜索记录</el-button></span>-->
      <ul>
        <li v-for="(item, index) in searchHistory" :key="index" @click="handleClickHistory(item)">
          {{ item }}
        </li>
      </ul>
    </div>
  </div>

  <!--该组件对各个子组件的引用 1.props传递变量 2.ref调用子组件中的方法-->
  <div>
    <el-row>
      <cityMap :cityName1="cityName" ref="cityMapRef" style="margin-left:20px"></cityMap>
      <cityEnviron :cityName3="cityName" ref="cityEnvironRef" style="margin-left:30px"></cityEnviron>
      
    </el-row>
    <el-row style="margin-top:30px">
      
      <cityInfo :cityName2="cityName" ref="cityInfoRef" style="margin-left:20px"></cityInfo>
      <cityNews :cityName4="cityName" ref="cityNewsRef" style="margin-left:30px"></cityNews>
    </el-row>
  </div>
  
</template>

<script>

import cityMap from './cityMap.vue';
import cityInfo from './cityInfo.vue';
import cityEnviron from './cityEnviron.vue';
import cityNews from './cityNews.vue';
export default {
    name: "mainPage",//此组件名
    components: { cityMap,cityInfo,cityEnviron,cityNews },//挂载子组件
    data() {
      return {
        cityName: '',//输入的城市
        searchHistory: [],
        showHistory: false,
      }
  },
    
    mounted() {
      // 从本地存储中读取搜索记录
      const history = localStorage.getItem("searchHistory");
      if (history) {
        this.searchHistory = JSON.parse(history);
      }
    },
    
    methods: {
      search() {//调用各个子组件中展示城市信息的方法
        // 将搜索关键字添加到搜索记录中
        if (this.cityName) {
          const index = this.searchHistory.indexOf(this.cityName);
          if (index !== -1) {//索引不等于-1代表在当前搜索记录中找到了相同的
            this.searchHistory.splice(index, 1);//将已有的相同元素移除
          }
          this.searchHistory.unshift(this.cityName);
          // 保存搜索记录到本地存储中
          localStorage.setItem("searchHistory", JSON.stringify(this.searchHistory));
          //使用 localStorage 将更新后的搜索历史保存到本地存储中。
          //searchHistory 数组被转换为 JSON 字符串，然后存储在名为 "searchHistory" 的本地存储变量中，以便将其保留在浏览器中
        }
        // 执行搜索操作
        this.$refs.cityMapRef.getMap();//地图
        this.$refs.cityInfoRef.getInfo();//百科
        this.$refs.cityEnvironRef.getWeather();//环境
        this.$refs.cityNewsRef.getNews();//经济、科技文化等
      },
      handleClickHistory(item) {
        console.log("handleClickHistory")
        this.cityName = item;
        this.search();
      },
      clearSearchHistory() {
        console.log("clearSearchHistory");
        this.searchHistory.length=0;
        this.searchHistory = [];
        localStorage.removeItem("searchHistory");
      },
    }

  };
</script>

<style>
.search-history {
  position: absolute;
  top: 110px;
  left: 50px;
  width: 300px;
  max-height: 200px;
  overflow-y: auto;
  background-color: #fff;
  border: 1px solid #ccc;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  z-index: 999;
}
.search-history li {
  left: 0;
  padding: 5px;
  cursor: pointer;
  list-style-type: none; /* 去掉li前面的圆点 */
}
.search-history li:hover {
  background-color: #f5f5f5;
}
</style>