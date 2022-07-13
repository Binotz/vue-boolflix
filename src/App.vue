<template>
  <div id="app">
    <!-- Header -->
    <header>
      <HeaderComponentVue @userInput="searchElement"/>
    </header>
    <!-- Main -->
    <main>
      <MainComponent :listOfResults="queryResults" :errorMessage="errorMessage"/>
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
      userSearch:'',
      errorMessage: '',
      genresList: [],
      queryResults:[]
    }
  },
  methods:{
    searchElement: async function(elem){
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
          resp.data.results.forEach(async (result) => {
            //inizio BONUS:
            //recupero l'id del singolo risultato e facendo un'altra chiamata API ne recuopero il cast
            let queryURL = `https://api.themoviedb.org/3/movie/${result.id}/credits`;
            await axios.get(queryURL,{
              params:{
                api_key: this.API_KEY
              }
            })
            .then(results=>{
              //prendo i primi 5 attori/attrici del cast
              let cast = results.data.cast.slice(0,5);
              let castList = [];
              //pusho gli attori in un array di appoggio
              cast.forEach( act => {
                castList.push(act.name);
              });
              //creo una nuova chiave valore nell'oggetto, che contiene l'array del cast (primi 5)
              result.castName = castList;
            })
            .catch(err =>{
              console.error(err.message);
              result.castName = [];
            });
            result.genres = this.mapGenre(result);
            //pusho l'elemento finale nell'array che passo ai vari components
            console.log(result)
            this.queryResults.push(result);
          });
        })
        //catch di eventuali errori nella richiesta axios
        .catch(err => {
          this.errorMessage = err.message;
        });
      });
    },
    getGenres: function(){
      //recupero i generi sia per i film che per le serie TV
      const genresURL = 
      ['https://api.themoviedb.org/3/genre/movie/list?api_key=04ddd3cb54ab12bb2ff93493d1b1b8a5',
      'https://api.themoviedb.org/3/genre/tv/list?api_key=04ddd3cb54ab12bb2ff93493d1b1b8a5'];
      genresURL.forEach(async genreURL =>{
        await axios.get(genreURL,{
          params:{
            api_key: this.API_KEY
          }
        })
        .then(resp =>{
          resp.data.genres.forEach(genre =>{
            this.genresList.push(genre);
          });
        })
      });
    },
    mapGenre: function(element){
      let genresIDS = element.genre_ids;
      let mappedGenres = [];
      if (genresIDS.length > 0){
          console.log(element.name, element.genre_ids);
          genresIDS.forEach(genresID =>{
            this.genresList.forEach(genre =>{
              if (genresID === genre.id){
                //element.genres.push(genre.name);
                mappedGenres.push(genre.name);
              }
            });
          });
        //rimozione dupicati
          return [...new Set(mappedGenres)];
      }
    }
  },
  mounted(){
    this.getGenres();
  }
}
</script>

<style lang="scss">
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body{
  background-color: #222;
}
</style>
