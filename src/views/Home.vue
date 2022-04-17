<template>
  <div style="height: 60px;"></div>
  <div class="date" v-loading="state.loadingChainsTag">
    <el-row :gutter="20">
      <el-col :span="4"></el-col>

      <el-col :span="10" class="">
        <el-date-picker v-model="state.rangeDate" type="datetimerange" range-separator="To" start-placeholder="Start date" end-placeholder="End date" :clearable="false" :offset="-110"
          :show-arrow="false">
        </el-date-picker>
      </el-col>

      <el-col :span="6" class="">
        <el-button @click="reset">Reset</el-button>
        <el-button type="primary" @click="getChainsData">Apply</el-button>
      </el-col>

      <el-col :span="4"></el-col>
    </el-row>

  </div>



  <div class="chains" v-loading="state.loadingChainsTag">
    <template v-if="chainInfos.length > 0">
      <div v-for="(item, index) of chainInfos" :key="index">
        <ChainContent :chainInfo="item" />
      </div>
    </template>

    <el-empty v-else description="Empty"></el-empty>
  </div>
</template>

<script lang="ts" setup>
import { reactive } from 'vue'
import { $axios } from '@/plugins/axios'
import ChainContent from '@/components/ChainContent.vue'
const chainInfos = reactive<any>([])
const state = reactive({
  rangeDate: [] as Date[],
  loadingChainsTag: false
})
const reset = () => {
  const endTime = new Date()
  const startTime = new Date(endTime.getTime() - 43200000)
  state.rangeDate = [startTime, endTime]
}
reset()
const getChainsData = async () => {
  state.loadingChainsTag = true
  try {
    const params = {
      startTime: state.rangeDate[0].getTime(),
      endTime: state.rangeDate[1].getTime()
    }
    const resp: any = await $axios.get("/period/chains", { params })
    if (resp.status == 200 && resp.data && resp.data.chainInfos) {
      chainInfos.length = 0
      chainInfos.push(...resp.data.chainInfos)
      console.log(chainInfos)
    }
    state.loadingChainsTag = false
  } catch (error) {
    state.loadingChainsTag = false
    console.log(error)
  }

};
getChainsData();
</script>

<style lang="scss">
.date {
  width: 100%;
  text-align: center;
  background-color: #f8f8f8;
  padding: 10px 0;
}

.chains {
  background-color: #f8f8f8;
  margin: 10px 0;
  padding: 10px 0;
}
</style>
