<template>
   <section class="wrapper">
      <video id="camera" width="480px" height="240px" autoplay="autoplay"></video>
      <canvas id="canvas"></canvas>
      <audio id="audio" preload="auto" src=""></audio>
   </section>
</template>

<script setup>
   
   import Webcam from 'webcam-easy'
   import { ref } from 'vue'
   
   //Get elements
   const canvas = document.querySelector('#canvas')
   const camera = document.querySelector('#camera')
   const audio = document.querySelector('#audio')
   
   console.log(canvas, camera, audio)
   
   //Init
   //const webcam = new Webcam(camera, 'user', canvas, audio)
   /*webcam.start()
      .then( res => console.log(res) )
      .catch( err => console.log(err) )
   */
</script>