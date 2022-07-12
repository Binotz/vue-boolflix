<template>
  <div id="app">
    <!-- Header -->
    <header>
      <HeaderComponentVue @userInput="searchElement"/>
    </header>
    <!-- Main -->
    <main>
      <CardsComponentVue />

    </main>
  </div>
</template>

<script>
import HeaderComponentVue from './components/HeaderComponent.vue';
import CardsComponentVue from './components/CardsComponent.vue';

import axios from "axios";

export default {
  name: 'App',
  components: {
    HeaderComponentVue,
    CardsComponentVue
  },
  props:{},
  data(){
    return{
      API_KEY: '04ddd3cb54ab12bb2ff93493d1b1b8a5',
      userSearch: '',
      queryResults:[]
    }
  },
  methods:{
    searchElement: function(elem){
      axios.get(`https://api.themoviedb.org/3/search/movie`, {
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
