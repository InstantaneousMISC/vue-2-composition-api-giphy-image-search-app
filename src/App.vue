<template>
  <div id="app">
    <form @submit.prevent="searchGifs">
      <label for>Search</label>
      <input v-model="search" />
    </form>
    <div class="loading" v-if="loading">
      <img alt="loading" src="/public/assets/loading.gif" />
    </div>
    <div v-if="!loading">
      <div v-for="gif in gifs" :key="gif" class="image-container">
        <img :src="gif.images.fixed_height.url" />
        <p>{{gif.slug}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { value } from "vue-function-api";
import { setTimeout } from "timers";

const API_URL =
  "https://api.giphy.com/v1/gifs/search?api_key=JnxTmEGKXjZeUKBzRjTQoMDg8OX8pS5U&rating=pg&q=";

export default {
  setup() {
    const search = value("");
    const loading = value(false);
    const gifs = value([]);
    const gifSlug = value([]);

    const searchGifs = async () => {
      loading.value = true;
      const response = await fetch(API_URL + search.value);
      const json = await response.json();
      console.log(json);
      gifs.value = json.data.map(gif => gif);
      gifSlug.value = json.data.map(gif => gif.slug);
      setTimeout(() => {
        loading.value = false;
      }, 1000);
    };

    return {
      search,
      searchGifs,
      loading,
      gifs,
      gifSlug
    };
  }
};
</script>

<style>
.image-container {
  width: 200px;
  display: inline-block;
  float: left;
}
.image-container:hover {
  background: #ccc;
}
.image-container p {
  white-space: pre-wrap;
  word-break: break-all;
  padding: 10px;
  width: calc(100% - 50px);
}
.images {
  column-count: 4;
}

img {
  width: calc(100% - 20px);
  margin: auto;
  padding: 5px;
  display: block;
}

.loading {
  margin: 0 auto;
}
</style>
