<template>
  <div class="header">
    <div class="logo">Boolflix</div>
    <div class="search-form">
        <input type="text" id="SearchText" v-model="searchText" @keyup.enter="search()">
        <button @click="search()">Cerca</button>
    </div>
  </div>
</template>

<script>
export default {
    name:"HeaderComponent",
    data(){
        return{
            searchText: ''
        }
    },
    methods:{
        search: function(){
            this.$emit('userInput', this.searchText);
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
