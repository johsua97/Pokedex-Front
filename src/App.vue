<template>
  <h1>POKEDEX</h1>
  <img src="./assets/pokedex.png" />
  <div>
    <label for="search-input">Buscar Pokémon:</label>
    <input type="text" id="search-input" v-model="searchTerm">
    <button @click="searchPokemon">Buscar</button>


  </div>
  <br />
  <div>
    <div class="pokemon-box" v-if="pokemon">
      <h2>Pokemon: {{ pokemon.name }}</h2>
      <h3>N. °: {{ pokemon.id }}</h3>
      <p>Tipo: {{ pokemon.type }}</p>
      <img :src="pokemon.image" width="200" alt="Imagen de {{ pokemon.name }}">
    </div>
    <div v-if="pokemonNotFound">
      <h2>No se encontró ningún Pokémon con ese ID o nombre</h2>
      <img src="./assets/not-found.png" width="100">
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: '',
      pokemon: null,
      pokemonNotFound: false
    };
  },
  methods: {
    searchPokemon() {
      fetch(`http://localhost:5000/pokemon/${this.searchTerm}`)
        .then(response => {
          if (!response.ok) {
            throw new Error('No se encontró ningún Pokémon con ese ID o nombre');
          }
          return response.json();
        })
        .then(data => {
          this.pokemon = data;
          this.pokemonNotFound = false;
        })
        .catch(error => {
          console.error(error);
          this.pokemon = null;
          this.pokemonNotFound = true;
        });
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.pokemon-box {
  border: 1px solid #ddd;
  box-shadow: 0px 0px 10px #ddd;
  padding: 10px;
  max-width: 500px;
  margin: 0 auto;
}

label {
  font-size: 18px;
  font-weight: bold;
  margin-right: 10px;
}

input[type="text"] {
  padding: 8px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

button {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0062cc;
}
</style>
