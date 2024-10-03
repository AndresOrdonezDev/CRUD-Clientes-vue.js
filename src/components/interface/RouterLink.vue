<script setup>
import {RouterLink} from 'vue-router'
defineProps({
    to:{
        type:String,

    }
})
</script>

<template>
    <router-link 
        :to="{name:to}"
         class="block md:inline-block rounded bg-indigo-800 py-2 px-3 text-white font-bold hover:bg-indigo-700 shadow-md"
    >
        <slot></slot>
    </router-link>
</template>


