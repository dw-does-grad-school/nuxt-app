<template>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Gidole&family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&display=swap" rel="stylesheet">
    <div class="list border-4 border-red-700 rounded-md p-4 max-w-md mx-auto h-[600px] overflow-y-auto shadow-lg">
      <div class="border-grey-300">
        <div class="grid grid-cols-3 gap-4">
        <article v-for="(pokemon, index) in pokemons"
        :key="'poke'+index"
        @click="setPokemonUrl(pokemon.url)"
        class="flex flex-col items-center justify-center">
          <img :src="imageUrl + pokemon.id + '.png'" width="96" height="96" alt="" class="border-2 border-gray-300 rounded-md">
          <h3 class="text-2xl text-center capitalize">{{ pokemon.name }}</h3>
        </article>
      </div>
      <div id="scroll-trigger" ref="infinitescrolltrigger" class="py-4">
        <i class="fas fa-spinner fa-spin"></i>
      </div>
      </div>
    </div>
  </template>
  
  <script>
    export default {
      props: [
        'imageUrl',
        'apiUrl'
      ],
      data: () => {
        return {
          pokemons: [],
          nextUrl: '',
          currentUrl: ''
        }
      },
      methods: {
        fetchData() {
          let req = new Request(this.currentUrl);
          fetch(req)
            .then((resp) => {
              if(resp.status === 200)
                return resp.json();
            })
            .then((data) => {
              this.nextUrl = data.next;
              data.results.forEach(pokemon => {
                pokemon.id = pokemon.url.split('/')
                  .filter(function(part) { return !!part }).pop();
                this.pokemons.push(pokemon);
              });
            })
            .catch((error) => {
              console.log(error);
            })
        },
        scrollTrigger() {
          const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
              if(entry.intersectionRatio > 0 && this.nextUrl) {
                this.next();
              }
            });
          });
  
          observer.observe(this.$refs.infinitescrolltrigger);
        },
        next() {
          this.currentUrl = this.nextUrl;
          this.fetchData();
        },
        setPokemonUrl(url) {
          this.$emit('setPokemonUrl', url);
        }
      },
      created() {
        this.currentUrl = this.apiUrl;
        this.fetchData();
      },
      mounted() {
        this.scrollTrigger();
      }
    }
  </script>

  <style>
    body {
      font-family: 'Gidole', sans-serif;
    }
  </style>