<script setup lang="ts">
  const props = defineProps(['title', 'side', 'size', 'height']);
</script>

<template>
  <div class="relative w-full" :class="[height, size]">
    <span :class="[side == 'right'? 'right-0': 'left-0']" class="absolute font-semibold text-[#546e7a] text-start line-clamp-3">{{ title }}</span>
  </div>
</template>