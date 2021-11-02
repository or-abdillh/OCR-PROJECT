<template>
   
   <section class="wrapper">
      <h2>Extract text from an image</h2>
      
      <div class="input-wrapper" v-if="showUpload">
         <label for="file" class="custom-file-input">Upload</label>
         <input id="file" type="file" @change="getFile" />
         <p>Support format file .jpg .jpeg .png .bmp. Upload high resolution for better result</p>
      </div>
      
      <div class="preview-wrapper" v-if="showPreview">
         <div class="preview">
            <img ref="img" :src="src" width="240"  />
         </div>
         <button class="btn-action" @click="recognize" type="button">Recognize</button>
      </div>
      
      <div class="progress-wrapper">
         <p class="progress" ref="status"></p>
      </div>
      
      <div class="result-wrapper" v-if="showResult">
         <div class="result">
            <h4>Result :</h4>
            <p> {{ result }} </p>
         </div>
         <button class="btn-close" @click="btnClose" type="button">Close</button>
      </div>
      
   </section>
</template>

<style scoped lang="scss">
   
   @mixin buttons {
      padding: 1rem 1.75rem;
      border-radius: 13px;
      font-size: 1.2rem;
      color: white;
      font-weight: 500;
      box-shadow: 3px 3px 10px rgba(black, .25);
      transition: .3s ease-in-out;
   }
   
   .wrapper {
      padding: .75rem 1rem;
      
      .btn-close {
         border: 1.5px solid 82125;
         width: 100%;
         padding: .25rem .5rem;
         font-size: 1rem;
         border-radius: 8px;
         transition: .3s ease-in-out;
         
         &:active {
            border-color: white;
         }
      }
      
      h2 {
         text-align: center;
      }
      
      .input-wrapper {
         padding: 1rem;
         display: flex;
         justify-content: center;
         flex-wrap: wrap;
         
         p {
            display: inline-block;
            width: 100%;
            font-size: 1rem;
            text-align: center;
         }
         
         input[type=file] {
            display: none;
         }
         
         .custom-file-input {
            background: #1dbae2;
            @include buttons;
            
            &:active {
               box-shadow: 3px 3px 8px rgba(black, .25) inset;
               font-size: 1.35rem;
               letter-spacing: 1px;
            }
         }
      }
      
      .preview-wrapper {
         width: 100%;
         flex-wrap: wrap;
         display: flex;
         justify-content: center;
         
         .preview {
            width: 100%;
            text-align: center;
            
            img {
               border-radius: 12px;
            }
         }
         
         .btn-action {
            background: #17d12a;
            border: none;
            margin-top: 1rem;
            @include buttons;
            
            &:active {
               box-shadow: 3px 3px 8px rgba(black, .25) inset;
               font-size: 1.35rem;
               letter-spacing: 1px;
            }
         }
      }
      
      .result-wrapper {
         padding: 0 1rem;
         
         .result {
            display: inline-block;
            width: 100%;
            margin: auto;
            font-size: 1.25rem;
            line-height: 150%;
            text-overflow: break-all;
         }
      }
      
      .progress-wrapper {
         padding: .5rem;
         text-align: center;
         
         .progress {
            font-size: 1rem;
         }
      }
   }
   
</style>

<script setup>

   import { ref, watch } from 'vue'
   import { createWorker, PSM, OEM } from 'tesseract.js';
   
   //
   const showPreview = ref(false)
   const showResult = ref(false)
   const showProgress = ref(false)
   const showUpload = ref(true)
   
   //Preview image and get a file
   const src = ref('')
   
   //Show result
   const result = ref('')
   const img = ref(null)
   const status = ref('')
   
   //Btn close
   const btnClose = () => {
      showResult.value = false
      showUpload.value = true
      src.value = ''
      status.value.innerHTML = ''
   }

   const getFile = e => {
      
      //Init reader
      const reader = new FileReader()
      reader.onload = () => {
         src.value = reader.result  
      }
      reader.readAsDataURL(e.target.files[0])
      showPreview.value = true
      showUpload.value = false
      
   }
   
   const worker = createWorker({
     logger: m => status.value.innerHTML = `${m.status} : ${(m.progress * 100).toFixed(2)}%`
   })
   
   const recognize = async () => {
      
      await worker.load()
      await worker.loadLanguage('eng')
      await worker.initialize('eng', OEM.LSTM_ONLY)
      
      await worker.setParameters({
        tessedit_pageseg_mode: PSM.SINGLE_BLOCK,
      })
      
      const { data } = await worker.recognize(img.value)
      result.value = data.text
      showResult.value = true
      showPreview.value = false
      status.value.innerHTML = ''
   } 
   
</script>

