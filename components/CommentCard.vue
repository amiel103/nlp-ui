<template>
  <v-card
    class="mx-auto pa-5 ma-2"
    width="450"
    outlined
    elevation="5"
  >
    

    <div align="center">
     
        <v-card-text>
          <b>{{class_label}}</b>
        </v-card-text>
    </div>

    <v-divider></v-divider>

    <div class="d-flex justify-start mt-3">
   
      <div class="">
        <v-avatar
          size="50px"
        >
          <img
            alt="Avatar"
            :src='photo'>
          
        </v-avatar>
      </div>

      <div class="ml-3">
        <div>
          <b>
            {{author}}
          </b>
            {{"&nbsp &nbsp" +time}}
            <br>
            {{text}}
    
        </div>
      </div>
    </div>

    
    
    <v-dialog
      class="mt-5"
      v-model="dialog"
      width="650"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          class="mt-5"
          
          dark
          v-bind="attrs"
          v-on="on"
          outlined
          color="indigo"
        >
          MODIFY
        </v-btn>
      </template>
      <v-card>
        <v-card-title class="d-flex justify-center">
          <span class="text-h5">MODIFY LABEL</span>
        </v-card-title>

        <v-card-text>
          <v-radio-group
            class="d-flex justify-center"
            v-model="radioData"
            mandatory
            row
          >

            <v-radio
              label="SUGGESTION"
              value="suggestion"
            ></v-radio>
            <v-radio
              label="QUESTION"
              value="question"
            ></v-radio>
            <v-radio
              label="SHOUT OUT"
              value="shoutout"
            ></v-radio>
            <v-radio
              label="COMPLIMENT"
              value="compliment"
            ></v-radio>
            <v-radio
              label="OTHERS"
              value="others"
            ></v-radio>

          </v-radio-group>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            text
            @click="dialog = false"
          >
            Disagree
          </v-btn>
          <v-btn
            color="green darken-1"
            text
            @click= "$emit('receiveCorrection',    getCommentData()); dialog = false"
          >
            Agree
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

  
    
    
    
  
  
  </v-card>
</template>
<script>

export default {
  props:[
    "cid",
    "text",
    "time",
    "author",
    "channel",
    "votes",
    "photo",
    "heart",
    "time_parsed",
    "class_label",
  ],
  
  data () {
    return {
      dialog:'',
      radioData:this.class_label,
      commentData:{
          "cid":this.cid,
          "text": this.text,
          "time": this.time,
          "author": this.author,
          "channel": this.channel,
          "votes": this.votes,
          "photo": this.photo,
          "heart": this.heart,
          "time_parsed": this.time_parsed,
          "class_label": this.radioData
        }
    }
  },
  methods:{

    getCommentData(){
      let commentData = {
        "cid":this.cid,
        "text": this.text,
        "time": this.time,
        "author": this.author,
        "channel": this.channel,
        "votes": this.votes,
        "photo": this.photo,
        "heart": this.heart,
        "time_parsed": this.time_parsed,
        "class_label": this.radioData
      }
      return commentData
    }
  }

}
</script>

<style>


</style>
