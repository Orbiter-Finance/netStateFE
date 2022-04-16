<template>

  <div class="chain-info">
    <div class="chain-name">{{ chainInfo.chainName }}:{{ chainInfo.apiUrl }}</div>

    <div class="chain-timeline">
      <template v-for="(item, index) in chainInfo.netstatList" :key="index">

        <a-tooltip color="#2b2929">
          <template #title><span style=" white-space: pre-line;">blockNumber:{{ item.last_block_num }}<br />time:{{ formatDate(item.created_at) }}</span></template>
          <div class="chain-item" :style="{ 'background-color': item.ten_minite_net_state ? 'orange' : 'green' }"></div>
        </a-tooltip>

      </template>
    </div>
    <div class="chain-description">
      <span>{{ formatDate(chainInfo.netstatList[0].created_at) }}</span>
      <div class="decorate"></div>
      <span>10 minute period</span>
      <div class="decorate"></div>
      <span>latest</span>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { defineProps } from "vue"
defineProps({
  chainInfo: {
    type: Object,
    required: true,
  }
})
function formatDate(timeStamp: string) {
  try {
    const date = new Date(Number(timeStamp))
    var year = date.getFullYear();
    var month = date.getMonth() + 1;
    var day = date.getDate();
    var hour = date.getHours();
    var minute = date.getMinutes();
    var second = date.getSeconds();
    return `${year}-${formatTen(month)}-${formatTen(day)} ${hour}:${minute}:${second}`;
  } catch (error) {
    return "0"
  }

}
function formatTen(num: number) {
  return num > 9 ? (num + "") : ("0" + num);
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.chain-info {
  width: 70%;
  height: 200px;
  text-align: center;
  margin: 10px auto;
  padding: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: flex-start;
  border-radius: 20px;
  border: 1px #fdd3d3 solid;
}

.chain-name {
  font-size: 22px;
}

.chain-timeline {
  width: 100%;
  text-align: center;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 20px;
}

.chain-item {
  width: 7px;
  height: 60px;
  display: inline-display;
}

.chain-description {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.decorate {
  width: calc(50% - 200px);
  height: 2px;
  background-color: #9c9a9a;
}
</style>
