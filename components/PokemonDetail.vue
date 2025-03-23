<template>
    <div class="detail">
      <div class="detail-view" v-if="show">
        <div v-if="pokemon" class="image">
          <img :src="imageUrl + pokemon.id + '.png'" alt="">
        </div>
        <div v-if="pokemon" class="data">
          <h2>{{ pokemon.name }}</h2>
          <div class="property">
            <div class="left">Base Experience</div>
            <div class="right">{{ pokemon.base_experience }} XP</div>
          </div>
          <div class="property">
            <div class="left">Height</div>
            <div class="right">{{ pokemon.height / 10 }} m</div>
          </div>
          <div class="property">
            <div class="left">Weight</div>
            <div class="right">{{ pokemon.weight / 10 }} kg</div>
          </div>
          <h3>Pokemon Types</h3>
          <div class="types">
            <div class="type" 
              v-for="(value, index) in pokemon.types"
              :key="'value'+index">
              {{ value.type.name }}
            </div>
          </div>
          <h3>Abilities</h3>
          <div class="abilities">
            <div class="ability" 
              v-for="(value, index) in pokemon.abilities"
              :key="'value'+index">
              {{ value.ability.name }}
            </div>
          </div>
        </div>
        <h2 v-else>The pokemon was not found</h2>
        <button class="close" @click="closeDetail">close</button>
      </div>
      <i v-else class="fas fa-spinner fa-spin"></i>
    </div>
  </template>
  
  <script>
    export default {
      props: [
        'pokemonUrl',
        'imageUrl'
      ],
      data: () => {
        return {
          show: false,
          pokemon: {}
        }      
      },
      methods: {
        fetchData() {
          let req = new Request(this.pokemonUrl);
          fetch(req)
            .then((resp) => {
              if(resp.status === 200)
                return resp.json();
            })
            .then((data) => {
              this.pokemon = data;
              this.show = true;
            })
            .catch((error) => {
              console.log(error);
            })
        },
        closeDetail() {
          this.$emit('closeDetail');
        }
      },
      created() {
        this.fetchData();
      }
    }
  </script>
  