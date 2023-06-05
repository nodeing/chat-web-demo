<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import { NButton, NModal } from 'naive-ui'
import axios from 'axios'
import { useSettingStore } from '@/store'
// 接收属性
const props = defineProps({
  balanceModel: {
    type: Boolean,
    default: false,
  },
},
)
// 发射事件
const emit = defineEmits(['update:balanceModel'])
const balanceModel = computed({
  get: () => props.balanceModel,
  set: (value) => {
    emit('update:balanceModel', value)
  },
})
// 获取余额函数
const settingStore = useSettingStore()
const balance = ref(0)
const getBalanceHandle = async () => {
  const { data } = await axios.get('https://openai.api2d.net/dashboard/billing/credit_grants', {
    headers: {
      Authorization: `Bearer ${settingStore.systemKey}`,
    },
  })
  balance.value = data?.total_available
}
onMounted(() => {
  getBalanceHandle()
})
</script>

<template>
  <NModal
    v-model:show="balanceModel"
    class="custom-card"
    preset="card"
    title="余额查询"
    size="huge"
    :bordered="false"
    style="width: 50%;"
  >
    <div class="flex flex-col">
      <div class="flex items-center">
        <span class=" font-bold">余额:</span>
        <span class="mx-4 text-green-500">{{ balance }}P</span>
        <NButton @click="getBalanceHandle">
          刷新
        </NButton>
      </div>
      <div class=" py-4">
        <p class=" my-1">
          余额查询，请先设置key
        </p>
        <p class=" my-1">
          余额充值，联系微信：nodeing-cn
        </p>
      </div>
    </div>
  </NModal>
</template>
