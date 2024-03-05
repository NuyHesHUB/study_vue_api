<template>
  <h1>AG-GRID-VUE</h1>
  <p>Compositon API</p>
  <div style="width: 100%">
    <ag-grid-vue
          :key="index"
          :rowData="rowData"
          :columnDefs="colDefs"
          :style="{width: '850px', height: '500px', margin: '0 auto'}"
          class="ag-theme-quartz"
      >
      </ag-grid-vue>
  </div>
</template>

<script setup>

import axios from 'axios';
import { onMounted, ref } from 'vue';
import { AgGridVue } from 'ag-grid-vue3';
import "ag-grid-community/styles/ag-grid.css";
import "ag-grid-community/styles/ag-theme-quartz.css";

const rowData = ref([]);
const colDefs = ref([
      { headerName: "개봉년도", field: "year", cellStyle:{"textAlign":"left", 'background-color': '#f1f7ff'}},
      { headerName: "영화제목", field: "title" },
      { headerName: "장르", field: "genres" },
      { headerName: "런타임 (min)", field: "runtime" },
]);

const fetchData = async() => {
  try {
    const response = await axios.get("https://yts.mx/api/v2/list_movies.json");
    rowData.value = response.data.data.movies.map(movie => ({
      title: movie.title,
      year: movie.year,
      genres: movie.genres,
      runtime: movie.runtime
    }));
    console.log('test', rowData.value);
  } catch (error) {
    console.error('error', error);
  }
}

onMounted(() => {
  fetchData();
});

</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
