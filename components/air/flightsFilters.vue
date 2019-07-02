<template>
    <div class="filters">
        <el-col :span="8">
            <div>
                单程： 
                {{this.$route.query.departCity}} - {{this.$route.query.destCity}} / {{this.$route.query.departDate}}
            </div>
        </el-col>
        <el-col :span="16">
            <el-row type='flex' justify="space-between" align="middle" class="filters-top" >
                <el-col :span="6">
                    <el-select size="mini" v-model="airport" placeholder="起飞机场" @change="handleAirport">
                        <el-option
                        :label="item"
                        :value="item"
                        v-for="(item, index) in data.airport"
                        :key="index"
                        >
                        </el-option>
                    </el-select>
                </el-col>
                <el-col :span="6">
                    <el-select size="mini" v-model="flightTimes" placeholder="起飞时间" @change="handleFlightTimes">
                        <el-option
                        v-for="(item, index) in data.flightTimes"
                        :key="index"
                        :value="`${item.from}:00 - ${item.to}:00`"
                        :label="`${item.from}:00 - ${item.to}:00`"
                        >
                        </el-option>
                    </el-select>
                </el-col>
                <el-col :span="6">
                    <el-select size="mini" v-model="company" placeholder="航空公司" @change="handleCompany">
                        <el-option
                        :label="item"
                        :value="item"
                        v-for="(item, index) in data.company"
                        :key="index"
                        >
                        </el-option>
                    </el-select>
                </el-col>
                <el-col :span="6">
                    <el-select size="mini" v-model="airSize" placeholder="机型" @change="handleAirSize">
                        <el-option
                        :label="item.name"
                        :value="item.type"
                        v-for="(item, index) in airSizeList"
                        :key="index"
                        >
                        </el-option>
                    </el-select>
                </el-col>
            </el-row>
        </el-col>
        <div>
            筛选：
            <el-button 
            type="primary" 
            round 
            plain 
            size="mini" 
            @click="handleFiltersCancel">
                撤销
    		</el-button>
        </div>
    </div>
</template>

<script>
export default {
data(){
        return {
            airport: "",        // 机场
            flightTimes: "",    // 出发时间
            company: "",        // 航空公司
            airSize: "",        // 机型大小

            // 机型的大小选项
            airSizeList: [
                { name:"大", type: "L" },
                { name:"中", type: "M" },
                { name:"小", type: "S" },
            ]
        }
    },

    props: {
        // 接收外部的对象数据
        data: {
            type: Object,
            default: {}
        },
        // 接收外部的数组
        initFlights:{
            type: Array,
            default: []
        }
    },

    methods: {
        // 选择机场时候触发
        handleAirport(value){
            // console.log(this.initFlights);
            var arr = this.initFlights.filter( v => {
                return v.org_airport_name === value
            })
            this.$emit('dataListChange',arr);
        },

        // 选择出发时间时候触发
        handleFlightTimes(value){
            // console.log(+value.substring(0,value.indexOf(':')));
            // console.log(+value.substring(value.lastIndexOf('- ')+1,value.lastIndexOf(':')));
            var start = +value.substring(0,value.indexOf(':'));
            var end = +value.substring(value.lastIndexOf('- ')+1,value.lastIndexOf(':'));
            // console.log(+this.initFlights[0].dep_time.substring(0,this.initFlights[0].dep_time.indexOf(':')));
            var arr = this.initFlights.filter( v => {
                return (+v.dep_time.substring(0, v.dep_time.indexOf(':'))) >= start && (+v.dep_time.substring(0, v.dep_time.indexOf(':'))) < end
            })
            this.$emit('dataListChange',arr);
        },

         // 选择航空公司时候触发
        handleCompany(value){
            // console.log(value);
            // console.log(this.initFlights);
            var arr = this.initFlights.filter( v => {
                return v.airline_name === value
            })
            // console.log(arr);
            this.$emit('dataListChange',arr);
        },

         // 选择机型时候触发
        handleAirSize(value){
            var arr = this.initFlights.filter( v => {
                return v.plane_size === value
            })
            this.$emit('dataListChange',arr);
        },
        
        // 撤销条件时候触发
        handleFiltersCancel(){
            this.airport = "";        
            this.flightTimes = "";   
            this.company = "";        
            this.airSize = ""
            this.$emit('dataListChange',this.initFlights);
        },
    },
    watch:{
        $route(){
            this.airport = "";        
            this.flightTimes = "";   
            this.company = "";        
            this.airSize = ""
        }
    }
}
</script>

<style>

</style>
