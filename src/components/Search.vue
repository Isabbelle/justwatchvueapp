<template>
  <header>
    <h1>The movie database</h1>
    <form class="search-box">
      <input
        placeholder="Search here"
        class="search-field"
        @input="onInput"
        required
        
      />
    </form>
  </header>
  
</template>

<script>
export default {
  name: "Search",
  data() {
    return {
      movies:[],
      timeout: null,
    };
  },
  methods: {
    onInput() {
      clearTimeout(this.timeout);
      this.timeout = setTimeout(() => {
        this.search();
      }, 500);
    },
    search() {
      fetch(
        `https://apis.justwatch.com/content/titles/en_US/popular?language=en&body={%22page_size%22:50,%22query%22:%22%22,%22content_types%22:[%22show%22,%22movie%22]}`
      )
        .then((response) => response.json())
        .then((results) => {
          console.log(results.items);
    
        });
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
</style>
