<template>
  <div id="app">
<!-- 
    <button v-on:click="fetch1mb()">   fetch json file 1 mb </button>
        <button   v-on:click="fetch15mb()" >   fetch json file 15 mb </button>
        <button   v-on:click="download15mb()" >    download file 15 mb </button> -->
    <!--     the one to test -->
        <button   v-on:click="initBoard()" >    download file blob 15 mb </button>

    <div v-if="dataTab">
        <div v-for="(row,index) in dataTab" :key="index">    {{row}} </div>


    </div>


  </div>
</template>


<script>

// Normally you can fetch a first time and then see the data appears on the Dom
// Then if you scroll a bit, and try to click on the button again, it should be slow or even freezing.
//If you try to close the page it also freeze
import HelloWorld from "./components/HelloWorld.vue";
import { constants } from "zlib";

export default {
  name: "App",
  components: {
    HelloWorld
  },
  data() {
    return {
      dataTab:null
    }},
  methods: {


async initBoard(){
    const urlBlob = await this.download15mbBlob()
    console.log(urlBlob,'Blob URL');
    this.dataTab = await this.renderTab(urlBlob)
},


 async renderTab(file){
  return new Promise((resolve, reject) => {
    console.log(file,'the blob file to parse');
    const reader = new FileReader();
    reader.addEventListener('loadend', (e) => {
      console.log('listening to the reader method');
      const text = e.srcElement.result;
      console.log(text,'the text file to parse to JSON');
      const data = JSON.parse(text)
      console.log(data,'The Json data');
      if(data){
        resolve(data)
      }else{
        reject('no data in the blob')
      }  
     })
    reader.readAsText(file); 
     })
 },
      async download15mbBlob() {
      console.log("FETCHING");
      const method = "GET";
        const apiUrl = "https://s3.eu-central-1.wasabisys.com/owave-europe/acs2017_census_tract_data.json"
      const blob = await fetch(apiUrl)
        .then(response => response.blob())
        .then(blob =>  {
          console.log(blob,'the blob file received from the get request');
          return blob
        })
        return blob
    },
  /*   async download15mb() {
      console.log("fetch 1");
      const method = "GET";
      const apiUrl =
        "https://s3.eu-central-1.wasabisys.com/owave-europe/acs2017_census_tract_data.csv";
      fetch(apiUrl)
        .then(res => res.blob())
        .then(blob => {
          var file = window.URL.createObjectURL(blob);
          window.location.assign(file);
        });
    },
    async fetch15mb() {
      console.log("fetch 1");
      const method = "GET";
      const apiUrl =
        "https://s3.eu-central-1.wasabisys.com/owave-europe/acs2017_census_tract_data.csv";
      fetch(apiUrl)
        .then(res => res.text())
        .then(text => {
          console.log(text);
             console.log(text.json(),'the json text');
          return text;
        });
    },
    async fetch1mb() {
      const method = "GET";

     const apiUrl =
        "https://s3.eu-central-1.wasabisys.com/owave-europe/acs2017_census_tract_data.csv";
      let request = await fetch(apiUrl, {
        method,
        headers: {}
      })
        .then(response => response.json())
        .then(json => {
          console.log(json, "THE JSON RECEIVED  1 mb ");
          return json;
        });

      console.log("fetch 2");
    } */
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
