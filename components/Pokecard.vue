<template>
  <article :class="[type.length == 2 ? 'multiple-' + type.join('-') : type]">
    <h2>#{{ dex.id }}: {{ dex.name }}</h2>
    <div>
      <img :src="image" alt="pokemon" />
    </div>
    <p>{{ type.join(", ") }}</p>
  </article>
</template>

<script>
export default {
  name: "Pokecard",
  props: ["dex"],
  data() {
    return {
      id: 0,
      name: "",
      type: [],
      image: null,
    };
  },

  async fetch() {
    const pokemon = await this.$axios.$get(`pokemon/${this.dex.id}`);
    this.image = pokemon.sprites.other["official-artwork"].front_default;
    pokemon.types.forEach((type) => {
      this.type.push(type.type.name);
    });
  },
};
</script>

<style lang="scss" scoped>
article {
  @apply border border-gray-400 m-2.5 text-center overflow-auto rounded-2xl p-3;
  width: 210px;
  height: 210px;

  h2 {
    @apply m-0;
  }

  div {
    @apply bg-gray-100 rounded-full h-28 w-28 mx-auto my-2;

    img {
      @apply m-0 m-auto w-screen object-contain;
    }
  }

  p {
    margin: 0;
  }
}
</style>