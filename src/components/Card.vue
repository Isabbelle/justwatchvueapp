<template>
  <div class="card">
    <img v-bind:href="movie.poster"/>
    <h3>Title: {{ movie.title }}</h3>
    <p class="type">Type: {{ movie.object_type }}</p>
    <div class="result-link">
      <p>Link: <a
        v-bind:href="'https://www.justwatch.com' + movie.full_path"
        target="_blank"
        >Go to JustWatch</a
      ></p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: ["movie"],
};
</script>

<style lang="scss" scoped>
.card {
  flex: 1 1 33% ;
  max-width: 33%;
  padding: 0px 20px;
 
  .result-link{
   margin: 1rem;
   width: 33%;
   padding: 0px 8px;
   text-decoration: none;
  
}
.type{
  margin: 1rem;
  padding: 0px 8px;
}
  
  h3 {
    padding: 9px;
    color: black;
    transition: 0.4s;
  }
  &:hover {
    img {
      transform: scale(1.05);
    }
    h3 {
      color: #9858cc;
    }
    a{
       color: #9858cc;
        transition: 0.4s;
    }
  }
  a{
    text-decoration:none;
    color: black;
    font-weight: 600;
  }
}
</style>
