<template>
  <div class="parse">
    <div id="app">
    <div id="flex-container">
      <div id="converter">
      <img src="https://shop.svggest.co/wp-content/uploads/2018/06/png-logo-curve-300x132.png">
      <h3>Packing Slip Converter</h3>
      </div>
    </div>
    </div>
    <input 
      id="fileInput"
      type="file"
      @change="upload">
    <a
      @click='save'
      type='button'
      download >
      Convert & Download Package Slip
    </a>
    <!-- {{doc}} -->
    <div class="paper" v-for="(data, idx) in doc" :key="idx">
        <div class="content">
            <p class="kepada">Kepada:</p>
            <p>{{data.name}}</p>
            <p>Jl. H. Munajat 260D/126C RT06/06, Kebon Gedang, Kiaracondong, Bandung 40274</p>
            <p>Kec. Batununggal, Kota Bandung</p>
            <p>Telp. +6285793710841</p>
            <br>
            <p class="order">No. Order #0309389 (26/12/1990)</p>
            <p>1x Wallaby, 2x Deerwaves, 3x Fox</p>
            <p>NOTES: Wallaby item, deerwaves maroon & item, fox ochre + black + maroon</p>
            <br>
            <p>JNE - REG</p>
            <br>
            <p class="pengirim">PENGIRIM:<br>Svggest | 087746239603</p>
            <img src="https://shop.svggest.co/wp-content/uploads/2018/06/png-logo-curve-300x132.png">

        </div>
    </div>
  </div>
</template>

<script>
  import Papa from 'papaparse'
  import Blob from 'blob'
  import FileSaver from 'file-saver'

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
              that.doc = results.data
              console.log(that.doc)
            },
            error (errors) {
              console.log('error', errors)
            }
          })
        }
        reader.readAsText(fileToLoad)
      },
      save () {
        const blob = new Blob([this.parseJSONtoCSV()], { type: 'text/csv' })
        FileSaver.saveAs(blob, 'test.csv')
      },
      parseJSONtoCSV () {
        return Papa.unparse(this.doc)
      }
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

  .paper {
        width: 298px;
        height: 198px;
        background: white;
        margin-top: 30px;
        margin-left: 30px;
    }
    
  .content {
        font-size: 9px;
        font-family: 'Roboto', sans-serif;
        padding: 5px 10px;
        letter-spacing: 0.5px;
    }
    
  p {
        margin: 0;
        padding: 0;
    }
    
  .pengirim {
        display: inline-block;
    }
    
  img {
        display: inline-block;
        width: 20%;
        text-align: right;
        position: relative;
        left: 95px;
    }
    
    .order, .kepada {
        font-weight: bold;
    }
</style>
