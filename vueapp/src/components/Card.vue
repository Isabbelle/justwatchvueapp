<template>
  <div class="card">
    <img v-bind:href="movie.poster" />
    <h3>Title: {{ movie.title }}</h3>
    <p>Type: {{ movie.object_type }}</p>
    <div class="result-link">
      <p>
        Link:
        <a
          v-bind:href="'https://www.justwatch.com' + movie.full_path"
          target="_blank"
          >Go to JustWatch</a
        >
      </p>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: "Card",
  props: ["movie"],
};
</script>

<style lang="scss" scoped>
.card {
  flex: 1 1 33%;
  width: 33%;
  max-width: 33%;
  padding: 0px 8px;
  margin-bottom: 16px;

  img {
    object-fit: cover;
    border-radius: 16px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
    transition: 0.4s;
  }
  h3 {
    padding: 16px;
    color: #666;
    transition: 0.4s;
  }
  &:hover {
    img {
      transform: scale(1.05);
    }
    h3 {
      color: #24c444;
    }
  }
}
</style>
