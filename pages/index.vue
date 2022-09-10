<template>

  <div>
    <v-alert
      :value="alertInvalid"
      shaped
      
      dark
      color="error"
    >
      Url not found or invalid
    </v-alert>

    <v-alert
      :value="alertWait"
      shaped
      
      dark
      color="info"
    >
      Please wait for comments to load. Most of the comments are labeled compliment and others.
    </v-alert>
    <v-alert
      :value="alertDone"
      
      outlined
      type="success"
    >
      Search success
    </v-alert>
    
    <div class="d-flex justify-center" width="auto">
      <v-text-field
          label="Video Url"
          v-model="url"
          hide-details="auto"
        ></v-text-field>

        <v-btn
          elevation="2"
          @click="search(url)"
        >Search</v-btn>

       
        <v-dialog
          v-model="dialog"
          width="500"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              class="ml-2"
              icon
              v-bind="attrs"
              v-on="on"
              outlined
              small
              fab
              color="indigo"
            >
              <v-icon>mdi-check</v-icon>
            </v-btn>
          
          </template>

          <v-card>
            <v-card-title class="text-h5 grey lighten-2">
              Submit Correction
            </v-card-title>

            <v-text-field
              class="ma-3"
              label="Signed by"
              v-model="signature"
              hide-details="auto"
            ></v-text-field>

            <v-divider></v-divider>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="primary"
                text
                @click="dialog = false"
              >
                Cancel
              </v-btn>
              <v-btn v-if="signature.length!=0"
                color="primary"
                text
                @click="submitCorrections(signature);dialog = false"
              >
                Submit
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
       


      

      
        
        
        

    </div>
    <br>
    <v-divider></v-divider>
    
    <div class ="d-flex justify-center">
      
      <div
        class="pa-3"
        width="450" v-if="youtubeData!=''" >
        
        
        <div>
          <iframe width="400" height="300" :src="youtubeData['url'] +'?feature=oembed'" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen title="MUSHROOM SOUP 3 WAYS SAKTO SA TAG ULAN!| Ninong Ry"></iframe> 

        </div>
        

      
        <div>
          
          <b>{{youtubeData['title']}}</b><br>
          {{youtubeData['author_name']}}<br>
        </div>
        
        
      </div>

    </div>
    <v-divider></v-divider>
    
    
    <v-tabs class='d-flex justify-center'
      v-model="active">

      <v-tab>SUGGESTION</v-tab>
      <v-tab>QUESTION</v-tab>
      <v-tab>SHOUTOUT</v-tab>
      <v-tab>COMPLIMENT</v-tab>
      <v-tab>OTHERS</v-tab>
    </v-tabs>
    <v-divider></v-divider>
    <br>
    

    <v-tabs-items v-model="active">
      <br>
      <v-tab-item>
        
        <v-row v-for="(comment, i) in comments"
        :key="i">

        
          <CommentCard v-if="comment.class_label=='suggestion'"
            @receiveCorrection = "addCorrections"
            :text= comment.text
            :photo=comment.photo
            :author=comment.author
            :time=comment.time
            :class_label=comment.class_label
          />
        </v-row>
        
      </v-tab-item>

      <v-tab-item>
        <v-card flat>
          <v-row v-for="(comment, i) in comments"
        :key="i">

        
          <CommentCard v-if="comment.class_label=='question'"
          @receiveCorrection = "addCorrections"
            :text= comment.text
            :photo=comment.photo
            :author=comment.author
            :time=comment.time
            :class_label=comment.class_label
          />
        </v-row>
        </v-card>
      </v-tab-item>

      <v-tab-item>
        <v-row v-for="(comment, i) in comments"
        :key="i">

        
          <CommentCard v-if="comment.class_label=='shoutout'"
            @receiveCorrection = "addCorrections"
            :text= comment.text
            :photo=comment.photo
            :author=comment.author
            :time=comment.time
            :class_label=comment.class_label
          />
        </v-row>
      </v-tab-item>

      <v-tab-item>
        <v-row v-for="(comment, i) in comments"
        :key="i">

        
          <CommentCard v-if="comment.class_label=='compliment'"
          @receiveCorrection = "addCorrections"
            :text= comment.text
            :photo=comment.photo
            :author=comment.author
            :time=comment.time
            :class_label=comment.class_label
          />
        </v-row>
      </v-tab-item>

      <v-tab-item>
        <div class="ma-10">
        <v-row v-for="(comment, i) in comments"
        :key="i">

        
          <CommentCard v-if="comment.class_label=='others'"
            @receiveCorrection = "addCorrections"
            :text= comment.text
            :photo=comment.photo
            :author=comment.author
            :time=comment.time
            :class_label=comment.class_label
          />
        </v-row>
      </div>
      </v-tab-item>
     
      
    </v-tabs-items>


  </div>

 
  
</template>

<script>
import CommentCard from '~/components/CommentCard.vue'


