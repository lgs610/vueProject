<!-- <template>
 <div id="app">
 <x-chart :id="id" :option="option"></x-chart>
 </div>
</template>
<script>
// 导入chart组件
import XChart from 'chart.vue'
// 导入chart组件模拟数据
import options from './chart-options/options'
export default {
 name: 'app',
 data() {
 let option = options.bar
 return {
  id: 'test',
  option: option
  }
 },
 components: {
  XChart
 }
}
</script>
<style>
#test {
 width: 400px;
 height: 400px;
 margin: 40px auto;
}
</style>
 -->





<template>
  <div class="hello">
    <div class="block">
    <!-- <h1 class="demonstration">米歌资讯欢迎您</h1> -->
    <el-carousel height="500px">
      <el-carousel-item  class="image" v-for="(img,index) in imgList" :key="index">
        <img v-bind:src="img.url">
      </el-carousel-item>
    </el-carousel>
  </div>
  </div>
</template>
<script>

  export default{
    data(){
      return{
        imgList:[
        {url:require('@/image/9.jpg')},
        {url:require('@/image/3.jpg')},
        {url:require('@/image/7.jpg')},
        {url:require('@/image/5.jpg')}
      ]
      }
    }
  }
</script>



<style>

.demonstration{
  text-align: center;
  color: #6b56a3;
}
  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 150px;
    margin: 0;
  }

  .el-carousel__item:nth-child(2n) {
     background-color: #99a9bf;
  }
  
  .el-carousel__item:nth-child(2n+1) {
     background-color: #d3dce6;
  }
</style>