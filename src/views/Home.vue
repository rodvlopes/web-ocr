<template>
  <q-page>
    <q-progress :percentage="progress" color="info" height="10px" />
    <div class="row justify-center">
      <div class="col-8">
        <q-uploader multiple :url="picUploadUrl" @add="onAddFiles" />
        <q-select v-model="lang" :options="langOptions" />
      </div>
    </div>
    <div class="row justify-center">
      <div class="col-8">
        <img ref="imgToRecognize" src="../assets/ocr_sample.png">
      </div>
    </div>
    <div class="row justify-center">
      <div class="col-8">
        <q-input v-model="result" inverted
        type="textarea" float-label="Result"
        :max-height="100"
        rows="7"
        />
      </div>
    </div>
  </q-page>
</template>

<style>
</style>

<script>
import Tesseract from 'tesseract.js'

export default {
  name: 'PageHome',
  data() {
  	return {
  		progress: null,
  		result: '',
      picUploadUrl: '',
      lang: 'eng',
      langOptions: [
        {label: 'English', value: 'eng'},
        {label: 'Portuguese', value: 'por'}
      ]
  	}
  },
  mounted() {
  	console.log(this.$refs.imgToRecognize.src)
  	console.log(Tesseract.workerOptions)
  	Tesseract.workerOptions.workerPath = Tesseract.workerOptions.workerPath.replace('dist/worker.dev', 'worker')
  	this.recognize(this.$refs.imgToRecognize.src)
  },
  methods : {
    onAddFiles(files) {
      console.log(files)
      if (files && files.length) {
        this.recognize(files[0])
      }
    },
  	recognize(img) {
  		Tesseract.recognize(img, { 
        lang: this.lang
      }).progress(message => {
	    	console.log(message)
	    	this.progress = message.progress * 100
	    })
	    .catch(err => console.error(err))
	    .then(result => {
	    	console.log(result)
	    	this.result = result.text
	    })
	    .finally(resultOrError => console.log(resultOrError))
  	}
  }
}
</script>

<style scoped>
textarea {
	overflow-y: auto !important;
}
</style>