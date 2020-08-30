<template>
  <div class="wrapper">
    <div class="search">
      <label for="search"/>Search</label>
      <input 
      id="search" 
      name="search" 
      v-model="searchValue"
      @input="handleInput"/>
    </div>
    <ul v-for="item in results" :key="item.data[0].nasa_id">
      <li>{{item.data[0].description}}</li>
    </ul>
  </div>
</template>

<script>

import debounce from 'lodash.debounce';
import axios from 'axios';
const API = 'https://images-api.nasa.gov';

export default {
  name: 'Search',
  data(){
    return{
      searchValue: '',
      results: []
    }
  },
  methods:{
    handleInput: debounce(function(){
      axios.get(`${API}/search?q=${this.searchValue}&media_type=image`)
      .then((response) => {
        this.results = response.data.collection.items;
      })
      .catch((error) => {
        console.log(error);
      });
    }, 500)
  }
}
</script>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600&display=swap');
  .wrapper{
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    margin: 0;
    padding: 30px;
  }

  .search{
    display: flex;
    flex-direction: column;
    width: 300px;
  }

  input{
    border: 0;
    border-bottom: 1px solid black;
  }

  label{
    font-family: 'Montserrat', sans-serif!important;
  }
</style>
