<script setup lang="ts">
  const props = defineProps(['data', 'prefix', 'sufix', 'commas', 'radar', 'side', 'size', 'height', 'svg', 'svg_size']);

  function isNumber(value: any) {
    return !isNaN(parseFloat(value)) && isFinite(value);
  }
</script>

<template>
  <div :class="[height]" class="relative w-full flex justify-between">
    <span v-if="svg" :class="[side == 'right'? 'left-0': 'right-0', size]" class="flex justify-between items-end h-full absolute font-semibold text-gray-500">
      <component :class="[svg_size]" :is="svg" />
    </span>
    <span v-if="data" :class="[side == 'right'? 'right-0': 'left-0', size]" class="flex justify-between items-end h-full absolute font-semibold text-gray-500">
      <span :class="[size, side == 'right' ? 'right-0': 'left-0']">{{ commas && isNumber(data) ? Number(data).toLocaleString('en-US') : data }}</span>
    </span>
  </div>
</template>