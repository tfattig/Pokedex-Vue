<template>
  <!-- Main container for the Pokedex application -->
  <div class="container">
    <!-- Pokedex logo and title -->
    <img
      alt="Vue logo"
      src="https://upload.wikimedia.org/wikipedia/commons/9/98/International_Pok%C3%A9mon_logo.svg"
    >
    <h1>Pokedex in Vue JS</h1>
    
    <!-- Search component with the ability to set the Pokemon URL -->
    <Search :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl"/>
    
    <!-- List component to display Pokemon entries with the ability to set the Pokemon URL -->
    <List :imageUrl="imageUrl" :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl"/>
    
    <!-- Details component to show detailed information about a selected Pokemon -->
    <Details
      v-if="showDetail"
      :pokemonUrl="pokemonUrl"
      :imageUrl="imageUrl"
      @closeDetail="closeDetail"
    />
  </div>
</template>

<script>
  import Search from "./Search.vue";
  import List from "./List.vue";
  import Details from "./Details.vue";
  
  export default {
    data: () => {
      return {
        // Base URL for Pokemon images and API URL for fetching Pokemon data
        imageUrl:
          "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/",
        apiUrl: "https://pokeapi.co/api/v2/pokemon/",
        // URL of the selected Pokemon and flag to show/hide the Details component
        pokemonUrl: "",
        showDetail: false
      };
    },
    components: {
      // Importing child components for use in the template
      Search,
      List,
      Details
    },
    methods: {
      // Method to set the Pokemon URL and show the Details component
      setPokemonUrl(url) {
        this.pokemonUrl = url;
        this.showDetail = true;
      },
      // Method to close the Details component and reset the selected Pokemon URL
      closeDetail() {
        this.pokemonUrl = "";
        this.showDetail = false;
      }
    }
  };
</script>

<style lang="scss" scoped>
  /* Styling for the main container */
  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    padding: 10px;
    width: calc(100% - 20px);
    min-height: calc(100vh - 20px);
  
    font-family: "Acme", arial;
    font-size: 1rem;
    font-weight: normal;
  }
  
  /* Styling for the Pokedex title */
  h1 {
    color: #efefef;
  }
</style>