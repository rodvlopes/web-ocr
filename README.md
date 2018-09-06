# OCR Web

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

# To Make Tesseract Work Properly Offline

    cp node_modules/tesseract.js/dist/worker.js public/
    
    curl -k https://cdn.rawgit.com/naptha/tesseract.js-core/0.1.0/index.js -o public/tesseract.js-core.js

    mkdir public/tessdata
    
    curl -k https://raw.githubusercontent.com/naptha/tessdata/gh-pages/3.02/eng.traineddata.gz -o public/tessdata/eng.traineddata.gz
    
    curl -k https://raw.githubusercontent.com/naptha/tessdata/gh-pages/3.02/por.traineddata.gz -o public/tessdata/por.traineddata.gz


# Refs

* https://github.com/naptha/tesseract.js
* https://quasar-framework.org/components/
* https://dev.to/lexmartinez/creating-a-simple-ocr-application-with-electron-vuejs--tesseractjs-bnk