<template>
  <div class="parse">
    <div id="app">
    <div id="flex-container">
      <div id="converter">
      <img src="https://shop.svggest.co/wp-content/uploads/2018/06/png-logo-curve-300x132.png">
      <h4>Packing Slip Converter</h4>
      </div>
    </div>
    </div>
    <input 
      id="fileInput"
      type="file"
      @change="upload">
    <br>
    <a
      id ="print"    
      @click='save'
      type='button'
      download >
      Convert & Print Package Slip
    </a>
  </div>
</template>

<script>
  import Papa from 'papaparse'
  // import Blob from 'blob'
  // import FileSaver from 'file-saver'

  export default {
    name: 'parse',
    data () {
      return {
        doc: null
      }
    },
    methods: {
      upload (e) {
        const that = this
        const fileToLoad = event.target.files[0]
        const reader = new FileReader()
        reader.onload = fileLoadedEvent => {
          Papa.parse(fileLoadedEvent.target.result, {
            header: true,
            complete (results) {
              console.log('complete', results)
              that.doc = JSON.stringify(results.data, null, 2)
              console.log(that.doc)
            },
            error (errors) {
              console.log('error', errors)
            }
          })
        }
        reader.readAsText(fileToLoad)
      }
      // save () {
      //   const blob = new Blob([this.parseJSONtoCSV()], { type: 'text/csv' })
      //   FileSaver.saveAs(blob, 'test.csv')
      // },
      // parseJSONtoCSV () {
      //   return Papa.unparse(this.doc)
      // }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .body {
    display: flex;
    justify-content: center;
    margin-top: 30px;
  }

  .entry {
    width: 40%;
  }

  .entry-result {
    width: 100%;
    height: 50vh;
  }

  .preview {
    width: 40%;
    text-align: left;
  }

  #print {
    padding: 10px 20px;
    margin-top: 20px;
  }

  input {
    margin-top: 20px;
  }


</style>
