<template>
  <q-page>
    <q-progress :percentage="progress" color="info" height="10px" />
    <img alt="Quasar logo" src="../assets/logo.png">
    <img ref="imgToRecognize" src="../assets/ocr_sample.png">
    <div class="row">
	    <q-input v-model="result" inverted
			type="textarea" float-label="Result"
			:max-height="100"
			rows="7"
		/>
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
  		result: ''
  	}
  },
  mounted() {
  	console.log(this.$refs.imgToRecognize.src)
  	console.log(Tesseract.workerOptions)
  	Tesseract.workerOptions.workerPath = Tesseract.workerOptions.workerPath.replace('dist/worker.dev', 'worker')
  	this.recognize(this.$refs.imgToRecognize.src)
  },
  methods : {
  	recognize(img) {
		Tesseract.recognize(img)
		    .progress(message => {
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