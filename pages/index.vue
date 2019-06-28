<template>
  <div class="main">

    <!-- 录播图 -->
    <!-- <el-carousel height="700px">
      <el-carousel-item v-for="(item, index) in list" :key="index">
        <div class="carousel-img"
        :style="`
        background:url(${$axios.defaults.baseURL + item.url}) center center no-repeat;
        background-size:contain contain;
        `"></div>
      </el-carousel-item>
    </el-carousel> -->
    <Carousel/>

    <!-- 搜索框 -->
    <div class="search-tool">
      <el-row class="search-button">
        <el-row type="flex">
          <div 
          class="button" 
          ref="button"
          v-for='(item, index) in options'
          :key="index"
          @click="handleClick(index)">
            <div :class="{'trapezoid-choice':index === currentOption, 'trapezoid-unchoice':index !== currentOption }"></div>
            <div :class="{'prompt-choice':index === currentOption, 'prompt-unchoice':index !== currentOption}">{{item.name}}</div>
          </div>
        </el-row>
        <el-row class="search-input" type="flex" >
          <input type="text" :placeholder="options[currentOption].placeholder" >
          <i class="el-icon-search"></i>
        </el-row>
      </el-row>
    </div>

  </div>
</template>

<script>
//引入轮播组件
import Carousel from '@/components/carousel';

export default {
  data:function () {
    return {
      // list:[
      //   // 'http://157.122.54.189:9095/assets/images/th02.jfif',
      //   // 'http://157.122.54.189:9095/assets/images/th03.jfif',
      //   // 'http://157.122.54.189:9095/assets/images/th04.jfif'
      // ],

      //搜索框相关提示
      options: [
        { 
          name: "攻略", 
          placeholder: "搜索城市"
        },
        {
          name: "酒店", 
          placeholder: "请输入城市搜索酒店",
        },
        {
          name: "机票", 
          placeholder: "请输入出发地"
        }
      ],
            
      // 搜索框当前高亮的tab栏
      currentOption: 0
    }
  },
  // mounted(){
  //   this.$axios({
  //     url:'/scenics/banners',
  //     method:'GET'
  //   }).then(res => {
  //     // console.log(res);
  //     if(res.status === 200){
  //       let { data } = res.data;
  //       // console.log(data);
  //       this.list = data;
  //     }
  //   })
  // },
  methods:{
    handleClick:function (index) {
      // console.log(index);
      if(index !== 2){
        this.currentOption = index;
      }else {
        this.$router.push('/air');
      }
    }
  },
  components:{
    Carousel
  }
}
</script>

<style lang='less' scoped>
.main{
  position: relative;
  min-width: 1000px;
  // .carousel-img{
  //   width: 100%;
  //   height: 100%;
  // }
  .search-tool{
    z-index: 999;
    width: 552px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translateX(-50%) translateY(-50%);
    .search-button{
      .button{
        width: 90px;
        height: 42px;
        cursor: pointer;
        position: relative;
        .trapezoid-choice{
          height: 0;
          width: 75px;
          border-bottom: 42px solid #eee;
          border-right: 15px solid transparent;
        }
        .trapezoid-unchoice{
          height: 0;
          width: 75px;
          border-bottom: 42px solid rgba(0,0,0,.3);
          border-right: 15px solid transparent;
        }
        .prompt-choice{
          position: absolute;
          top: 0;
          left: 0;
          width: 90px;
          height: 42px;
          line-height: 42px;
          text-align: center;
          z-index: 1;
          font-size: 16px;
          color: #333;
        }
        .prompt-unchoice{
          position: absolute;
          top: 0;
          left: 0;
          width: 90px;
          height: 42px;
          line-height: 42px;
          text-align: center;
          z-index: 1;
          font-size: 16px;
          color: #fff;
        }
      }
    }
    .search-input{
      width: 552px;
      input{
        border: none;
        outline: none;
        width: 478px;
        height: 20px;
        padding: 13px 15px;
        background-color: #fff;
        border-radius: 0 0 0 5px;
        font-size: 16px;
      }
      i{
        cursor: pointer;
        text-align: center;
        width: 44px;
        height: 46px;
        line-height: 46px;
        background-color: #FFF;
        border-radius: 0 5px 5px 0;
        font-size: 22px;
      }
    }
  }
}

//组件样式
.el-carousel__item:nth-child(2n) {
     background-color: #99a9bf;
  }
  
  .el-carousel__item:nth-child(2n+1) {
     background-color: #d3dce6;
  }
</style>
