<template>
<header>
<h2 class = "title">{{ wordValue?wordValue: "Word Dictionary"}} </h2>
<div class = "input-wrap" :class="{ dark: isDark }">
<input type = "text" :class="{ dark: isDark }" placeholder="Search A word" v-model = "wordValue" @input = "$emit('update:modelValue', $event.target.value)" />


</div>
</header>
</template>

<script setup>

import { isDark } from '../composables';
import {ref} from 'vue'
const wordValue = ref()

defineEmits(['update:modelValue']);

</script>

<style lang="scss" scoped>
header{
    height: 30vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    justify-content: space-evenly;
    h2{
        font-size: 5vw;
        font-weight: 500;
        @media (max-width: 1000px){
            font-size: 11vw;
        }
    }
    .input-wrap{
        input{
            width : 50vw;
            height: 40px;
            width: min-content;
            border: none;
            // background-color: #282c34;
            border-bottom: 2px solid rgba(197, 188, 188);
            padding : 5px;
            // color: white;
            font-size:25px;
            &:focus{
                outline: none;
                // border-bottom: 2px solid white;
            }
        }
        
    }
} 
</style>