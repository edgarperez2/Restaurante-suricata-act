<script setup>
    import { onMounted, ref, computed } from "vue"
    import axios from "axios"

    const emit = defineEmits(['descubrirPokemon'])

    const props = defineProps({
        pokemon: Object
    })

    const nombre = ref("")
    const imageUrl = ref("")
    const descubierto = ref(false)
    const mostrarPokemon = computed(() => {
        return descubierto.value
    })

    const onSubmit = e => {
        if ( !nombre.value.trim() ) {
            alert("Debe ingresar un nombre");
            return;
        }
        
        if ( props.pokemon.nombre !== nombre.value.trim() ) {
            alert(`El nombre "${nombre.value.trim()}" es incorrecto`)
            return;
        }
        emit('descubrirPokemon', 1);
        descubierto.value = true
    };

    onMounted(() => {
        const getPokemon = async () => {

          try {
            const { data } = await axios(props.pokemon.url)
            imageUrl.value = data.sprites.other.dream_world.front_default
          } catch (error) {
            console.log(error.message);
          }

        }
        getPokemon()
    })

</script>

<template>
    <div class="card">
        <div class="card__imagen">
            <img 
            :class="{escondido: !mostrarPokemon}"
            :src="imageUrl" :alt="`imagen ${pokemon.nombre}`">
        </div>
        
        <div class="card__body">
           <p v-if="mostrarPokemon"  class="card__name">
            {{ pokemon.nombre }}
           </p>

           <form 
           @submit.prevent="onSubmit"
           v-else class="card__form">
                <input 
                :style="{
                    width: '100%'
                }"
                v-model="nombre"
                name="nombre"
                id="nombre"
                type="text">

                <button class="submit">Descubrir</button>
            </form>
        </div>
    </div>
</template>


<style scoped>
    .card {
        width: 100px;
        margin: 0 auto;
        border: 1px solid #9c8c8c;
        border-radius: 10px;
        padding: 2rem;
        display: flex;
        flex-direction: column;
        gap: .5rem;
        align-items: center;
        box-shadow: gray 2px 2px 5px;
    }
    .card__body {
        display: flex;
        flex-direction: column;
        gap: .5rem;
        align-items: center;
    }
    .card__imagen {
        margin-bottom: .5rem;
    }
    .card__imagen img {
        height: 100px;
        width: 100px;
    }
    .card__form {
        display: flex;
        flex-direction: column;
        gap: .5rem;
        align-items: center;
    }
    .card__form button {
        padding: 0 1rem;
    }

    .escondido {
        filter: blur(5px) grayscale(100%);
    }

    .submit {
        background-color: rgb(137, 172, 238);
        border-radius: 10px;
    }

</style>