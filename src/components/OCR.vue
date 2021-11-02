<template>
   <img ref="img" :src="src" width="240"  />
   <input type="file" @change="getFile" />
   <button @click="recognize" type="button">Recognize</button>
   <pre ref="pre"></pre>
</template>

<script setup>

   import { ref } from 'vue'
   import { createWorker, PSM, OEM } from 'tesseract.js';
   
   //Preview image and get a file
   const src = ref('')
   const getFile = e => {
      //Init reader
      const reader = new FileReader()
      reader.onload = () => {
         src.value = reader.result  
      }
      reader.readAsDataURL(e.target.files[0])
   }
   
   //Show result
   const pre = ref(null)
   const img = ref(null)
   
   const worker = createWorker({
     logger: m => console.log(m),
   })
   
   const recognize = async () => {
      await worker.load()
      await worker.loadLanguage('eng')
      await worker.initialize('eng', OEM.LSTM_ONLY)
      
      await worker.setParameters({
        tessedit_pageseg_mode: PSM.SINGLE_BLOCK,
      })
      
      const { data } = await worker.recognize(img.value)
      pre.value.innerHTML = data.text
   } 
   
</script>