<template>
  <header>
    <h1>The movie database</h1>
    <div class="search-box">
      <input
        placeholder="Search here"
        class="search-field"
        required
        v-model="searchQuery"
      />
    </div>
  </header>

  <main>
    <div class="cards" v-if="movieList.length > 0">
      <Card v-for="movie in movieList" :key="movie.id" :movie="movie" />
    </div>
    <div class="no-results" v-else>
      <h3>Sorry, we have no results...</h3>
    </div>
  </main>
</template>

<script>
import _ from "lodash";
import Card from "./components/Card.vue";

export default {
  components: { Card },
  props: {
    movies: {
      type: String,
    },
  },

  data() {
    return {
      searchQuery: "",
      movieList: [],
    };
  },

  watch: {
    searchQuery: function() {
      this.debouncedGetMovieData();
    },
  },
  created: function() {
    this.debouncedGetMovieData = _.debounce(this.getMovieData, 500);
  },
  methods: {
    getMovieData: async function() {
      const body = {
        page_size: 50,
        query: this.searchQuery,
        content_types: ["show", "movie"],
      };
      this.movieList = await fetch(
        `https://apis.justwatch.com/content/titles/en_US/popular?language=en&body=${JSON.stringify(
          body
        )}`
      )
        .then((response) => response.json())
        .then((results) => results.items);
      console.log(this.movieList);
    },
  },
};
</script>

<style lang="scss">
header {
  padding-top: 50px;
  padding-bottom: 50px;
}

h1 {
  color: #888;
  font-size: 42px;
  text-align: center;
  text-transform: uppercase;
  margin-bottom: 30px;
}
.search-box {
  display: flex;
  justify-content: center;
  padding-left: 30px;
  padding-right: 30px;
}
.search-field {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  background-color: #f3f3f3;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);
  display: block;
  width: 100%;
  max-width: 600px;
  padding: 15px;
  border-radius: 8px;
  color: #313131;
  font-size: 20px;
  transition: 0.4s;
  &::placeholder {
    color: #aaa;
  }
  &:focus,
  &:valid {
    color: #fff;
    background-color: #313131;
    box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
  }
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Fira Sans", sans-serif;
}


main {
  max-width: 1200px;
  margin: 0 auto;
  padding-left: 30px;
  padding-right: 30px;
}
.cards {
  display: flex;
  flex-wrap: wrap;
  margin: 0 -8px;
}
</style>
