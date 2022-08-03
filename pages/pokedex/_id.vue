<template>
  <main>
    <section v-if="pokedex">
      <h1>Pokedex da região de {{ regionName }}</h1>
      <ul>
        <li v-for="pokemon in pokedex" :key="pokemon.id">
          <nuxt-link :to="{ name: 'pokemon-id', params: { id: pokemon.id } }">
            <Pokecard :dex="pokemon" />
          </nuxt-link>
        </li>
      </ul>
    </section>
    <section v-else><h1>Região não encontrada.</h1></section>
  </main>
</template>

<script>
export default {
  layout: "dex",
  name: "Pokedex",
  loading: true,
  data() {
    return {
      pokedex: [],
      regionName: null,
    };
  },

  async fetch() {
    try {
      const generations = await this.$axios.$get(
        `generation/${this.$route.params.id}`
      );
      this.pokedex = generations.pokemon_species;
      this.regionName = generations.main_region.name;
      this.constructor();
    } catch (error) {
      this.pokedex = false;
    }
  },

  methods: {
    constructor() {
      this.pokedex.forEach((poke) => {
        poke.id = poke.url.split("/")[6];
      });
      this.pokedex.sort((a, b) => (parseInt(a.id) > parseInt(b.id) ? 1 : -1));
    },
  },
};
</script>

<style lang="scss" scoped>
section {
  @apply px-5 mt-5;

  h1 {
    @apply text-center text-4xl;
  }

  ul {
    @apply flex flex-wrap;
  }
}
</style>