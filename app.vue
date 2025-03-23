<template>
  <div class="flex flex-col items-center justify-center">
    <Pokemon />
  </div>
</template>

<script>
import Pokemon from './components/Pokemon.vue'

export default {
  components: {
    Pokemon
  }
}
</script>

<style>
body {
  background-color: #fefefe;
}
</style>