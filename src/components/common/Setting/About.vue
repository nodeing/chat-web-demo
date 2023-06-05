<script setup lang='ts'>
import { onMounted, ref } from 'vue'
import { NSpin } from 'naive-ui'
import { fetchChatConfig } from '@/api'

interface ConfigState {
  timeoutMs?: number
  reverseProxy?: string
  apiModel?: string
  socksProxy?: string
  httpsProxy?: string
  usage?: string
}

const loading = ref(false)

const config = ref<ConfigState>()

async function fetchConfig() {
  try {
    loading.value = true
    const { data } = await fetchChatConfig<ConfigState>()
    config.value = data
  }
  finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchConfig()
})
</script>

<template>
  <NSpin :show="loading">
    <!-- 付费说明 -->
    <div class="p-4 space-y-4">
      <h2 class="text-xl font-bold">
        付费说明
      </h2>
      <div class="p-2 space-y-2 rounded-md bg-neutral-100 dark:bg-neutral-700">
        <p>
          OpenAI官网是基于token来收费的，即按照你每次发送的请求的内容和返回的内容来收费，你发送的内容越多，返回的答案越长，消耗的token数就越多
        </p>
        <p>
          我们这边是做了一层代理转发，你的客户端是直接使用的OpenAI返回的数据，和官网返回内容一致的
        </p>
        <p>
          使用门槛比较低，充值门槛比较低，支持微信充值，请联系微信：nodeing-cn
        </p>
      </div>
    </div>
  </NSpin>
</template>
