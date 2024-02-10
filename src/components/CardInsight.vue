<script setup>
import { Icon } from '@iconify/vue';

defineProps({
  status: String,
  total: {
    type: Number,
    default: 0
  },
  percentage: {
    type: Number,
    default: 0
  },
  chartType: {
    type: String,
    default: 'up'
  },
  loading: Boolean
})
</script>

<template>
  <div class="animate-pulse" v-if="loading">
    <div class="rounded-lg bg-gray-200 h-40 w-full" />
  </div>
  <div class="border rounded-lg bg-white" v-else>
    <div class="p-6">
      <div class="flex flex-row gap-4 justify-between">
        <div class="wrapper-caption">
          <p class="text-sm text-[#727272] mb-2">
            {{ status }}
          </p>
          <p class="text-xl md:text-2xl font-semibold text-[#080808]">
            {{ total.toLocaleString('en') }}
          </p>
        </div>
        <div class="wrapper-icon">
          <div class="p-1.5 bg-[#F2F4F7] text-[#817F7F] rounded-md">
            <Icon icon="solar:square-transfer-horizontal-broken" width="20" height="20" />
          </div>
        </div>
      </div>
    </div>
    <div class="px-6 py-4 bg-[#F9FAFC]">
      <div class="flex flex-row gap-2 justify-between items-center">
        <p class="text-xs inline-flex items-center">
          <span :class="`${chartType === 'up' ? 'text-green-500 rotate-45' : 'text-red-500 -rotate-45'}`" class="mr-1">
            <Icon icon="ph:arrow-up-bold" v-if="chartType === 'up'" width="18" height="18" />
            <Icon icon="ph:arrow-down-bold" v-if="chartType === 'down'" width="18" height="18" />
          </span>
          <span class="mr-1" :class="`${chartType === 'up' ? 'text-green-500' : 'text-red-500'}`">
            {{ percentage.toFixed(2) }}%
          </span>
          <span class="text-[#AAAAAA]">Last Month</span>
        </p>
        <p class="text-xs text-[#6E6E6E] hover:underline cursor-pointer">View More</p>
      </div>
    </div>
  </div>
</template>