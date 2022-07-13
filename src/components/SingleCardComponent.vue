<template>
  <div class="card-layout">
    <div class="card-content">
        <!-- Item details -->
        <div class="item-details">
            <div class="title"><span>Titolo:</span> {{ item.title? item.title : item.name }} </div>
            <div class="title-original"><span>Titolo originale: </span> {{ item.original_title ? item.original_title : item.original_name }} </div>
            <div class="star-ranking">
                <span>Voto: </span><span v-for="star,index in getRanking1to5(item.vote_average)" :key="index">
                    <i class="fa-solid fa-star"></i>
                </span>
            </div>
            <div class="cast"><span>Cast: <br/></span>
                <span class="cast-list" v-for="cast,index in item.castName" :key="index">{{ cast }}<br/></span>  
            </div>
            <div class="overview">
                <span>Overview: </span><p>{{item.overview}}</p>
            </div>
            <!-- castlist: {{item.castName}} -->
            <!-- <div class="rating"><span>Voto: </span>{{ item.vote_average}}</div> -->
            <!-- <div class="language"><span>Lingua: </span> {{ item.original_language}} </div> -->
            <!-- <img :src="setFlag(item.original_language)" alt="flag"> -->
        </div>
        <!-- Cover Image -->
        <img :src="coverBaseURL + item.poster_path" alt="cover" class="cover-image">
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
    },
}
</script>

<style lang="scss" scoped>
.card-layout{
    width: calc((100% / 5) - 1rem);
    background-color: black;
    &:hover{
        .cover-image{
            display: none;
        }
    }
}
.card-content{
    position: relative;
    height: 509px;
    padding: 2rem 1rem;
    color: white;
    overflow: hidden;
    .cover-image{
        position: absolute;
        top: 0;
        left: 0;
        display: block;
        height: 100%;
        object-fit: cover;
    }
    .item-details{
        display: flex;
        flex-direction: column;
    }
    div{
        font-size: 1.1rem;
        margin-bottom: .5rem;
    }
    span{
        font-weight: bold;
        &.cast-list{
            font-weight: 400;
        }
        i{
            color: yellow;
        }
    }
}
</style>
