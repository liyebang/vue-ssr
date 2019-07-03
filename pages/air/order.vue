<template>
    <div class="container">
        <el-row type="flex" justify="space-between">
            <!-- 订单表单 -->
            <div class="main">
                <OrderForm 
                @setOrderinfo='setOrderinfo'
                @setTotal='setTotal'/>
            </div>

            <!-- 侧边栏 -->
            <div class="aside">
                <OrderAside :data='orderinfo' :total='total'/>  
            </div>
        </el-row>
    </div>
</template>

<script>
import OrderForm from '@/components/air/orderForm.vue';
import OrderAside from '@/components/air/orderAside.vue';

export default {
    data:function () {
        return {
            orderinfo:{
                seat_infos:{}
            },
            total: 0
        }
    },
    components:{
        OrderForm,
        OrderAside
    },
    methods:{
        setOrderinfo:function (obj) {
            this.orderinfo = obj;
        },
        setTotal:function (total) {
            this.total = total;
        }
    }
}
</script>

<style lang="less" scoped>
    .container{
        width:1000px;
        margin:20px auto;
    }
    
    /*aside*/
    .aside{
        width: 350px;
        height: fit-content;
        border:1px #ddd solid;
    }
</style>