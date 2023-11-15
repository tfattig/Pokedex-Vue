<template>
  <div class="list">
    <!-- Display each Pokemon in a grid layout -->
    <article v-for="(pokemon, index) in pokemons"
    :key="'poke'+index"
    @click="setPokemonUrl(pokemon.url)">
      <!-- Display Pokemon image and name -->
      <img :src="imageUrl + pokemon.id + '.png'" width="96" height="96" alt="">
      <h3>{{ pokemon.name }}</h3>
    </article>
    <!-- Infinite scroll trigger for loading more Pokemon -->
    <div id="scroll-trigger" ref="infinitescrolltrigger">
      <i class="fas fa-spinner fa-spin"></i>
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
        pokemons: [],       // Array to store Pokemon data
        nextUrl: '',        // URL for the next set of Pokemon
        currentUrl: ''       // Current URL for fetching Pokemon data
      }
    },
    methods: {
      // Fetch Pokemon data from the API
      fetchData() {
        let req = new Request(this.currentUrl);
        fetch(req)
          .then((resp) => {
            if(resp.status === 200)
              return resp.json();
          })
          .then((data) => {
            this.nextUrl = data.next;
            // Extract Pokemon ID from the URL and push to the array
            data.results.forEach(pokemon => {
              pokemon.id = pokemon.url.split('/').filter(function(part) { return !!part }).pop();
              this.pokemons.push(pokemon);
            });
          })
          .catch((error) => {
            console.log(error);
          })
      },
      // Setup IntersectionObserver to trigger next() when scrolling
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
      // Load the next set of Pokemon
      next() {
        this.currentUrl = this.nextUrl;
        this.fetchData();
      },
      // Emit an event to parent component when a Pokemon is clicked
      setPokemonUrl(url) {
        this.$emit('setPokemonUrl', url);
      }
    },
    created() {
      // Initialize with the base API URL and fetch initial data
      this.currentUrl = this.apiUrl;
      this.fetchData();
    },
    mounted() {
      // Setup Infinite Scroll functionality on component mount
      this.scrollTrigger();
    }
  }
</script>

<style lang="scss" scoped>
  /* Styling for the grid layout */
  .list {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    grid-gap: 10px;
    width: 100%;
    max-width: 510px;    
  }
    /* Styling for each Pokemon entry */
    article {
      height: 150px;
      background-color: #efefef;
      text-align: center;
      text-transform: capitalize;
      border: 4px solid #ffcb05;
      border-radius: 5px;
      cursor: pointer;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 10px rgba(0,0,0,.2);      
    }
    /* Styling for Pokemon names */
    h3 {
        margin: 0;
      }
  /* Styling for the infinite scroll trigger */
  #scroll-trigger {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 150px;
    font-size: 2rem;
    color: #efefef;
  }
</style>