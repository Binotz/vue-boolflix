<template>
  <section class="cards-layout">
    <CardsComponentVue :listOfResults="listOfResults"/>
  </section>
</template>

<script>
import CardsComponentVue from './CardsComponent.vue';

export default {
    name:"CardsComponent",
    components:{
      CardsComponentVue
    },
    props:{
      listOfResults: Array
    }
}
</script>

<style>

</style>
