<template>
  <el-row class="flight-item">
    <div @click="handleShow">
      <el-row class="flight-info" type="flex" align="middle">
        <el-col :span="5">
            <span>{{data.airline_name}}</span>
            <span>{{data.flight_no}}</span>
        </el-col>
        <el-col :span="14">
          <el-col :span="8">
            <el-row class="flight-depart">
              <div style="font-size: 24px">{{ data.dep_time }}</div>
              <div class="info-message">{{ data.org_airport_name }}</div>
            </el-row>
          </el-col>
          <el-col :span="8" class="flight-time">
            <span>{{rankTime}}</span>
          </el-col>
          <el-col :span="8">
            <el-row class="flight-dest">
              <div style="font-size: 24px">{{ data.arr_time }}</div>
              <div class="info-message">{{ data.dst_airport_name }}</div>
            </el-row>
          </el-col>
        </el-col>
        <el-col :span="5">
          ￥
          <span class="info-price">{{data.base_price / 2}}</span>起
        </el-col>
      </el-row>
    </div>

    <!-- 隐藏的内容开始 -->
    <el-row class="flight-recommend" type="flex" align="middle" v-if="isShow">
      <el-col :span="6" class="flight-cheap">低价推荐</el-col>
      <!-- 低价推荐里面的内容开始 -->
      <el-col :span="18">
        <!-- 低价推荐里面的内容循环部分开始 -->
        <el-row 
        type="flex" 
        align="middle" 
        class="flight-sell"
        v-for="(item, index) in data.seat_infos"
        :key="index"
        >
          <el-col :span="14">
            <span>{{item.name}}</span> | {{item.supplierName}}
          </el-col>
          <el-col :span="5" class="recommend-price">￥{{item.org_settle_price}}</el-col>
          <el-col :span="5">
            <el-row>
              <div>
                <el-button type="warning" size="medium" @click="handleChoose(item)">选定</el-button>
              </div>
              <div class="recommend-surplus">剩余：83</div>
            </el-row>
          </el-col>
        </el-row>
        <!-- 低价推荐里面的内容循环部分结束 -->
      </el-col>

      <!-- 低价推荐里面的内容开始 -->
    </el-row>
    <!-- 隐藏的内容结束 -->
  </el-row>
</template>

<script>
export default {
  props:{
      data:{
          type: Object,
          // 默认是空对象
          default: {}
      }
  },
  data: function() {
    return {
      isShow: false
    };
  },
  methods: {
    handleShow: function() {
      this.isShow = !this.isShow;
    },
    handleChoose: function (item) {
      // console.log(item.seat_xid);
      // console.log(this.data.id);
      this.$router.push({
        // 跳转的页面路径
        path: "/air/order",
        query: {
            id: this.data.id,
            seat_xid: item.seat_xid
        }
      })
    }
  },
  computed:{
      rankTime(){
          const { dep_time, arr_time } = this.data;
          let arr = arr_time.split(":");
          const dep = dep_time.split(":");
          // 如果到达时间小于出发时间 - 就是第二天的时间
          if(arr[0] < dep[0]){
            arr[0] += 24;
          }
          const count = (arr[0] * 60 + +arr[1]) - (dep[0] * 60 + +dep[1]);
          const hours = Math.floor(count / 60);
          const min = count % 60;
          return `${hours}时${min}分钟`;
      }
  }
};
</script>

<style lang='less' scoped>
.flight-item {
  border: 1px solid #ddd;
  margin-bottom: 10px;
  .flight-info {
    text-align: center;
    padding: 15px;
    cursor: pointer;
    border-bottom: 1px solid #ddd;
    .flight-depart {
      text-align: right;
    }
    .flight-dest {
      text-align: left;
    }
    .flight-time {
      color: #999;
      font-size: 14px;
      span {
        padding: 10px 0;
        border-bottom: 1px solid #eee;
      }
    }
    .info-message {
      font-size: 12px;
      color: #999;
    }
    .info-price {
      font-size: 24px;
      color: orange;
      margin: 0 2px;
    }
  }
  .flight-recommend {
    padding: 0 20px;
    text-align: center;
    background-color: #f6f6f6;
    .flight-cheap {
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .flight-sell {
      box-sizing: border-box;
      padding: 10px 0;
      border-bottom: 1px solid #eee;
      &:last-child {
        border-bottom: none;
      }
      .recommend-price {
        font-size: 20px;
        color: orange;
      }
      .recommend-surplus {
        color: #666;
      }
      span {
        color: green;
      }
    }
  }
}
</style>
