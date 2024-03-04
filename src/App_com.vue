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

<script>

/* vue3 Compositon API */
import { onMounted, ref } from 'vue';

/* axios */
import axios from 'axios';

/* ag-grid-vue */
import "ag-grid-community/styles/ag-grid.css";
import "ag-grid-community/styles/ag-theme-quartz.css";
import { AgGridVue } from "ag-grid-vue3";

export default {
  name: "App",
  components: {
    AgGridVue,
  },

  setup() {
    const rowData = ref([]);
    const colDefs = ref([
      { headerName: "개봉년도", field: "year", cellStyle:{"textAlign":"left", 'background-color': '#f1f7ff'}},
      { headerName: "영화제목", field: "title" },
      { headerName: "장르", field: "genres" },
      { headerName: "런타임 (min)", field: "runtime" },
    ]);

/* console.log('test', rowData.value.data.movies); */

  const fetchData = async() => {
    try {
        const response = await axios.get('https://yts.mx/api/v2/list_movies.json'); 
        rowData.value = response.data.data.movies.map(movie => ({
          title: movie.title,
          year: movie.year,
          genres: movie.genres,
          runtime: movie.runtime,
        }));
        console.log('test', rowData.value);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
  };
  
  onMounted(() => {
    fetchData();
  });
  
  /* console.log('test2',  rowData.value); */
  return {
    rowData,
    colDefs,
  };
},
};

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
