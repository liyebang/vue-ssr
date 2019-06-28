<template>
  <div class="main">
    <!-- 录播图 -->
    <el-carousel height="700px">
      <el-carousel-item v-for="(item, index) in list" :key="index">
        <div
          class="carousel-img"
          :style="`
                background:url(${$axios.defaults.baseURL + item.url}) center center no-repeat;
                background-size:contain contain;
                `"
        ></div>
      </el-carousel-item>
    </el-carousel>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      list: [
        // 'http://157.122.54.189:9095/assets/images/th02.jfif',
        // 'http://157.122.54.189:9095/assets/images/th03.jfif',
        // 'http://157.122.54.189:9095/assets/images/th04.jfif'
      ]
    };
  },
  mounted() {
    this.$axios({
      url: "/scenics/banners",
      method: "GET"
    }).then(res => {
      // console.log(res);
      if (res.status === 200) {
        let { data } = res.data;
        // console.log(data);
        this.list = data;
      }
    });
  }
};
</script>

<style scoped lang='less'>
.main {
  min-width: 1000px;
  .carousel-img {
    width: 100%;
    height: 100%;
  }
}
</style>
