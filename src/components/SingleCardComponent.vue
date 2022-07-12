<template>
  <div class="card-layout">
    <div class="card-content">
        <span>Titolo: {{ item.title }} </span>
        <span>Titolo originale: {{ item.original_title }} </span>
        <span>Lingua: {{ item.original_language}} </span>
        <span>Voto: {{ item.vote_average}} </span>
    </div>
  </div>
</template>

<script>

export default {
    name:"SingleCardComponent",
    props:{
        item: Object
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
</style>
