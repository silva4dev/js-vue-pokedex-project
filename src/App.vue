<template>
  <div id="app">
    <div class="has-text-centered mt-5">
      <p class="is-size-3">
        <img src="./assets/logo.png" width="80" alt="{{ name }}" /><br />
        <strong>{{ name }}</strong>
      </p>
    </div>
    <div class="column is-half is-offset-one-quarter">
      <input
        class="input is-rounded"
        type="text"
        placeholder="Buscar pokemon pelo nome"
        v-model="busca"
      />
      <button @click="buscar" class="button is-fullwidth is-link" id="buscaBtn">
        Buscar
      </button>
      <hr />
      <div
        v-for="(pokemon, index) in filteredPokemons"
        v-bind:key="pokemon.name"
      >
        <Pokemon
          v-bind:name="pokemon.name"
          v-bind:url="pokemon.url"
          :num="index + 1"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon.vue";

export default {
  data() {
    return {
      name: "PokeDex - VueJS",
      busca: "",
      pokemons: [],
      filteredPokemons: [],
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((response) => {
        console.log("Pegou a lista de pokemons");
        this.pokemons = response.data.results;
        this.filteredPokemons = response.data.results;
      });
  },
  components: {
    Pokemon,
  },

  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(
          (pokemon) =>
            pokemon.name == this.busca ||
            pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1) ==
              this.busca
        );
      }
    },
  },
  computed: {
    /*resultadoBusca: function () {
      if (this.busca == "" || this.busca == " ") {
        return this.pokemons;
      } else {
        return this.pokemons.filter((pokemon) => pokemon.name == this.busca);
      }
    },*/
  },
};
</script>

<style scoped>
#buscaBtn {
  margin-top: 2%;
}
</style>
