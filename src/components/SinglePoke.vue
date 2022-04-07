<template>        
    <div               
        class="single-poke"    
        :class="{isFighting : props.isFighting}"            
    >        
        <div class="single-poke__inner">
            <div class="single-poke__img">
                <img :src="props.data.sprites.front_shiny" :alt="props.data.name" class="img-fluid">
            </div>
            <div class="single-poke__content">                                                
                <p class="title"><strong>{{props.data.name}}</strong></p>                 
                <p v-if="props.data.types.length" class="types">
                    <span><strong>Type:</strong></span>                    
                    <span>{{getTypes(props.data.types)}}</span>                                      
                </p>
                 
                    
                <div class="single-poke__link">
                    <router-link 
                        v-if="props.fight"
                        class="btn-primary d-block" 
                        :to="`/fight/${props.data.name}`">
                            I choose you, {{props.data.name}}!
                    </router-link>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from "@vue/reactivity"
import { onMounted } from "@vue/runtime-core"
import axios from 'axios'

const props = defineProps({
    data: Object,
    index: Number,
    fight: {
        type: Boolean,
        default: true
    },
    isFighting: {
        type: Boolean,
        default: false
    }
})

const getTypes = (types) => {    
    const pokeType = ref([])
    types.forEach(single => {
        pokeType.value.push(single.type.name)
    })      
    return pokeType.value.join(', ')
}
</script>

<style lang="scss">
@import "/src/assets/styles/style.scss";
@import "/src/assets/styles/_variables.scss";

.single-poke {
    border-radius: 6px;    
    box-shadow: 1px 1px 9px rgba($black, .4);
    border: 1px solid #e3e3e3;
    transition: .2s ease-in-out all;
    display: flex;

    &:hover {
        box-shadow: 1px 1px 9px rgba($color3, .4);
        border-color: $color3;
    }

    &.-active {
        transform: scale(1.1);        
    }   

    &__inner {
        width: 100%;
        display: flex;
        flex-direction: column;
    }

    &__img {
        padding: 10px 10px;
        background: #e3e3e3;
        text-align: center;

        img {
            height: 115px;
        }
    }
    
    &__content {
        padding: 20px 20px 20px;
        text-align: left;
        display: flex;
        flex-direction: column;
        flex-grow: 1;

        .title {
            font-size: 20px;
            text-transform: capitalize;
            margin-bottom: 10px;
        }

        .types {
            span {               
                & + span {
                    padding-left: 5px;
                }
            }
        }
    }

    &__link {
        display: flex;
        flex-grow: 1;
        align-items: flex-end;

        a {
            width: 100%;
        }
    }
}
</style>