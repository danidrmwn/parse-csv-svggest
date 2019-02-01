<template>
  <div class="parse">
    <div class="main" id="app">
      <div id="flex-container">
        <div id="converter">
          <img class="main-logo" src="https://shop.svggest.co/wp-content/uploads/2018/06/png-logo-curve-300x132.png">
          <h5>Packing Slip Converter</h5>
        </div>
        <div class="file-input">
          <b-form-file @change="upload" v-model="file" :state="Boolean(file)" placeholder="Choose a file..."></b-form-file>
        <hr>
        </div>
        <div class="print-button">
          <b-button @click='print'>Print!!!</b-button>
        </div>
    </div>
    </div>
    <!-- <input 
      id="fileInput"
      type="file"
      @change="upload"> -->
    <!-- <a
      @click='print'
      type='button'
      download >
      PRINT!!!
    </a> -->
    <div class="printarea" id="printarea">
    <div class="paper" v-for="(data, idx) in doc" :key="idx">
        <hr class="shadow-line">
        <div class="content">
            <p class="kepada">Kepada:</p>
            <p>{{data.name}}</p>
            <p>{{data.address}}, {{data.district}}, {{data.city}}</p>
            <p>Telp. {{data.phone}}</p>
            <p class="order">No. Order #{{data.order_id}}</p>
            <p>{{data.variation}}</p>
            <p>NOTES: {{data.Warna}}</p>
            <p class="kurir">{{data.courier}}</p>
            <p class="pengirim">PENGIRIM:<br>Svggest | 087746239603</p>
            <img class="logo-print" src="https://shop.svggest.co/wp-content/uploads/2018/06/png-logo-curve-300x132.png">
        </div>
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
      print () {
        var printContents = document.getElementById('printarea').innerHTML
        var originalContents = document.body.innerHTML
        document.body.innerHTML = printContents
        window.print()
        document.body.innerHTML = originalContents
      },
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
<style>
  #app {
    margin-top: 0!important;
    padding-top: 30px;
  }

  .main-logo {
    width: 10%;
  }

  html, body {
    background: #efefef;
  }

  @font-face {
    font-family: 'warlow-sans';
    src: url(warlow-sans.otf);
    font-style: normal;
    font-weight: 100;
  }

  h5 {
    font-family: warlow-sans;
    margin: 20px 0px;
    letter-spacing: 1px;
  }

  .file-input {
    width: 30%;
    margin: 0 auto;
  }

  #__BVID__3__BV_file_control_ {
    text-align: left;
  }

  .print-button {
    margin: 20px 0;
    font-family: warlow-sans;
  }

  .btn-secondary {
    letter-spacing: 2px;
    background-color: #197519;
    border-color: #197519;
  }

  .btn-secondary:hover {
    background-color: #0f580f;
    border-color: #0f580f;
  }

  hr {
    border-color: #b1b1b1;
  }
  /* .body {
    display: flex;
    justify-content: center;
    margin-top: 30px;
  } */

  .paper {
        text-align: left;
        width: 15.75cm;
        height: 10.5cm;
        /* margin-bottom: 0.5cm; */
        position: relative;
        background-color: white;
    }
    
  .content {
        font-size: 16px;
        font-family: warlow-sans;
        padding: 10px 10px;
        letter-spacing: 0.5px;
        line-height: 1.6;
    }
    
  p {
        margin: 0;
        padding: 0;
    }
    
  .pengirim {
        display: inline-block;
        position: absolute;
        bottom: 20px;
    }
    
  .order, .kepada, .kurir {
        font-weight: bold;
    }

  .logo-print {
      display: inline-block;
      width: 20%;
      position: absolute;
      right: 30px;
      bottom: 20px;
    }
  
  .shadow-line {
    padding: 0;
    margin: 0;
    border-color: #b0b0b0;
  }

  @media print {
    .main, .shadow-line {
      display: none;
    }

    #printarea {
      display: block;
    }
  }

  @page {
    size: auto;
    margin: 0mm;
  }
</style>
