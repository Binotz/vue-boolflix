<template>
  <div class="cards-component-layout">
    <SingleCardComponent v-for="card in listOfResults" :key="card.id" 
        :item="card"
    />
  </div>
</template>

<script>
import SingleCardComponent from './SingleCardComponent.vue';
export default {
    name: "CardsComponent",
    props: {
        listOfResults: Array
    },
    components: { SingleCardComponent }
}
</script>

<style lang="scss" scoped>
    .cards-component-layout{
        display: flex;
        flex-wrap: wrap;
        gap: 1rem;
        padding: 4rem;
    }
</style>
