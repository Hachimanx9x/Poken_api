<template>
  <div class="w-full flex justify-center">
    <input
      type="text"
      placeholder="Pokemon"
      class="mt-10 p-2 border-blue-500 border-2"
      v-model="text"
    />
  </div>
  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div
      class="ml-4 text-2x text-blue-200 mt-5"
      v-for="(pokemon, idx) in filterPokemon"
      :key="idx"
    >
      <router-link
        class="bg-transparent hover:bg-blue-500 text-blue-700 font-semibold hover:text-white py-2 px-4 border border-blue-500 hover:border-transparent rounded"
        :to="`/about/${urlid[pokemon.name]}`"
        >{{ pokemon.name }}</router-link
      >
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { computed, reactive, toRefs } from "vue";
export default {
  name: "Home",
  components: {},
  setup() {
    const state = reactive({
      pokemons: [],
      urlid: {},
      text: "",
      filterPokemon: computed(() => updatePokemon()),
    });
    function updatePokemon() {
      if (!state.text) {
        return [];
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      );
    }
    fetch("https://pokeapi.co/api/v2/pokemon?limit=150")
      .then((datapoke) => datapoke.json())
      .then((data) => {
        //     console.log(data);
        state.pokemons = data.results;
        let temp = {};
        data.results.map((ele, i) => {
          temp[ele.name] = i + 1;
        });
        state.urlid = temp;
        //   console.log(temp);
        /*
        state.urlid = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
          {}
        );*/
      });

    return { ...toRefs(state) };
  },
};
</script>
