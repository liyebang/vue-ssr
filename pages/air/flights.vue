<template>
    <el-row class="main" type="flex" justify="space-between">
        <el-row class="main-left">

            <!-- 左边头部 -->
            <el-row class="main-left-header">

                
                <FlightsFilters 
                :data='options' 
                :initFlights='initFlights'
                @dataListChange='dataListChange'/>
                    

                <!-- 列表头部 -->
                <FightsListHead style='margin: 20px 0 5px 0' v-if="total !== 0"/>

                <div v-if="total === 0" style="text-align: center">
                    暂无航班信息
                </div>

                <!-- 机票列表 -->
                <FlightsItem
                v-for="(item, inde) in dataList"
                :key="inde"
                :data='item'
                />

                <!-- 分页工具 -->
                <el-pagination  
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="currentPage"
                :page-sizes="[5, 10, 15, 20]"
                :page-size="pageSize"
                layout="total, sizes, prev, pager, next, jumper"
                :total="total"
                class="pagination">
                </el-pagination>

            </el-row>
        </el-row>
        <el-row class="main-right">
            <FlightsAside/>
        </el-row>
    </el-row>
</template>

<script>
import FightsListHead from '@/components/air/fightsListHead.vue';
import FlightsItem from '@/components/air/flightsItem.vue';
import FlightsFilters from '@/components/air/flightsFilters.vue';
import FlightsAside from '../../components/air/flightsAside.vue';

export default {
    data:function () {
        return {
            //机票展示列表
            flights:[],
            options:{},
            currentPage: 1,
            pageSize: 5,
            total: 0,
            dataList: [],  // 分页之后的数据列表
            initFlights:[] //原始的机票数据
        }
    },
    components:{
        FightsListHead,
        FlightsItem,
        FlightsFilters,
        FlightsAside
    },
    mounted(){
        this.$axios({
            url: '/airs',
            params: this.$route.query,
            method: 'GET'
        }).then( res => {
            // console.log(res);
            let { flights, total, options } = res.data;
            this.flights = flights;
            this.total = total;
            this.options = options;
            this.initFlights = flights;
            // console.log(this.flights);
            // this.dataList = this.flights.slice(
            //     (this.currentPage - 1) * this.pageSize,
            //     this.currentPage * this.pageSize
            // );
            this.dataListChange();
        })
    },
    methods:{
        dataListChange(arr){
            if(arr){
                this.flights = arr;
                this.total = arr.length;
                this.currentPage = 1;
            }
            this.dataList = this.flights.slice(
                (this.currentPage - 1) * this.pageSize,
                this.currentPage * this.pageSize
            );
        },
        handleSizeChange(val) {
        this.pageSize = val;
        this.dataListChange();
      },
      handleCurrentChange(val) {
        this.currentPage = val;
        this.dataListChange();
      }
    },
    watch:{
        $route(){
            this.$axios({
            url: '/airs',
            params: this.$route.query,
            method: 'GET'
        }).then( res => {
                // console.log(res);
                let { flights, total, options } = res.data;
                this.flights = flights;
                this.total = total;
                this.options = options;
                this.initFlights = flights;
                this.currentPage = 1;
                // console.log(this.flights);
                // this.dataList = this.flights.slice(
                //     (this.currentPage - 1) * this.pageSize,
                //     this.currentPage * this.pageSize
                // );
                this.dataListChange();
            })
        }
    }
}
</script>

<style scoped lang='less'>
.main{
    width: 1000px;
    margin: 20px auto;
    .main-left{
        width: 745px;
        .main-left-header{
            font-size: 14px;
        }
    }
    .main-right{
        width: 240px;
    }
    .pagination{
        text-align: center;
    }
}
</style>
