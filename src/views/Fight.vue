<template>
  <section class="poke-fight">
    <div class="container">
      <div class="row">
        <div class="col-md-6 poke-fight__left">
          <SinglePoke 
            v-for="(single, index) in store.chosen" :key="index"
            :data="single"
            :fight="false"
            :isFighting="store.isFighting"
          />
        </div>
        <div class="col-md-6 poke-fight__right">
          <SinglePoke 
            v-for="(single, index) in store.random" :key="index"
            :data="single"
            :fight="false"
            :isFighting="store.isFighting"
          />
        </div>
      </div>

      <div class="poke-fight__action">
        <button 
          class="btn-primary"
          @click="randomOponent"
        >
          Click to Fight!
        </button>        
      </div>
    </div> <!-- //container -->
  </section>  

  <BattleModal 
    :chosen="store.chosen"
    :opponent="store.random"
    :isActive="store.activeModal"
    @click="closeModal"
  />
</template>


<script setup>
import { onMounted, ref } from '@vue/runtime-core'
import {useRoute} from 'vue-router'
import SinglePoke from '../components/SinglePoke.vue'
import BattleModal from '../components/BattleModal.vue'

import axios from 'axios'
  const route = useRoute()
  
  const store = ref({
    api: 'https://pokeapi.co/api/v2/pokemon/',
    chosen: [],    
    chosenID: null,
    random: [],
    isFighting: false,
    activeModal: false
  })

  const closeModal = (winLoose) => {    
    store.value.activeModal = !store.value.activeModal        
    if(winLoose.target.dataset.winLoose === "false") {      
      window.location.href="/"
    } else {
      store.value.random = []
    }
  }

  const getChosenPoke = async () => {
    try {
      await
        axios
          .get(store.value.api + route.params.name)
          .then(resp => {
            store.value.chosen.push(resp.data)
            store.value.chosenID = resp.data.id          
          })
    } catch(err) {
      console.error(err)
    }    
  }

  const displayWinner = () => {
    setTimeout(() => {
      store.value.activeModal = true      
    }, 800)
  }

  const randomOponent = () => {    
    store.value.random = []
    store.value.isFighting = false
    let randomNumber = Math.floor(Math.random() * (+102 - 1)) + 1;  
    
    try {
        if(store.value.chosenID !== randomNumber) {
          axios
            .get(store.value.api + randomNumber)
            .then(resp => store.value.random.push(resp.data))
        }
      } catch(err) {
        console.error(err)
      }
    
    setTimeout(() => {
      store.value.isFighting = true
      displayWinner()
    }, 800);
  }

  onMounted(() => {    
    getChosenPoke()    
  })
</script>

<style lang="scss">
  .poke-fight {

    @media only screen and (max-width: 768px) {
      div[class*="col-md"] + div[class*="col-md"] {
        margin-top: 30px;
      }
    }
    
    &__action {
      padding-top: 40px;
      text-align: center;
    }

    @keyframes rotateRight {
      0% {
        transform: rotate(0);
      }
      50% {
        transform: rotate(45deg);
      }
      100% {
        transform: rotate(0deg);
      }
    }

    @keyframes rotateLeft {
      0% {
        transform: rotate(0);
      }
      50% {
        transform: rotate(-45deg);
      }
      100% {
        transform: rotate(0deg);
      }
    }

    &__left {
      .single-poke {
        &.isFighting {
          animation-name: rotateRight;
          animation-fill-mode: forwards;
          animation-duration: .4s;
        }
      }
    }

    &__right {
      .single-poke {
        &.isFighting {
          animation-name: rotateLeft;
          animation-fill-mode: forwards;
          animation-duration: .4s;          
        }
      }
    }
  }
</style>