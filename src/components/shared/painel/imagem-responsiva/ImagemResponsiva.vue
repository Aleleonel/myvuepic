<template>
    <img class="imagem-responsiva" :src="url" :alt="titulo">
</template>

<script>
// import { defineComponent } from '@vue/composition-api'

export default {
    props: ['url', 'titulo']
}
</script>

<style scoped>
     .imagem-responsiva {

    width: 100%;
  }
</style>