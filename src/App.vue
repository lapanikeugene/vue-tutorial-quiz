<script setup lang="ts">
  import {ref, computed} from "vue";
  const questions = ref([
    {question:"What is Bahnhof?", 
    answer:0, 
    options:["Railway station","supermarket","pupil","school"],
    selected:null}
  ])
</script>

<template>
  <h1>
    Hello World!
  </h1>
</template>

<style scoped>
*{
  margin:0;
  padding: 0;
  box-sizing: border-box;
}
body{
  background-color: #456b7c;
  color: white;
}
</style>
