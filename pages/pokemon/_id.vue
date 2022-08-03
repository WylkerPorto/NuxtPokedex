<template>
  <body>
    <main v-if="pokemon">
      <h1>{{ pokemon.id }} - {{ pokemon.name }}</h1>
      <img
        v-if="pokemon.sprites"
        :src="pokemon.sprites.other['official-artwork'].front_default"
        :alt="pokemon.name"
      />

      <section>
        <p>Peso: {{ pokemon.weight }}</p>
        <p>Altura: {{ pokemon.height }}</p>
      </section>

      <h2>Tipos</h2>
      <section v-if="types">
        <p v-for="type in types" :key="type.name">
          {{ type.name }}
        </p>
      </section>

      <section v-if="sprites.length">
        <h2>game sprites</h2>
        <article>
          <img
            :src="sprite"
            :alt="sprite"
            v-for="(sprite, key) in sprites"
            :key="key"
          />
        </article>
      </section>

      <section v-if="otherSprites.length">
        <h2>outras sprites</h2>
        <div v-for="(other, key) in otherSprites" :key="key">
          <h3>{{ other.origin }}</h3>
          <article>
            <img
              :src="st"
              :alt="st"
              :key="skey"
              v-for="(st, skey) in other.sprites"
            />
          </article>
        </div>
      </section>

      <section v-if="stats.length">
        <h2>Status</h2>
        <div v-for="(stt, key) in stats" :key="key">
          <span>{{ stt.name }}</span>
          <progress :value="stt.base" max="255">
            {{ stt.base }}
          </progress>
        </div>
      </section>

      <section v-if="abilities.length">
        <h2>Habilidades</h2>
        <ul>
          <li v-for="(ability, key) in abilities" :key="key">
            {{ ability.name }}
          </li>
        </ul>
      </section>

      <section v-if="moves.length">
        <h2>Movimentos</h2>
        <ul>
          <li v-for="(move, key) in moves" :key="key">
            {{ move.name }}
          </li>
        </ul>
      </section>
    </main>
    <main v-else><h1>Pokemon não encontrado</h1></main>
  </body>
</template>

<script>
export default {
  layout: "dex",
  name: "pokemon",
  data() {
    return {
      pokemon: {},
      types: [],
      moves: [],
      sprites: [],
      otherSprites: [],
      stats: [],
      abilities: [],
    };
  },

  async fetch() {
    try {
      this.pokemon = await this.$axios.$get(`pokemon/${this.$route.params.id}`);
      this.constructor();
    } catch (e) {
      this.pokemon = false;
    }
  },

  methods: {
    constructor() {
      this.pokemon.types.forEach((type) => {
        this.types.push({ name: type.type.name });
      });

      Object.entries(this.pokemon.sprites).forEach(([key, sprite]) => {
        if (typeof sprite == "string") this.sprites.push(sprite);
      });

      Object.entries(this.pokemon.sprites.other).forEach(([key, other]) => {
        let osprite = {
          origin: key,
          sprites: [],
        };

        Object.entries(other).forEach(([chave, spt]) => {
          if (spt) osprite.sprites.push(spt);
        });

        this.otherSprites.push(osprite);
      });

      this.pokemon.moves.forEach((move) => {
        this.moves.push({ name: move.move.name });
      });

      this.pokemon.abilities.forEach((ability) => {
        this.abilities.push({ name: ability.ability.name });
      });

      this.pokemon.stats.forEach((stats) => {
        this.stats.push({
          name: stats.stat.name,
          base: stats.base_stat,
        });
      });
    },
  },
};
</script>

<style lang="scss" scoped>
h1 {
  @apply text-6xl text-center mt-5 font-semibold;
}

h2 {
  @apply text-center font-semibold text-4xl font-sans;
}

h3 {
  @apply text-center text-2xl;
}

main {
  @apply container mx-auto pb-16;

  > img {
    @apply mx-auto h-full w-max border-4 border-black rounded-3xl;
  }

  > section {
    @apply mt-3;

    &:nth-child(3),
    &:nth-child(5) {
      @apply flex justify-center;

      p {
        @apply mr-2;
      }
    }

    div {
      @apply text-center;

      progress {
        @apply justify-around align-middle my-1 w-72 h-6 border-2 border-gray-500 rounded-2xl;

        &::-webkit-progress-bar {
          @apply bg-gray-200 rounded-2xl;
        }

        &::-webkit-progress-value {
          @apply rounded-2xl;
          background-image: -webkit-linear-gradient(
              -45deg,
              transparent 33%,
              rgba(0, 0, 0, 0.1) 33%,
              rgba(0, 0, 0, 0.1) 66%,
              transparent 66%
            ),
            -webkit-linear-gradient(top, rgba(255, 255, 255, 0.25), rgba(0, 0, 0, 0.25)),
            -webkit-linear-gradient(left, rgb(0, 204, 0), rgb(194, 10, 10));
          background-size: 35px 20px, 100% 100%, 100% 100%;
        }
      }
    }

    article {
      @apply flex justify-around;

      img {
        @apply w-1/5 h-auto;
      }
    }

    ul {
      @apply grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-4;

      li {
        @apply text-center;
      }
    }
  }
}
</style>