<template>
    <div>
        旅游首页
        <Digui :data='list'/>
    </div>
</template>

<script>
import Digui from '@/components/post/diugui.vue';
export default {
    data:function () {
      return {
          list:[
              {type: '电器', 
              children:[
                  {type: '电脑',children:[
                      {type:'笔记本'}
                  ]},
                  {type: '电冰箱'}
              ]},
              {type: '衣服'}
          ]
      }  
    },
    components:{
        Digui
    }
}
</script>

<style>

</style>
