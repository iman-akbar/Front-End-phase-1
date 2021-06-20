<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <h1>IMAN AKBAR</h1>
    </v-app-bar>
  <v-row>
    <v-col v-for="(data,i) in listData" :key='i'>
      <v-card>
        <v-card-title>{{data.title}}</v-card-title>
        <v-card-text>
          <p>{{data.item}}</p>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
   <v-btn
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
                  </b-form-group>
    <v-main>
      <router-view/>
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
    foto:null,}
  }),
  mounted(){
    axios.get(`URL`)
    .then((res)=>{
      this.listData= res.data
    })
  },
  methods: {
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
