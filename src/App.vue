<script setup>
  import { onMounted, ref, computed } from "vue"
  import axios from "axios"
  import Pokemon from "./components/Pokemon.vue"

  const pokemones = ref([])
  const count = ref(0)

  // Computed property para manejar la lista de pokemones
  const computedPokemons = computed(() => {
    return pokemones.value
  })

  // Función para obtener los pokemones desde la API
  onMounted(() => {
    const getPokemons = async () => {
      try {
        const { data } = await axios('https://apimocha.com/api-deportes/list-sport')
        pokemones.value = data.results
        console.log(pokemones.value)

        // Inicializamos el contador en 0, por si no está iniciado.
        count.value = 0
      } catch (error) {
        console.log(error.message)
      }
    }
    getPokemons()
  })

  // Función para incrementar el contador cuando un pokemon es descubierto
  const incrementarContador = () => {
    count.value += 1
  }
</script>

<template>
  <div class="adivinapokemon">
    <div class="adivinapokemon__logo">
      <img src="../src/assets/pokemon-logo.png" class="logo" alt="pokemon logo" />
    </div>

    <h1 class="adivinapokemon__titulo">
      ¿Quién es ese Pokémon?
    </h1>

    <p class="adivinapokemon__cantidad">
      Pokemones descubiertos:  <span>{{ count }}</span>
    </p>

    <div class="adivinapokemon__grid">
      <!-- Se pasa la función de incremento como prop al componente Pokemon -->
      <Pokemon 
        v-for="pokemon in computedPokemons"
        :pokemon="pokemon"
        :key="pokemon.name"
        @descubrirPokemon="incrementarContador"
      />
    </div>
  </div>
</template>

<style scoped>

  .adivinapokemon__logo {
    width: 20rem;
    margin: 0 auto;
  }

  .adivinapokemon__cantidad {
    font-weight: 900;
    text-align: center;
    margin: 0 0 3rem 0;
  }

  .adivinapokemon__cantidad span {
    color: rgb(207, 32, 19);
  }
  .adivinapokemon__grid {
    display: grid;
    gap: 2rem;
    grid-template-columns: repeat(5, 1fr); 
    grid-template-rows: repeat(5, auto); 
  }
p {
  font-size: 20px;
}
</style>

