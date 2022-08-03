<template>
  <section>
    <vue-typer text="Bem vindo a essa incrível Pokedex em NuxtJS"></vue-typer>
    <nuxt-link to="/pokedex/1">Ir para Pokedex</nuxt-link>
  </section>
</template>

<script>
export default {
  name: "IndexPage",
};
</script>

<style lang="scss" scoped>
section {
  @apply flex flex-col items-center;

  span {
    @apply font-extralight text-3xl font-mono;
  }

  a {
    @apply px-5 py-3 border border-white rounded-2xl bg-red-500 hover:bg-red-600 mt-5;
  }
}
</style>