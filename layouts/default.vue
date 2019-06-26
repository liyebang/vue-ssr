<template>
  <div>
    <Header/>
    <!-- 内容占位 -->
    <nuxt />
    <Footer/>
  </div>
</template>

<script>
import Header from '@/components/header.vue';
import Footer from '@/components/footer.vue';
export default {
  components:{
    Header,
    Footer
  }
}
</script>



<style>
*{
  margin: 0;
  padding: 0;
}
a{
  color:  inherit;
  text-decoration: none;
}
</style>
