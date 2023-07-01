<template>
  <div class="container" v-if="puntaje < 10 && intento <= 5">
    <h1>Casino</h1>
    <div class="score">
      <h2>Puntaje: {{ puntaje }}</h2>
      <h2>Intentos: {{ intento }}</h2>
    </div>
    <div class="imagenes">
      <PokemonImg
        :pokemonId="id1"
        :nombrePokemon="nombre1"
        :mostrarPokemon="revelarImagen"
      />
      <PokemonImg
        :pokemonId="id2"
        :nombrePokemon="nombre2"
        :mostrarPokemon="revelarImagen"
      />
      <PokemonImg
        :pokemonId="id3"
        :nombrePokemon="nombre3"
        :mostrarPokemon="revelarImagen"
      />
    </div>
  </div>

  <button @:click="obtenerValoresPokemon()" v-if="puntaje < 10 && intento < 5">
    Jugar
  </button>
  <div class="win-message" v-if="puntaje >= 10 && intento <= 5">
    <h2>Puntaje: {{ puntaje }}</h2>
    <h2>Felicitaciones has ganado un premio de $10.000,00</h2>
    <button @:click="reiniciarJuego">Nuevo Juego</button>
  </div>
  <div class="lose-message" v-if="puntaje < 10 && intento >= 5">
    <h2>Has utilizado tus 5 intentos</h2>
    <h2>El juego ha terminado, intentalo nuevamente</h2>
    <button @:click="reiniciarJuego">Nuevo Juego</button>
  </div>
</template>

<script>
import PokemonImg from "../components/PokemonImg.vue";
import obtenerPokemonsFachada from "../helpers/clientePokemonAPI";

export default {
  name: "CasinoPage",
  data() {
    return {
      nombre1: "XXXXXXXXXXX",
      nombre2: "XXXXXXXXXXX",
      nombre3: "XXXXXXXXXXX",
      id1: 1,
      id2: 1,
      id3: 1,
      puntaje: 0,
      intento: 0,
      revelarImagen: false,
    };
  },
  components: {
    PokemonImg,
  },
  methods: {
    obtenerNumerosAleatorios() {
      return Math.floor(Math.random() * 4);
    },
    async obtenerValoresPokemon() {
      const pokemons = await obtenerPokemonsFachada();
      console.log(pokemons);

      const obj1 = pokemons[this.obtenerNumerosAleatorios()];
      const obj2 = pokemons[this.obtenerNumerosAleatorios()];
      const obj3 = pokemons[this.obtenerNumerosAleatorios()];

      console.log(obj1);

      this.id1 = obj1.id;
      this.id2 = obj2.id;
      this.id3 = obj3.id;

      this.nombre1 = obj1.nombre;
      this.nombre2 = obj2.nombre;
      this.nombre3 = obj3.nombre;

      this.revelarImagen = true;
      this.calcularPuntajeIntentos();
    },

    calcularPuntajeIntentos() {
      if (this.id1 == this.id2 && this.id2 == this.id3) {
        this.puntaje += 5;
        this.intento++;
      } else if (
        this.id1 == this.id2 ||
        this.id2 == this.id3 ||
        this.id1 == this.id3
      ) {
        this.puntaje += 2;
        this.intento++;
      } else {
        this.intento++;
      }
    },

    reiniciarJuego() {
      this.nombre1 = "XXXXXXXXXXX";
      this.nombre2 = "XXXXXXXXXXX";
      this.nombre3 = "XXXXXXXXXXX";
      this.puntaje = 0;
      this.intento = 0;
      this.revelarImagen = false;
    },
  },
};
</script>

<style>
.imagenes{
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
}
img {
    margin-left: 10px;
    margin-right: 10px;
}
.win-message h2 {
    color: blue;
}
.win-message button {
    border: 2px solid black;
    width: 100px;
    height: 30px;
}
.lose-message h2 {
    color: red;
}
.lose-message button {
    border: 2px solid black;
     width: 100px;
    height: 30px;
}
.score{
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: row;
}

.score h2{
    margin-right: 10px;
    margin-left: 10px;
}
</style>