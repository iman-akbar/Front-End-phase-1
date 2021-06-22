<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <h1>IMAN AKBAR</h1>
    </v-app-bar>
    <v-card>
      <v-card-title>Location</v-card-title>
      <v-card-text>
        <p>
          {{ location.region }}
          {{ location.postalCode }}
          {{ location.country }}
          {{ location.city }}
        </p>
      </v-card-text>
    </v-card>
    <v-row>
      <v-col v-for="(domainsData, i) in domainsData" :key="i">
        <v-card>
          <v-card-title>{{ domainsData.data }}</v-card-title>
          <!-- <v-card-text>
          <p>{{data.item}}</p>
        </v-card-text> -->
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-card>
        <v-card-title>dataAs</v-card-title>
        <v-card-text>
          <v-row> Name: {{ dataAs.name }} </v-row>
          <v-row> route: {{ dataAs.route }} </v-row>
        </v-card-text>
      </v-card>
    </v-row>
    <!-- <v-btn
                    x-large
                    color="success"
                    outlined
                   @click="newData"
                    block
                    >Add New</v-btn
                  >
                  <v-text-field v-show="input" v-model="param.title"></v-text-field>
                   <b-form-group class="text-label" v-show="input">
                    <b-form-file
                      @change="upload($event)"
                      accept="image/*"
                      
                      ref="fileinput"
                    ></b-form-file>
                  </b-form-group> -->
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  name: 'App',

  data: () => ({
    //
    listData:[],
    param:{title:'',
    foto:null,},
    dataAs:{},
    domains:{},
    domainsData:[],
    location:{}
  }),
  mounted(){
    this.getData()
    this.getLOcate()
    this.getLocateByIP()
    this.getVisit1()
  },
  methods: {
    getLocateByIP(){
      fetch('https://api.ipify.org?format=json')
      .then(result => result.json())
      .then(data => console.log(data.ip))
    },
    getLOcate(){
      console.log("Cookies: " + navigator.cookieEnabled);
console.log("Browser Language: " + navigator.browserLanguage);
console.log("Language: " + navigator.language);
console.log("Platform: " + navigator.platform);
console.log("Connection Speed: " + navigator.connectionSpeed);
console.log("User Agent: " + navigator.userAgent);
console.log("Webdriver: " + navigator.webdriver);
console.log("Geolocation: " , navigator.geolocation);
    },
    getvisitByNpm(){
  //     this.$getLocation(options)
  // .then(coordinates => {
  //   console.log(coordinates);
  // });

    },
    getVisit1(){
    //   navigator.geolocation.getCurrentPosition(function(position){
    //     console.log("longitude:"position.coords.longitude)
    //     console.log("latitude:"position.coords.latitude)
    // })
    let check = (position) =>{
      const{latitude,longitude} = position.coords;
      fetch(`https://api.opencagedata.com/geocode/v1/json?q=${latitude}+${longitude}&key=d8ed7c30fde54744bf25c812465d0115`)
      .then(response => response.json())
      .then(console.log)
    }
    navigator.geolocation.getCurrentPosition(check, console.log)
    },
    getData(){
      axios.get(`https://geo.ipify.org/api/v1?apiKey=at_IRAUZj8fPx2gHBdlZfU6wt0AiuWpL&ipAddress=8.8.8.8`)
    .then((res)=>{
      console.log(res);
      this.dataAs = res.data.as
      this.domains = res.data.domains
      this.location = res.data.location
      console.log(this.dataAs);
      console.log(this.domains);
      for(let i = 0 ; i < this.domains.length ; i++){
        this.domainsData.push({
          data:this.domains[i]
        })
      }
      console.log(this.domainsData);
      console.log(this.location);
      // this.listData= res.data
    })
    .catch((err)=>console.log(err))
    },
    upload(event) {
      console.log("VALUE " + JSON.stringify(event));
      this.foto = event.target.files[0];
    },
    newData(){
      axios.post(`URL`, this.param)
    .then((res)=>{
      this.listData.push(res.data)

    })
    .catch((err)=>console.log(err))
    }
  },
};
</script>
