<template>
  <main class="container text-white">
    <div class="pt-4 mb-4 relative">
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:bg-gray-800 focus:outline-none focus:shadow[0px_1px_0_0_004E71]"
      />
      <ul
        class="absolute bg-gray-800 text-white w-full shadow-md py-2 px-1 top-[66px]" v-if="searchResults"
      >
        <li
          v-for="searchResult in searchResults"
          :key="searchResult"
          class="py-3 cursor-pointer"
        >
          {{ searchResult.place_name }}
        </li>
      </ul>
    </div>
  </main>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";

const tokenAPI =
  "pk.eyJ1Ijoibm9wc2t5IiwiYSI6ImNsaGgxcmZqbzBhNW4zaXMwZW44dDh0b20ifQ.-lhBdtqrSlSn1iuMcGy7tg";
const searchQuery = ref("");
const queryTimeout = ref(null);
const searchResults = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      const result = await axios.get(
        `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${tokenAPI}&types=place`
      );
      searchResults.value = result.data.features;
      return;
    }
    searchResults.value = null;
  }, 300);
};
</script>
