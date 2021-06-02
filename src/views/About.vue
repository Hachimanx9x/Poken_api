<template>
  <div class="about">
    <div
      class="w-3/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center"
      v-if="state.pokemon"
    >
      <h3 class="text-2xl text-green-900 uppercase">
        {{ state.pokemon.forms[0].name }}
      </h3>
      <div class="flex justify-center">
        <img
          class="w-48"
          :src="state.pokemon.sprites.front_shiny"
          :alt="state.pokemon.forms[0].name"
        />
        <img
          class="w-48"
          :src="state.pokemon.sprites.back_shiny"
          :alt="state.pokemon.forms[0].name"
        />
      </div>
      <h3 class="text-yellow-400">Tipo</h3>
      <div v-for="(type, idx) in state.pokemon.types" :key="idx">
        <h5 class="text-blue-900">{{ type.type.name }}</h5>
      </div>
    </div>
  </div>
  <div class="mt-4 flex justify-center flex-col items-center">
    <router-link
      class="bg-blue-500 hover:bg-blue-400 text-white font-bold py-2 px-4 border-b-4 border-blue-700 hover:border-blue-500 rounded"
      to="/"
      >Atras</router-link
    >
  </div>
</template>
<script>
import { reactive } from "vue";
import { useRoute } from "vue-router";

export default {
  setup() {
    const route = useRoute();
    const state = reactive({
      pokemon: null,
    });
    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.poke}`)
      .then((res) => res.json())
      .then((data) => {
        state.pokemon = data;
        //  console.log(state.pokemon.forms[0].name);
      });
    return { state };
  },
};
</script>
