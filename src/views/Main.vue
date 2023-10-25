<script setup lang="ts">
  import { ref, onMounted, onBeforeUnmount } from 'vue';
  import { useRoute } from 'vue-router';

  import HeaderBlock from '../components/HeaderBlock.vue';
  import DataBlock from '../components/DataBlock.vue';

  const get_data = (query: any, commands: any) => {
    const data = Object.fromEntries(
      Object.entries(query).filter(([key]) => !commands.includes(key)).sort((a, b) => a[0].localeCompare(b[0]))
    );
    return data;
  };

  const calculate_cols = () => {
    const window_width = window.innerWidth; let col_num = 1;
    if (window_width >= 1436) { col_num = 10; } 
    else if (window_width >= 1320) { col_num = 9; } 
    else if (window_width >= 1024) { col_num = 7; } 
    else if (window_width >= 868) { col_num = 6; } 
    else if (window_width >= 768) { col_num = 5; } 
    else if (window_width >= 640) { col_num = 4; } 
    else if (window_width >= 540) { col_num = 3; } 
    else if (window_width >= 440) { col_num = 2; } 
    cols.value = col_num; rows.value = Math.ceil(Object.keys(data.value).length / col_num);
    const main_1 = document.getElementById("main-1");
    main_1!.style.height = `${rows.value*120}px`;
    const main_2 = document.getElementById("main-2");
    main_2!.style.gridTemplateColumns = `repeat(${cols.value}, minmax(0, 1fr))`; 
    main_2!.style.gridTemplateRows = `repeat(${rows.value}, minmax(0, 1fr))`; 
  }
  
  onMounted(() => { calculate_cols(); window.addEventListener('resize', calculate_cols); });
  onBeforeUnmount(() => { window.removeEventListener('resize', calculate_cols); });

  const cols = ref(1); const rows = ref(1);
  const route = useRoute();
  const commands = ['delimiter'];
  const data = ref<Object>(get_data(route.query, commands));
  const delimiter = String(route.query.delimiter);
</script>

<template>
  <div id="main-1" class="bg-white p-3" :style="`height:${rows*120}px;`"> 
    <div id="main-2" class="w-full h-full grid gap-3">
      <div v-for="(value, key) in data" :key="key" class="cell bg-[#ECEFF1] border border-[#CFD8DC] col-span-1 row-span-1">
        <HeaderBlock :height="'h-10'" :title="String(value).split(delimiter)[0]" :side="'left'" :size="'text-[16px]'"/>
        <DataBlock :height="'h-10'" :data="String(value).split(delimiter)[1]" :side="'right'" :size="'text-[22px]'"/>
      </div>
    </div>
  </div>
</template>

<!-- http://localhost:5173/?delimiter=-&a=Test-40&b=Test-9&c=Test-12&d=Test-12&e=Test-40&f=Test-90&g=Test-9&h=Test-12&i=Test-12&j=Test-40&k=Test-9 -->

<!-- https://just-bax.github.io/ov-analytical-blocks/?delimiter=-&a=Test-40&b=Test-9&c=Test-12&d=Test-12&e=Test-40&f=Test-90&g=Test-9&h=Test-12&i=Test-12&j=Test-40&k=Test-9 -->