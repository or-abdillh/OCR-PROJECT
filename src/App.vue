<template>
   <OCR></OCR>
   <Webcam></Webcam>
</template>

<script setup>
   import OCR from './components/OCR.vue'
   import Webcam from './components/Webcam.vue'
</script>
<style>
   
   body {
      background: #F4F4F4;
      margin: 0;
      padding: 0;
      font-size: 24px;
   }
   
   #app {
     font-family: Avenir, Helvetica, Arial, sans-serif;
     -webkit-font-smoothing: antialiased;
     -moz-osx-font-smoothing: grayscale;
     color: #2c3e50;
}
</style>
