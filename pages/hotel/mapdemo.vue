<template>
  <div>
    123
    <div id="container" class="map"></div>
    <div id="panel"></div>
    <div>
      <select v-model="type">
        <option value="Driving">驾车</option>
        <option value="Transfer">公交</option>
        <option value="Walking">步行</option>
      </select>
      <input type="text" placeholder="请输入查询出行的城市" v-model="city" />
      <input type="text" placeholder="请输入起点" v-model="start" />
      <input type="text" placeholder="请输入目的地" v-model="end" />
      <button @click="handleClick">查询</button>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      type: "Driving",
      city: "",
      start: "",
      end: "",
      map: null
    };
  },
  methods: {
    getTrip: function() {
      if (this.type === "Walking") {
        var trip = new AMap.Walking({
          map: this.map,
          panel: "panel"
        });
      }

      if (this.type === "Transfer") {
        var trip = new AMap.Transfer({
          // 驾车路线规划策略，AMap.DrivingPolicy.LEAST_TIME是最快捷模式
          policy: AMap.TransferPolicy.LEAST_TIME,
          map: this.map,
          panel: "panel"
        });
      }

      if (this.type === "Driving") {
        var trip = new AMap.Driving({
          // 驾车路线规划策略，AMap.DrivingPolicy.LEAST_TIME是最快捷模式
          policy: AMap.DrivingPolicy.LEAST_TIME,
          map: this.map,
          panel: "panel"
        });
      }

      var points = [
        { keyword: this.start, city: this.city },
        { keyword: this.end, city: this.city }
      ];


      trip.search(points, function(status, result) {
        // 未出错时，result即是对应的路线规划方案
        console.log(result);
      });
    },
    handleClick:function () {
        this.map = new AMap.Map('container', {
            zoom:11,//放大级别
        });
        document.querySelector('#panel').innerHTML = ''
        this.getTrip()
    }
  },
  mounted() {
    window.onLoad = function() {
      var map = new AMap.Map("container", {
        zoom: 11, //级别
        center: [116.397428, 39.90923], //中心点坐标
        viewMode: "3D" //使用3D视图
      });

      // 工具条插件
      var toolbar = new AMap.ToolBar();
      map.addControl(toolbar);

      var marker = new AMap.Marker({
        draggable: true,
        position: new AMap.LngLat(116.39, 39.9), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
        title: "北京123"
      });

      // 将创建的点标记添加到已有的地图实例：
      map.add(marker);
      this.map = map;
      //  var driving = new AMap.Driving({
      //     map: map,
      //     panel: "panel"
      // });

      //  driving.search([
      //     {keyword: '北京市地震局(公交站)',city:'北京'},
      //     {keyword: '亦庄文化园(地铁站)',city:'北京'}
      // ], function(status, result) {
      //     // result 即是对应的驾车导航信息，相关数据结构文档请参考  https://lbs.amap.com/api/javascript-api/reference/route-search#m_DrivingResult
      //     if (status === 'complete') {
      //         log.success('绘制驾车路线完成')
      //     } else {
      //         log.error('获取驾车数据失败：' + result)
      //     }
      // });

      //  map = new AMap.Map('container', {
      //     zoom:11,//放大级别
      // });

      // 查询驾车的路线
      // var driving = new AMap.Driving({
      //     // 驾车路线规划策略，AMap.DrivingPolicy.LEAST_TIME是最快捷模式
      //     policy: AMap.DrivingPolicy.LEAST_TIME,
      //     map: map,
      //     panel: "panel"
      // });

      // var points = [
      //     { keyword: '吉山幼儿园',city: '广州' },
      //     { keyword: '天河客运站',city:  '广州'  }
      // ]

      // driving.search(points, function (status, result) {
      //     // 未出错时，result即是对应的路线规划方案
      //     console.log(result);
      // })
    };

    // 引入高德地图的js文件,插件可以通过在地址栏凭借的方式添加
    var url = `https://webapi.amap.com/maps?v=1.4.15&key=${"763aebf096e7824f4d4e14237beddac0"}&callback=onLoad&plugin=AMap.ToolBar,AMap.Driving,AMap.Transfer,AMap.Walking`;
    var jsapi = document.createElement("script");
    jsapi.charset = "utf-8";
    jsapi.src = url;
    document.head.appendChild(jsapi);
  }
};
</script>

<style scoped lang='less'>
#container {
  width: 500px;
  height: 500px;
}

#panel {
  position: absolute;
  background-color: white;
  max-height: 400px;
  overflow-y: auto;
  top: 80px;
  right: 195px;
  width: 280px;
}
#panel .amap-call {
  background-color: #009cf9;
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
}
#panel .amap-lib-driving {
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
  overflow: hidden;
}
select {
  width: 50px;
}
</style>
