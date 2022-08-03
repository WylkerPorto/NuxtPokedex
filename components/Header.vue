<!-- Please remove this file from your project -->
<template>
  <nav>
    <nuxt-link to="/">
      <img src="~/static/pokeball.png" alt="logo" />
    </nuxt-link>
    <ul v-if="generations.length">
      <li>Geração</li>
      <li v-for="(gen, key) in generations" :key="key">
        <nuxt-link :to="`/pokedex/${++key}`">
          {{ key }}
        </nuxt-link>
      </li>
    </ul>
    <form @submit.prevent="goPokemon">
      <label for="dexnumber">#</label>
      <input type="number" min="1" max="906" v-model="number" />
      <button>Pegue!</button>
    </form>
  </nav>
</template>

<script>
export default {
  name: "HeaderComponent",
  data() {
    return {
      number: null,
      generations: [],
      maxPokemon: 0,
    };
  },

  async fetch() {
    const generations = await this.$axios.$get("generation");
    this.generations = generations.results;

    const nationalDex = await this.$axios.$get("pokedex/1");
    this.maxPokemon = nationalDex.pokemon_entries.length;
  },

  methods: {
    goPokemon() {
      if (this.number && this.number > 0 && this.number <= this.maxPokemon) {
        this.$router.push({ name: "pokemon-id", params: { id: this.number } });
      }
    },
  },
};
</script>

<style lang="scss" scoped>
nav {
  background: var(--dex-color);

  @apply flex flex-wrap py-3 px-10 justify-between items-center;

  ul {
    @apply flex flex-wrap;

    li {
      &:not(:first-child) {
        @apply border-2 border-red-900 rounded-2xl mx-2;
      }
      a {
        @apply w-6 h-6 flex items-center justify-center text-sm;
      }
    }
  }

  img {
    @apply w-10 h-10;
  }

  form {
    label {
      @apply font-mono text-sm relative left-6;
    }

    input {
      -moz-appearance: textfield;
      width: 70px;
      @apply border-2 border-red-900 rounded-2xl text-sm py-0.5 pr-3 pl-5;

      &::-webkit-outer-spin-button,
      &::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
      }
    }
  }
}
</style>