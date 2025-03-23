<template>
    <div class="text-center">
      <form @submit.prevent="setPokemonUrl">
        <input type="text" v-model="searchvalue" class="bg-slate-100">
      </form>
      <i class="fas fa-search" @click="setPokemonUrl"></i>
    </div>
  </template>
  
  <script>
    export default {
      props: [
        'apiUrl' 

      ],
      data: () => {
        return {
          searchvalue: '',
        }
      },
      methods: {
        setPokemonUrl(url) {
          if(this.searchvalue !== '')
            this.$emit('setPokemonUrl', this.apiUrl + this.searchvalue);
        }
      }
    }
  </script>