import axios from 'axios'
export default {
  name: 'IndexPage',
  
  components:{
  
    CommentCard,

  },

  data () {
    return {
      url:"",
      signature:'',
      active:'',
      dialog:'',
      alertDone:false,
      alertWait:false,
      alertInvalid:false,
      youtubeData:"",
      corrections:[],
      comments:[{
          "cid": "UgyA4KKWFUokLTGo-TJ4AaABAg",
          "text": " Welcome to youtube comment labeler. It works by submitting a youtube video link then comments are loaded after a few seconds.  ",
          "time": "2 hours ago",
          "author": "Slade Wilson",
          "channel": "UC9DNF86O49sXZ5HMaeP5waw",
          "votes": 0,
          "photo": "https://yt3.ggpht.com/ytc/AMLnZu9jZ6Kw1RP_Kn-DhNfb2C62paOxAbVlLBOKMw=s176-c-k-c0x00ffffff-no-rj",
          "heart": false,
          "time_parsed": 1661261415.550021,
          "class_label": "suggestion"
        },{
          "cid": "UgzcLKVcqi6hyiBAWNZ4AaABAg",
          "text": "Most comments are labeled as compliment or others. Submitting corrections currently not available.",
          "time": "22 hours ago",
          "author": "Malcolm Merlyn",
          "channel": "UC0ZiMSPveyDxaxgsLhik2SA",
          "votes": 0,
          "photo": "https://yt3.ggpht.com/ytc/AMLnZu9jZ6Kw1RP_Kn-DhNfb2C62paOxAbVlLBOKMw=s176-c-k-c0x00ffffff-no-rj",
          "heart": false,
          "time_parsed": 1661189415.554021,
          "class_label": "question"
        },{
          "cid": "UgzcLKVcqi6hyiBAWNZ4AaABAg",
          "text": "Most comments are labeled as compliment or others. Submitting corrections currently not available.",
          "time": "12 hours ago",
          "author": "Ra's al Ghul",
          "channel": "UC0ZiMSPveyDxaxgsLhik2SA",
          "votes": 0,
          "photo": "https://yt3.ggpht.com/ytc/AMLnZu9jZ6Kw1RP_Kn-DhNfb2C62paOxAbVlLBOKMw=s176-c-k-c0x00ffffff-no-rj",
          "heart": false,
          "time_parsed": 1661189415.554021,
          "class_label": "shoutout"
        },{
          "cid": "UgzcLKVcqi6hyiBAWNZ4AaABAg",
          "text": "Most comment goes here",
          "time": "22 hours ago",
          "author": "Damien Darhk",
          "channel": "UC0ZiMSPveyDxaxgsLhik2SA",
          "votes": 0,
          "photo": "https://yt3.ggpht.com/ytc/AMLnZu9jZ6Kw1RP_Kn-DhNfb2C62paOxAbVlLBOKMw=s176-c-k-c0x00ffffff-no-rj",
          "heart": false,
          "time_parsed": 1661189415.554021,
          "class_label": "compliment"
        },{
          "cid": "UgzcLKVcqi6hyiBAWNZ4AaABAg",
          "text": "Most comment goes here",
          "time": "12 hours ago",
          "author": "Floyd Lawton",
          "channel": "UC0ZiMSPveyDxaxgsLhik2SA",
          "votes": 0,
          "photo": "https://yt3.ggpht.com/ytc/AMLnZu9jZ6Kw1RP_Kn-DhNfb2C62paOxAbVlLBOKMw=s176-c-k-c0x00ffffff-no-rj",
          "heart": false,
          "time_parsed": 1661189415.554021,
          "class_label": "others"
        },
      ],
    }
  },
  methods:{

    async search( urll ){
      
      
      
      this.alertWait = true
      this.alertDone = false
      function isValidHttpUrl(string) {
        let url;
        
        try {
          url = new URL(string);
        } catch (_) {
          return false;  
        }

        return url.protocol === "http:" || url.protocol === "https:";
      }

  

      let urlID = urll.split("=");
      let x;

      
      if( isValidHttpUrl(urll) && urlID.length==2 ){
        let sss;
        
        const headers = { 'content-type': 'application/json' , 'Access-Control-Allow-Origin' : '*'};
        await axios.get('https://noembed.com/embed?url='+ urll ,headers).then(function (response) {
          sss = response['data']
          sss['url'] = sss['url']+'?feature=oembed'
          sss['url'] = sss['url'].replace("watch?v=", "embed/")
        }).catch(function(error) {
          console.log(" error" )
          console.log(error)
        })

        this.youtubeData = sss
        
        urlID = urlID[1] ;
        let x;

        
        await axios.get('https://amiel-nlpmodel.herokuapp.com/get-comments/'+ urlID ,headers).then(function (response) {
          x = response['data']
        }).catch(function(error) {
          console.log(" error" )
          console.log(error)
        })

        this.comments = x;
        this.alertDone = true
        this.alertWait = false
        this.alertInvalid = false


        
      }else{
        this.alertInvalid = true
        this.alertWait = false
        return false
      }
    

    },

    addCorrections( c ){
      this.corrections.push( c )
    },

    async submitCorrections( signature  ){

      for (let i = 0; i < this.corrections.length; i++) {
        
        this.corrections[i]['signature'] = signature

      } 


      const headers = { 'content-type': 'application/json' , 'Access-Control-Allow-Origin' : '*'};
      await axios.post('https://amiel-nlpmodel.herokuapp.com/add-correction/' ,{'data':this.corrections} , headers).then(function (response) {
        console.log("done")  
      }).catch(function(error) {
        console.log(" error" )
        console.log(error)
      })

      this.corrections = [];
      console.log( this.corrections ) 


    }

  },
  computed: {           
      toHtml:function(){
          return this.youtubeData['html'];
      }
  }


}
</script>
