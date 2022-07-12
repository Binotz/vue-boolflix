<template>
  <div class="card-layout">
    <div class="card-content">
        <span>Titolo: {{ item.title? item.title : item.name }} </span>
        <span>Titolo originale: {{ item.original_title ? item.original_title : item.original_name }} </span>
        <span>Lingua: {{ item.original_language}} </span>
        <span>Voto: {{ item.vote_average}} </span>
        <img :src="setFlag(item.original_language)" alt="flag">
        <img :src="coverBaseURL + item.poster_path" alt="cover">
        <div class="star-ranking">
            <span v-for="star,index in getRanking1to5(item.vote_average)" :key="index">
                <i class="fa-solid fa-star"></i>
            </span>
        </div>
    </div>
  </div>
</template>

<script> 

export default {
    name:"SingleCardComponent",
    props:{
        item: Object
    },
    data(){
        return{
            coverBaseURL: 'https://image.tmdb.org/t/p/w342/',
            flagsArray:['it','fr','de','uk','en'],
        }
    },
    methods:{
        setFlag:function(flag){
            //se la lang è inclusa nell'array delle bandiere, setta il path della bandiera...
            if (this.flagsArray.includes(flag)){
                flag = flag + '.png';
                return require('@/assets/img/flags/' + flag);
            }
            //... altrimenti ne usa uno di default
            return require('@/assets/img/flags/default.png');
        },
        getRanking1to5: function(rank){
            //divido il punteggio per due, arrotondando all'intero più vicino, e faccio l'approssimazione per eccesso
            return Math.ceil(Math.round(rank/2));
        }
    }
}
</script>

<style lang="scss" scoped>
.card-layout{
    width: calc((100% / 5) - 1rem);
    background-color: pink;
}

.card-content{
    display: flex;
    flex-direction: column;
}
span{
    margin-bottom: 10px;
}
img{
    width: 20%;
}
</style>
