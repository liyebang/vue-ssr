<template>
  <section class="container">
    <h2 class="air-title"><span class="iconfont iconfeiji"></span>
      <i>国内机票</i>
    </h2>

    <!-- 搜索广告栏 -->
    <el-row type="flex" justify="space-between">
        <!-- 搜索表单 -->
        <!-- <div>搜索</div> -->
        <SearchForm/>

        <!-- banner广告 -->
        <div class="sale-banner">
            <img src="http://157.122.54.189:9093/images/pic_sale.jpeg">
        </div>
    </el-row>

    <!-- 广告 -->
    <el-row type="flex" class="statement">
      <el-col :span="8">
        <i class="iconfont iconweibiaoti-_huabanfuben" style="color:#409EFF;"></i>
        <span>100%航协认证</span>
      </el-col>
      <el-col :span="8">
        <i class="iconfont iconbaozheng" style="color:green;"></i>
        <span>出行保证</span>
      </el-col>
      <el-col :span="8">
        <i class="iconfont icondianhua" style="color:#409EFF;"></i>
        <span>7x24小时服务</span>
      </el-col>
    </el-row>

    <h2 class="air-sale-title">
      <span class="iconfont icontejiajipiao"></span>
      <i>特价机票</i>
    </h2>

    <!-- 特价机票 -->
    <div class="air-sale">
        <div 
        class='air-sale-item'
        v-for="(item, index) in salelist"
        :key="index"
        @click="handleClick(item)">
          <img :src="item.cover" alt="">
          <div class="air-sale-bottom">
            <div>
              {{item.departCity}} - {{item.destCity}}
            </div>
            <div class="air-sale--price">
              ￥{{item.price}}
            </div>
          </div>
        </div>
    </div>
  </section>
</template>

<script>
//引入搜索机票的组件
import SearchForm from '@/components/air/searchForm.vue';

export default {
    components:{
        SearchForm
    },
    data:function () {
      return {
        salelist:[]
      }
    },
    mounted(){
      this.$axios({
        url: '/airs/sale',
        mounte: 'GET'
      }).then( res => {
        // console.log(res);
        let { data } = res.data;
        // console.log(data);
        this.salelist = data;
      })
    },
    methods:{
      handleClick: function (item) {
        // console.log(item);
        this.$router.push(`/air/flights?departCity=${item.departCity}&departCode=${item.departCode}&destCity=${item.destCity}&destCode=${item.destCode}&departDate=${item.departDate}`)
      }
    }
}
</script>

<style scoped lang="less">
.air-sale{
  border: 1px #ddd solid;
  padding:20px;
  margin-bottom:50px;

  .air-sale-pic{
    > div{
      width:225px;
      height:140px;
      position: relative;
      overflow: hidden;

      img{
        width:100%;
      }

      .layer-bar{
        position:absolute;
        bottom:0;
        left:0;
        background: rgba(0,0,0,0.5);
        color:#fff;
        height:30px;
        line-height: 30px;
        width:100%;
        box-sizing: border-box;
        padding: 0 15px;
        font-size: 14px;

        span:last-child{
          font-size:18px;
        }
      }
    }
  }
}

.air-sale-group{
  margin-top:20px;
  padding-top:8px;
  border-right:1px #eee solid;

  &:last-child{
    border-right:none;
  }

  .air-sale-row{
    font-size:12px;
    color:#666;
    margin-bottom:8px;

    .air-sale-price{
      color:orange;
      font-size: 20px;
    }
  }
}

.container{
  width:1000px;
  margin:0 auto;
}

.air-title{
  margin:15px 0;
  font-size:20px;
  font-weight: normal;
  color:orange;

  span{
    font-size:20px;
  }
}

.statement{
  margin:15px 0;
  border:1px #ddd solid;
  background:#f5f5f5;
  height: 58px;
  padding:10px 0;
  box-sizing:border-box;

  > div{
    text-align: center;
    line-height: 38px;
    border-right:1px #ddd solid;

    &:last-child{
      border-right: none;
    }

    *{
      vertical-align: middle;
    }

    i{
      font-size:30px;
    }
  }
}

.air-sale-title{
  margin:15px 0;
  font-size:20px;
  font-weight: normal;
  color:#409EFF;

  span{
    font-size:20px;
  }
}

.air-sale{
  display: flex;
  justify-content: space-between;
  .air-sale-item{
    width: 225px;
    height: 141px;
    overflow: hidden;
    position: relative;
    cursor: pointer;
    img{
      width: 100%;
    }
    .air-sale-bottom{
      position: absolute;
      left: 0;
      bottom: 0;
      width: 195px;
      display: flex;
      justify-content: space-between;
      background-color: rgba(0,0,0,.4);
      padding: 0 15px;
      height: 30px;
      line-height: 30px;
      font-size: 14px;
      color: #fff;
      .air-sale--price{
        font-size: 18px;
        font-weight: 700;
      }
    }
  }
}


</style>