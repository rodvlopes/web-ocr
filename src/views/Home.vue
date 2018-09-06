<template>
  <q-page>
    <q-progress :percentage="progress" color="info" height="10px" />
    <div class="row justify-center my-gutter">
      <div class="col-sm-4 col-xs-12">
        <q-uploader hide-upload-button
        ref="uploader"
        float-label="Select an Image"
        :url="picUploadUrl" 
        @add="onAddFiles" />
      </div>
      <div class="col-sm-4 col-xs-12">
        <q-select class="" v-model="lang" :options="langOptions" float-label="Select a Language" />
      </div>
      <div class="col-sm-8 col-xs-12 bg-primary text-center">
        <img v-show="hasPic" class="fit" ref="imgToRecognize" src="../assets/logo.png" />
        <q-icon v-if="!hasPic" name="photo_size_select_actual" color="white" size="3rem" />
      </div>
      <div class="col-sm-8 col-xs-12">
        <q-input v-model="result"
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
      hasPic: false,
      lang: 'eng',
      langOptions: [
        {label: 'English', value: 'eng'},
        {label: 'Portuguese', value: 'por'}
      ]
  	}
  },
  mounted() {
  	console.log(Tesseract.workerOptions)
    let workerPath = Tesseract.workerOptions.workerPath.replace('dist/worker.dev', 'worker')
    Tesseract.workerOptions.workerPath = workerPath
    Tesseract.workerOptions.langPath = workerPath.replace(/worker.*/, 'tessdata/')
    Tesseract.workerOptions.corePath = workerPath.replace(/worker.*/, 'tesseract.js-core.js')
    console.log(Tesseract.version);
  },
  methods : {
    onAddFiles(files) {
      if (files && files.length === 1) {
        this.hasPic = true
        this.$refs.imgToRecognize.src = files[0].__img.src
        this.recognize(files[0])
      }
      else {
        this.$q.notify('Only the first image will be processed')
      }
      this.$refs.uploader.reset()
    },
    recognize(img) {
  		Tesseract.recognize(img, { 
        lang: this.lang
      }).progress(message => {
	    	console.log(message)
	    	this.progress = message.progress * 100
        this.$q.loading.show({
          message: message.status,
        })
	    })
	    .catch(err => console.error(err))
	    .then(result => {
	    	console.log(result)
	    	this.result = result.text
	    })
	    .finally(resultOrError => {
        console.log(resultOrError, this.$q.loading)
        this.$q.loading.hide()
      })
  	}
  }
}
</script>

<style scoped>
textarea {
	overflow-y: auto !important;
}
.my-gutter>div {
  padding: 4px;
}
</style>