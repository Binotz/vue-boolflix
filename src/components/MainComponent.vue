<template>
  <section class="cards-layout">
    <CardsComponentVue v-if="errorMessage === ''" :listOfResults="listOfResults"/>
    <ErrorComponentVue v-else :msg="errorMessage"/>
  </section>
</template>

<script>
import CardsComponentVue from './CardsComponent.vue';
import ErrorComponentVue from './ErrorComponent.vue';

export default {
    name:"CardsComponent",
    components:{
      CardsComponentVue,
      ErrorComponentVue      
    },
    props:{
      listOfResults: Array,
      errorMessage: String
    }
}
</script>

<style>

</style>
