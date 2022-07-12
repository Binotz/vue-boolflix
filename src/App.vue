<template>
  <div id="app">
    <!-- Header -->
    <header>
      <HeaderComponentVue @userInput="searchElement"/>
    </header>
    <!-- Main -->
    <main>
      <MainComponent :listOfResults="queryResults"/>
    </main>
  </div>
</template>

<script>
import HeaderComponentVue from './components/HeaderComponent.vue';
import MainComponent from './components/MainComponent.vue';

import axios from "axios";

export default {
  name: 'App',
  components: {
    HeaderComponentVue,
    MainComponent
},
  props:{},
  data(){
    return{
      API_KEY: '04ddd3cb54ab12bb2ff93493d1b1b8a5',
      baseQuery:['https://api.themoviedb.org/3/search/movie', 'https://api.themoviedb.org/3/search/tv'],
      userSearch: '',
      queryResults:[]
    }
  },
  methods:{
    searchElement: function(elem){
      this.queryResults = [];

      this.baseQuery.forEach((baseURL) =>{
          axios.get(baseURL, {
          params:{
            api_key: this.API_KEY,
            query: elem
          }
        })
        .then((resp)=>{
          //salvo tutti i risultati in un array
          resp.data.results.forEach(result => {
            this.queryResults.push(result);
          });
        })
        //catch di eventuali errori nella richiesta axios
        .catch(err => {
          console.log(err);
        });
      });
    }
  }
}
</script>

<style lang="scss">
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
</style>
