<template>
  <div class="header">
    <div class="logo">Boolflix</div>
    <div class="search-form">
        <select v-if="listOfResults.length > 0" name="genres-list" id="genres-list" v-model="filteredGenre" @change="filterGenre">
            <option v-for="genre, index in listOfGenres" :key="index"
            :value="genre"> {{genre}} </option>
        </select>
        <input type="text" id="SearchText" v-model="searchText" @keyup.enter="search()">
        <button @click="search()">Cerca</button>
    </div>
  </div>
</template>

<script>
export default {
    name:"HeaderComponent",
    props:{
        listOfResults: Array
    },
    data(){
        return{
            searchText: '',
            genreListArray:[],
            filteredGenre: 'All',
        }
    },
    methods:{
        search: function(){
            //al click, passo al padre il testo cercato e resetto il filtro per genere
            this.$emit('userInput', this.searchText);
            this.$emit('filter-genre', 'All');
        },
        filterGenre: function(){
            this.$emit('filter-genre', this.filteredGenre);
        }
    },
    computed:{
        listOfGenres: function(){
            let tmpGenre = [];
            let genresList = [];
            tmpGenre.push('All');
            //Se ci sono risultati...
            if(this.listOfResults.length > 0){
                this.listOfResults.forEach(result =>{
                    //... e questi risultati hanno un genere...
                    if(result.genres){
                        //...pusho ogni genere in un array di appoggio...
                        result.genres.forEach(genre =>{
                            tmpGenre.push(genre);
                        });
                    } else {
                        //...se non ci sono generi, metto un valore di default
                        tmpGenre.push('N/A');
                    }
                });
            }
            // rimuovo i duplicati e ritorno l'array
            genresList = [...new Set(tmpGenre)];
            return genresList;
        }
    }
}
</script>

<style lang="scss" scoped>
    input{
        margin-right: 1rem;
    }
    .header{
        display: flex;
        justify-content: space-between;
        padding: 2rem;
        color: red;
        background-color: black;
        .logo{
            font-size: 2rem;
        }
        select{
            margin-right: 1rem;
            padding: 4px 10px;
        }
        input{
            padding: 4px 10px;
        }
        button{
            padding: 4px 20px;
            border-radius: 5px;
        }
    }
</style>
