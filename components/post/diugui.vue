<template>
    <div>

        <div v-for="(item, index) in data" :key="index">
            {{item.type}}
            <digui :data='item.children' v-if="item.children"/>
                
        </div>

    </div>
</template>

<script>
export default {
    name: 'digui',
    props:{
        data:{
            type: Array,
            default:[]
        }
    }
}
</script>

<style>

</style>
