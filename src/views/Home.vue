<template>
  <div v-for="(item, index) in chainInfos" :key="index">
    <ChainContent :chainInfo="item" />
  </div>


</template>

<script lang="ts" setup>
import { reactive } from 'vue'
import { $axios } from '@/plugins/axios'
import ChainContent from '@/components/ChainContent.vue'
let chainInfos = reactive<any>([])
const getChainsData = async () => {
  let resp:any = await $axios.get("/period/chains")
  chainInfos.push(...resp.data.chainInfos)
};

getChainsData();

</script>


<style>
.ant-layout-header {
  font-size: 28px;
  background: #6aa0c7 !important;
}

.ant-layout-content {
  min-height: 120px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
