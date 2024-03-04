<template>
  <h1>AG-GRID-VUE</h1>
  <p>Option API</p>
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

/* axios */
import axios from 'axios';

/* ag-grid-vue */
import "ag-grid-community/styles/ag-grid.css";
import "ag-grid-community/styles/ag-theme-quartz.css";
import { AgGridVue } from 'ag-grid-vue3';

export default {
    name: "App",
    components: {
        AgGridVue
    },

    data() {
        return{
        rowData: [],
        colDefs: [
            { headerName: "개봉년도", field: "year", cellStyle:{"textAlign":"left", 'background-color': '#f1f7ff'}},
            { headerName: "영화제목", field: "title" },
            { headerName: "장르", field: "genres" },
            { headerName: "런타임 (min)", field: "runtime" },
            ]
        };
    },

    mounted() {
        this.fetchData();
    },

    methods: {
        async fetchData() {
        try {
            const response = await axios.get('https://yts.mx/api/v2/list_movies.json');
            this.rowData = response.data.data.movies.map((movie) => ({
                title: movie.title,
                year: movie.year,
                genres: movie.genres,
                runtime: movie.runtime,
            }))
        } catch (error) {
            console.log('error', error);
        }
        }
    }
}

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
