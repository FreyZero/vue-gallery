<template>
  <div class="container">
        <div class='box' style="margin-top:10px;">
                <textarea placeholder="Picture content..." class="textarea" rows=1 style="display:inline-block"></textarea>
                <button @click="onUpload" class="button is-primary" style="float:right;display:inline-block">Post</button>
        </div>
        
        <section class="section">  
        <div class="container" style="margin-top:15px">
            <input @change="onFileSelect" type="file">
            
        </div>
        </section>
        <div class="container">
            <div id="columns">
                <div v-for="(obj,index) in postArray" :key="index" class="post">
                    <div class="card">
                        <div class="card-image">
                            <figure class="image inbox">
                                <img :src="obj.imgURL" alt="Placeholder image">
                            </figure>
                        </div>
                        <div class="card-content">
                            <div class="media">
                                <div class="">
                                    <p class="title is-4">BNK48</p>
                                    <p class="subtitle is-6">index@{{index}}</p>
                                </div>
                            </div>
                            <div class="content">
                                Lorem ipsum dolor sit amet, consectetur adipiscing elit.
                                Phasellus nec iaculis mauris. <a>@bulmaio</a>.
                                <a href="#">#css</a> <a href="#">#responsive</a>
                                <br>
                                <time datetime="2016-1-1">11:09 PM - 1 Jan 2016</time>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
      </div>
    <div class="modal">
        <div class="modal-background"></div>
        <div class="modal-content">
            <p class="image is-4by3">
            <img src="https://bulma.io/images/placeholders/1280x960.png" alt="">
            </p>
        </div>
        <button class="modal-close is-large" aria-label="close"></button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import _ from 'lodash'

export default {
  data() {
      return {
          resData: 'Waiting for response data',
          resImgURL: 'https://bulma.io/images/placeholders/128x128.png',
          selectedFile: null,
          post:{
              topic: 'dummy topic',
              content: 'dummy content',
              name: 'BNK48',
              imgURL: ''
          },
          postArray : []
      }
  },
  methods: {
      onFileSelect(event) {
          this.selectedFile = event.target.files[0]
          console.log(event)
      },
      onUpload() {
          const fd = new FormData()
          fd.append('image', this.selectedFile, this.selectedFile.name)  
          axios.post('https://us-central1-ratchaponproject.cloudfunctions.net/uploadFile', fd)
          .then(res => {
              console.log(res)
              this.resData = res.data.message
              this.resImgURL = res.data.downloadURL
              this.post.imgURL = res.data.downloadURL
              return res.data.downloadURL
          }).then(URL => {
              axios.post('https://ratchaponproject.firebaseio.com/data.json',this.post)
            .then(response => {
                console.log(response,URL)
                this.getDataArray()
                })
          })
      },
      getDataArray() {
        axios.get('https://ratchaponproject.firebaseio.com/data.json')
        .then(response => {
            return response.data
        }).then(data => {
            const resultArray = []
            for(var key in data){
                resultArray.push(data[key])
            }
            this.postArray = resultArray
            this.postArray = _.reverse(this.postArray)
        })
      },
      testPromise() {
        //   axios.get('https://jsonplaceholder.typicode.com/posts')
        //   .then(response => {
        //       console.log('first')
        //       return true
        //     })
        //   .then(response => {
        //       console.log('second',response)
        //       axios.get('https://jsonplaceholder.typicode.com/users')
        //       .then(response => console.log('third',response))
        //     })
        axios.get('https://ratchaponproject.firebaseio.com/data.json')
            .then(response => console.log(response.data))
      }
  },
  created() {
        this.getDataArray()
    }
}
</script>

<style scoped>
    .inbox {
        height: auto;
        width: "100%";
    }
    #columns {
        column-width: 15vw;
        -moz-column-width: 15vw;
        column-gap: 15px;
        width: 100%;
        /* max-width: 1100px; */
        margin: 10px auto;
    }
    .post{
        cursor: pointer;
        margin-bottom: 10px;
        -webkit-column-break-inside: avoid; /* Chrome, Safari */
        page-break-inside: avoid;           /* Theoretically FF 20+ */
        break-inside: avoid-column;         /* IE 11 */
        display:table;                      /* Actually FF 20+ */
    }
    .card{
        border-color: thistle;
        filter: drop-shadow(0px 0px 3px thistle);
        border-radius: 7px;
    }
    img{
        border-top-left-radius: 7px;
        border-top-right-radius: 7px;
    }
    
</style>


