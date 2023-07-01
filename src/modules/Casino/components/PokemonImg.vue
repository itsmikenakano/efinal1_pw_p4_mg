<template>
  <div class="container">
    <div class="imagen">
      <img
        v-if="!mostrarPokemon"
        class="imagen-negra"
        src="../store/imagenNegro.png"
        alt="No se puede mostrar imagen"
      />
      <img
        v-if="mostrarPokemon"
        class="imagen-pokemon"
        :src="url"
        alt="No se puede mostrar imagen"
      />
      <label for="">{{ nombrePokemon }}</label>
    </div>
  </div>
</template>

<script>
export default {
  name: "PokemonImg",
  props: {
    pokemonId: {
      type: Number,
      required: true,
    },
    nombrePokemon: {
      type: String,
      required: true,
    },
    mostrarPokemon: {
      type: Boolean,
      required: true,
    },
  },
  computed: {
    url() {
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${this.pokemonId}.svg`;
    },
  },
};
</script>

<style scoped>
.imagen {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

img{
    width: 400px;
    height: 300px;
}
</style>