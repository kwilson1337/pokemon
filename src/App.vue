<template>
  <Header>
     <div v-if="$route.name === 'Fight'" class="back-arrow">       
       <router-link to="/">
        <i class="fa-solid fa-angle-left"></i>
       </router-link>
      </div>
  </Header>  
  <router-view />
</template>

<script setup>
import Header from './components/Header.vue';
</script>

<style lang="scss">
  .back-arrow {
    position: absolute;
    left: 0px;
    top: 50%;
    transform: translateY(-50%);       

    a {
       width: 40px;
      height: 40px;
      background-color: #fff;
      border-radius: 50%;
      color: #000;
      display: flex;
      align-items: center;
      justify-content: center;
      text-decoration: none;
      transition: .2s ease-in-out all;
      border: 1px solid transparent;
    }

    &:hover {
      a {
        border-color: rgba(0,116,236,.9294117647058824);
        color: rgba(0,116,236,.9294117647058824);
      }
    }
  }
</style>