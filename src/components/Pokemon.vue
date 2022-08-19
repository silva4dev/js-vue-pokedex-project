<template>
  <div id="pokemon">
    <div class="card has-text-centered">
      <div class="card-image">
        <figure>
          <img class="mt-5" v-bind:src="currentImg" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content text-center">
        <div class="media">
          <div class="media-content">
            <p class="title is-5">{{ num }} - {{ upper }}</p>
            <p class="subtitle is-6">{{ pokemon.type }}</p>
          </div>
        </div>

        <div class="content">
          <button @click="mudarSprite" class="button is-medium is-fullwidth">
            Mudar sprite
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  created: function () {
    axios.get(this.url).then((response) => {
      this.pokemon.type =
        response.data.types[0]["type"].name[0].toUpperCase() +
        response.data.types[0]["type"].name.slice(1);
      this.pokemon.front = response.data.sprites.front_default;
      this.pokemon.back = response.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
    });
  },
  data() {
    return {
      isFront: true,
      currentImg: "",
      pokemon: {
        type: "",
        front: "",
        back: "",
      },
    };
  },
  props: {
    num: Number,
    name: String,
    url: String,
  },
  computed: {
    upper: function () {
      const newName = this.name[0].toUpperCase();
      return newName + this.name.slice(1);
    },
  },
  methods: {
    mudarSprite: function () {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.back;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.front;
      }
    },
  },
};
</script>

<style scoped>
#pokemon {
  margin-top: 2%;
}
.media {
  position: relative;
  margin-top: -3%;
}
</style>
