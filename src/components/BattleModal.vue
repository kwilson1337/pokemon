<template> 
  <section     
    :data-win-loose="store.win"
    class="battle-modal"
    :class="{isActive : props.isActive}"             
>
      <div class="container">
          <div class="battle-modal__inner">     
              <h2>{{whoWins()}}</h2>                                     
              <div class="battle-modal__link">                  
                  <button :data-win-loose="store.win" @click="closeModal($event)" class="btn-primary">Play Again</button>
              </div>
          </div>
      </div>
  </section>
</template>

<script setup>
import { computed, onMounted, ref } from "@vue/runtime-core"
import { defineEmit } from 'vue'

    const props = defineProps({        
        chosen: Object,
        opponent: Object,            
        isActive: {
            type: Boolean,
            default: false
        }    
    })    

    const store = ref({
        win: false
    })
    
    const emit = defineEmits(['closeModal'])
    const closeModal = (winLoose) => {
        emit('closeModal', winLoose)        
    }                

    const whoWins = () => {
        if(props.chosen.length && props.opponent.length) {
            let chosenXP = ''
            let opponentXP = ''
            props.chosen.forEach(chosen => {
                chosenXP = chosen.base_experience
            })

            props.opponent.forEach(opponent => {
                opponentXP = opponent.base_experience
            })

            if(chosenXP > opponentXP) {
                store.value.win = true
                return 'You Won! Fight Again'
            } else {
                store.value.win = false
                return 'You Lost :('
            }
        }
    }    
</script>

<style lang="scss">
@import "/src/assets/styles/style.scss";
@import "/src/assets/styles/_variables.scss";

.battle-modal {   
    position: fixed;
    width: 100%;
    height: 100%;
    left: 0px;
    top: 0px;
    z-index: 10;
    display: flex;
    justify-content: center;
    align-items: center;
    opacity: 0;
    visibility: hidden;      
    background-color: rgba($black, .5);
    transition: .3s ease-in-out all;
    transition-delay: .5;

    &.isActive {        
        opacity: 1;
        visibility: visible;        
        transition-delay: .5;
        
        .battle-modal__inner {
            transform: scale(1);
        }
    }

    &__inner {
        background-color: $white;
        max-width: 500px;
        width: 100%;
        padding: 20px;
        margin: auto;
        border-radius: 5px;
        transform: scale(0);
        transition: .3s ease-in-out all;

        h2 {
            text-align: center;
            padding: 20px 0px;
        }
    }

    &__link {
        padding-top: 20px;

        button {
            width: 100%;
        }
    }
}
</style>