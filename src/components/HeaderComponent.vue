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
            this.$emit('userInput', this.searchText);
            this.$emit('filter-genre', 'All');
        },
        filterGenre: function(){
            this.$emit('filter-genre', this.filteredGenre);
        }
    },
    computed:{
        listOfGenres: function(){
            // console.log(this.listOfResults);
            let tmpGenre = [];
            let genresList = [];
            tmpGenre.push('All');
            if(this.listOfResults.length > 0){
                this.listOfResults.forEach(result =>{
                    if(result.genres){
                        result.genres.forEach(genre =>{
                            tmpGenre.push(genre);
                        });
                    } else {
                        tmpGenre.push('N/A');
                    }
                });
            }
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
    }
</style>
