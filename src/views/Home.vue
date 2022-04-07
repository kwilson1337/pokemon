<template>
<section class="poke-list">
  <div class="container">  
    <div class="poke-list__title">
      <h4>Choose your fighter!</h4>
    </div>

    <div       
      class="poke-list__grid">
      <SinglePoke 
        v-for="(single, index) in store.startPoke" :key="single.id"
        :data="single"
        :index="index"
      />
    </div>    

    <div
      :class="paginationActionClass" 
      class="poke-list__actions">
      <div v-if="store.page > 1" class="poke-list__prev">
        <button @click="prevPage">Previous</button>
      </div>
      
      <div class="poke-list__next">
        <button @click="nextPage">Next</button>
      </div>
    </div>
  </div>
</section>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import { computed, onMounted } from "@vue/runtime-core";

import axios from 'axios';
import SinglePoke from '../components/SinglePoke.vue'

  const store = ref({
    api: 'https://pokeapi.co/api/v2/pokemon/',
    startPoke: [],
    getPokeDetails: [],
    nextUrl: '',
    previousUrl: '',
    page: 1
  })

  const getDetails = (item) => {
    axios
        .get(item.url)
        .then(resp => {                                
            store.value.startPoke.push(resp.data)
            // console.log(store.value.startPoke) 
        })
  }

  const initPokemon = async () => {
    store.value.startPoke = []    
    try {
      await 
        axios.get(store.value.api)
        .then(data => {          
          store.value.nextUrl = data.data.next     
          store.value.previousUrl = data.data.previous ? data.data.previous : ''

          data.data.results.forEach(single => {
            getDetails(single)              
          })
        })
    }
    catch(err) {
      console.error(err)
    }
  }
  
  const nextPage = () => {
    store.value.api = store.value.nextUrl
    store.value.page++
    initPokemon()
  }

  const prevPage = () => {
    console.log(store.value.previousUrl)
    store.value.api = store.value.previousUrl
    store.value.page--
    
    initPokemon()
  }

  const paginationActionClass = computed(() => {
    if(store.value.page == 1) {
      return 'flexEnd'
    }
  })

  onMounted(() => {
    initPokemon()
  })
</script>

<style lang="scss">
  @import '../assets/styles/style.scss';

  .poke-list {

    &__title {
      text-align: center;
      margin-bottom: 40px;
    }

    &__grid {
      display: grid;
      gap: 15px;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    }

    &__actions {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 40px;

      &.flexEnd {
        justify-content: flex-end;
      }

      button,
      a {
        padding: 8px 24px;
        border-radius: 4px;
        background-color: $color1;
        color: $white;
        border: 0px;
      }
    }

    &__prev {
      button,
      a {
        background-color: $color3;
      }
    }
  }
</style>
