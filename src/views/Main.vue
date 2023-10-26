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

  function get_hash(length: number) {
    const symbols = "!@#$%^&*()_+-=[]{}|;:',.<>?"; let result = '';
    const symbols_length = symbols.length;
    for (let i = 0; i < length; i++) {
      const random_index = Math.floor(Math.random() * symbols_length);
      result += symbols.charAt(random_index);
    }
    return result;
  }

  const responsive = () => {
    const breakpoints = [
      { width: 1436, cols: 10 },
      { width: 1320, cols: 9 },
      { width: 1024, cols: 7 },
      { width: 868, cols: 6 },
      { width: 768, cols: 5 },
      { width: 640, cols: 4 },
      { width: 540, cols: 3 },
      { width: 440, cols: 2 },
    ];

    const window_width = window.innerWidth; var cols_number = 1
    for (const breakpoint of breakpoints) {
      if (window_width >= breakpoint.width) {
        cols_number = breakpoint.cols; break;
      }
    }

    const data_length = Object.keys(data.value).length;
    cols.value = cols_number;
    rows.value = Math.ceil(data_length / cols.value);
    height.value = rows.value*120;

    const window_1 = document.getElementById("window-1");
    window_1!.style.height = `${height.value}px`;

    const window_2 = document.getElementById("window-2");
    window_2!.style.gridTemplateColumns = `repeat(${cols.value}, minmax(0, 1fr)`;
    window_2!.style.gridTemplateRows = `repeat(${rows.value}, minmax(0, 1fr)`;
  };
  
  onMounted(() => { responsive(); window.addEventListener('resize', responsive); });
  onBeforeUnmount(() => { window.removeEventListener('resize', responsive); });

  const cols = ref(1); const rows = ref(1); const height = ref(1);
  const route = useRoute();
  const commands = ['delimiter'];
  const data = ref<Object>(get_data(route.query, commands));
  const hash = ref(get_hash(5));
  const delimiter = String(route.query.delimiter);
</script>

<template>
  <div id="window-1" class="bg-white p-3" :style="`height:${height}px;`"> 
    <div id="window-2" class="w-full h-full grid gap-3">
      <div v-for="(value, key) in data" :key="key" class="cell bg-[#ECEFF1] border border-[#CFD8DC] col-span-1 row-span-1">
        <HeaderBlock :height="'h-10'" :title="String(value).replace(delimiter, hash).split(hash)[0]" :side="'left'" :size="'text-[16px]'"/>
        <DataBlock :height="'h-10'" :data="String(value).replace(delimiter, hash).split(hash)[1]" :commas="true" :side="'right'" :size="'text-[18px]'"/>
      </div>
    </div>
  </div>
</template>