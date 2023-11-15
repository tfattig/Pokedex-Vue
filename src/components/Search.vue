Copy code
<template>
  <!-- Search bar component -->
  <div class="searchbar">
    <!-- Form to handle search input -->
    <form @submit.prevent="setPokemonUrl">
      <!-- Input field for searching Pokemon by name or Pokedex number -->
      <input type="text" placeholder="Search for a Pokemon using a name or Pokedex number" v-model="searchvalue">
    </form>
    <!-- Search icon to trigger search -->
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
        searchvalue: '', // Data property to store the search input value
      }
    },
    methods: {
      // Emit event to parent component with the constructed URL based on the search input
      setPokemonUrl() {
        if(this.searchvalue !== '')
          this.$emit('setPokemonUrl', this.apiUrl + this.searchvalue);
      }
    }
  }
</script>
  
<style scoped>
  /* Styling for the search bar container */
  .searchbar {
      position: relative;
      width: 100%;
      max-width: 510px;
      padding-bottom: 20px;
  }
  /* Styling for the input field */
  input {
    border: none;
    outline: none;
    border-radius: 5px;
    padding: 10px 40px 10px 10px;
    width: calc(100% - 50px);
    font-size: 1rem;
    /* Box shadow for a subtle visual effect */
    box-shadow: 0 15px 30px rgba(0,0,0,.2),
                0 10px 10px rgba(0,0,0,.2);
  }
  /* Styling for the search icon */
  .legenda {
      color: #fff;
  }
  i {
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 1.25rem;
    color: #0A2E50;
    cursor: pointer;
  }
</style>