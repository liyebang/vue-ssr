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
                    <el-select size="mini" v-model="flightTimes" placeholder="起飞事件" @change="handleAirport">
                        <el-option
                        :label="`${item.from}:00 - ${item.to}:00`"
                        :value="item"
                        v-for="(item, index) in data.flightTimes"
                        :key="index"
                        >
                        </el-option>
                    </el-select>
                </el-col>
                <el-col :span="6">
                    <el-select size="mini" v-model="company" placeholder="航空公司" @change="handleAirport">
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
        }
    },

    methods: {
        // 选择机场时候触发
        handleAirport(value){
            
        },

        // 选择出发时间时候触发
        handleFlightTimes(value){
            
        },

         // 选择航空公司时候触发
        handleCompany(value){
            
        },

         // 选择机型时候触发
        handleAirSize(value){
           console.log(value)
        },
        
        // 撤销条件时候触发
        handleFiltersCancel(){
            
        },
    }
}
</script>

<style>

</style>
