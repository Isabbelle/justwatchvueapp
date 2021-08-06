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

<script lang="ts">
import _ from "lodash";
import Card from "./components/Card.vue";
interface score {
  provider_type: string;
  value: number;
}
interface offers {
  jw_entity_id: string;
  type: string;
  monetization_type: string;
  provider_id: number;
  retail_price: number;
  last_change_retail_price: number;
  last_change_difference: number;
  last_change_percent: number;
  last_change_date: string;
  last_change_date_provider_id: string;
  currency: string;
  urls: {
    standard_web: string;
  };
  presentation_type: string;
  element_count: number;
  new_element_count: number;
  country: string;
}
interface MovieList {
  jw_entity_id: string;
  id: number;
  title: string;
  full_path: string;
  full_paths: {
    SHOW_DETAIL_OVERVIEW: string;
  };
  poster: string;
  poster_blur_hash: string;
  original_release_year: number;
  object_type: string;
  offers: offers[];
  scoring: score[];
}

export default {
  components: { Card },
  props: {
    movies: {
      type: String,
    },
  },

  data() {
    return {
      searchQuery: "" as string,
      movieList: [] as MovieList[],
    };
  },

  watch: {
    searchQuery: function() {
      this.debouncedGetMovieData();
    },
  },
  created: function() {
    this.debouncedGetMovieData = _.debounce(
      this.getMovieData,
      500
    ) as () => void;
  },
  methods: {
    getMovieData(): void {
      const body = {
        page_size: 50,
        query: this.searchQuery,
        content_types: ["show", "movie"],
      };
      this.movieList = fetch(
        `https://apis.justwatch.com/content/titles/en_US/popular?language=en&body=${JSON.stringify(
          body
        )}`
      )
        .then((response) => response.json())
        .then((results) => results.items);
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
a {
  text-decoration: none;
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
