<template>
<div class="container">
    <div class="darkmode" @click="toggleDark()">
      <i v-if="isDark" class="iconfont" >&#xe611;</i>
      <i v-else class="iconfont">&#xe601;</i>
      <!-- &#xe6ef -->
    </div>  
    <main>
      <Header v-model="wordSearch"  />
      <br>
      <Definition :word="wordSearch"/>
    </main>
</div>
</template>
<script setup>


import { toggleDark, isDark } from './composables';
import Header from "./components/Header.vue"
import Definition from "./components/Definition.vue";
import {ref, watch} from 'vue'
const wordSearch = ref()
// watch(wordSearch, (newValue) =>{
//   console.log(newValue)
// })
</script>


<style lang="scss" scoped>
div.container{
  height:100vh;
  overflow:  auto;
  // background-color: #282c34;
  main{
    max-height: 1000px;
    max-width: 800px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    // color:white;
    padding: 1rem;
    // position: inherit;
  }
  .darkmode{
    cursor: pointer;
    margin: 20px;
    max-width: 30px;
    max-height: 30px;
  }
}
</style>
