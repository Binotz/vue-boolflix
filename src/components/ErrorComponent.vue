<template>
  <div class="error">
        {{ msg}}
  </div>
</template>

<script>
export default {
    name: "ErrorComponent",
    props:{
        msg: String
    }
}
</script>

<style lang="scss" scoped>
.error{
    display: flex;
    justify-content: center;
    align-items: center;
    padding-top: 10rem;
    font-weight: bold;
    font-size: 4rem;
    color: white;
}
</style>
