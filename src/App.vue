<script setup>
import Header from './components/Header.vue'
</script>

<template>
  <body class="h-screen bg-gradient-to-r from-purple-100 via-white to-blue-100 
  dark:h-screen dark:bg-gradient-to-r dark:from-indigo-600 dark:from-30% dark:via-black dark:to-purple-600">
    <Header></Header>
  <router-view></router-view>
  </body>
</template>