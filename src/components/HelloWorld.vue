<template>
<section class="buana">
  <input type="search"
              placeholder="Search"
              v-model="inputSearch"
              @keyup="searchCats"
              class="form-control"
              >
  <div id="box-loading" >
  <div v-for="(cat, index) in cats" :key="index" >
    <button 
      class="btn btn-primary" 
      :data-bs-target="'#collapseExample'+index" 
      data-bs-toggle="collapse" getNextUser>
    {{ cat.name }} | {{index}}
    </button>
    <div class="desc collapse py-2" :id="'collapseExample'+index">
      {{ cat.description}}
    </div>
  </div>
  <VueEternalLoading :load="load"></VueEternalLoading>

  <!-- <infinite-loading target="#box-loading" @infinite="load"></infinite-loading> -->
  </div>
</section>

</template>

<script>
  import axios from 'axios'
  import {onMounted, ref} from 'vue'
  import "bootstrap/dist/css/bootstrap.min.css"
  import "bootstrap"  
  // import { VueEternalLoading, LoadAction } from '@ts-pro/vue-eternal-loading';
  //  import InfiniteLoading from "v3-infinite-loading";
  // import "v3-infinite-loading/lib/style.css";
const PAGE_SIZE = 5;

export default {
  // name: 'SimpleUsage',
  // components: { VueEternalLoading },

  setup() {
    let cats = ref([]);
    let inputSearch = ref("");
    // let page = 1;

    onMounted(() => {
        //get data from api
        axios.get('https://api.thecatapi.com/v1/breeds')
        .then((result) => {
          cats.value = result.data
          //console.log(cats)
        }).catch((err) => {
          console.log(err.response)
        })
    });

    function searchCats() {
      axios.get('https://api.thecatapi.com/v1/breeds')
      .then((result) => {
        if(inputSearch){
          cats.value = result.data.filter(cats =>
          cats.name.toLowerCase().includes(inputSearch.value.toLowerCase())
          );
        } else {
           cats.value = result.data.name
        };
          console.log(inputSearch)
        }).catch((err) => {
          console.log(err.response)
        })
    }

    

    // function loadCats(page) {
    //   return fetch(`https://api.thecatapi.com/v1/breeds?limit=1&page=${PAGE_SIZE}&page=${page}`)
    //       .then((res) => res.json())
    //       .then((res) => res.data);
    // }

    // async function load({ loaded }) {
    //   const loadedCats = await loadCats(page);
    //   cats.value.push(...loadedCats);
    //   page += 1;
    //   loaded(loadedCats.length, PAGE_SIZE);
      
    // }

    return {
      cats,
      inputSearch,
      searchCats,
      // load
    }
  }
}
</script>

<style>
.buana{
    width: 500px;
    border:1px solid #eee;
    border-radius:5px;
    padding: 20px;
}
.buana .btn-primary{
    width: 100%;
    margin-top: 15px;
    border-radius: 0px;
    text-align: left;
}
.buana .desc{
    border:1px solid #eee;
    padding: 20px;
    text-align: justify;
}
</style>