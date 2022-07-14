<template>
  <div id="app">
    <!-- Header -->
    <header>
      <HeaderComponentVue @userInput="searchElement" @filter-genre="filterGenre" :listOfResults="queryResults"/>
    </header>
    <!-- Main -->
    <main>
      <MainComponent :listOfResults="filterResults" :errorMessage="errorMessage"/>
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
      filterGenreString:'All',
      genresList: [],
      queryResults:[]
    }
  },
  methods:{
    searchElement: function(elem){
      //reset dei risultati precedenti
      this.queryResults = [];
      this.errorMessage = '';
      
      //faccio una query API per i film e serie TV
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
            //creo una nuova key contenente le categorie 
            result.genres = this.mapGenre(result);
            //pusho l'elemento finale nell'array che passo ai vari components
            this.queryResults.push(result);
          });
        })
        //catch di eventuali errori nella richiesta axios
        .catch(err => {
          console.error(err.message);
          if(this.queryResults.length === 0){
            this.errorMessage = 'Nessun risultato!'
          }
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
          //Pusho tutti i generi in un array
          resp.data.genres.forEach(genre =>{
            this.genresList.push(genre);
          });
        })
      });
    },
    mapGenre: function(element){
      //salvo gli id dei generi dell'elemento passato
      let genresIDS = element.genre_ids;
      let mappedGenres = [];
      //controllo se sono presenti i generi 
      if (genresIDS.length > 0){
          //controllo gli id dell'elemento corrent
          genresIDS.forEach(genresID =>{
            //per ogni id controllo l'array di object con id:genere
            this.genresList.forEach(genre =>{
              //se l'id del genere dell'object e quello dell'array concidono, pusho il genere in un array
              if (genresID === genre.id){
                mappedGenres.push(genre.name);
              }
            });
          });
        //rimozione dupicati e ritorno l'array con i generi dell'object
        return [...new Set(mappedGenres)];
      }
    },
    filterGenre: function(filter){
      this.filterGenreString = filter;
    },
  },
  computed:{
    filterResults: function(){
      //se è presente il valore di default, non filtro niente
      if(this.filterGenreString === 'All'){
        return this.queryResults;
      }
      //altrimenti filtro i risultati i quali contengono nell'array dei generi, il genere selezionato
      return this.queryResults.filter(element =>{
        if(element.genres){
          return element.genres.includes(this.filterGenreString);
        }
      
      });
    },
  },
  mounted(){
    this.getGenres();
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap');

*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body{
  background-color: #222;
  font-family: 'Roboto', sans-serif;
}
</style>
