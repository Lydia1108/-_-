<template>
  <div>
    <el-container ><!--此容器存放header-->
      <el-header style="background-color: rgba(35, 31, 52, 0.925);">
      <img src="@/assets/citylogo.png" style="width:35px;top:10px;position:relative">
      <span style="font-size: 20px;margin-left:15px;color:white">城市信息查询系统</span>
      </el-header>
    </el-container>
     
    <el-container><!--此容器存放main-->
      <el-main style="background-color: rgba(247, 241, 247, 0.986);">
        <main-page/>
      </el-main>
    </el-container>
  </div>
</template>

<script>
  import mainPage from './components/mainPage.vue';
  export default {
    name: "app",//此组件名
    components: { mainPage },//挂载子组件
    data() {
      return {
        
      }
    },
  };
</script>
  
 