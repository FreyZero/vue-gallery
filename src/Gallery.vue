<template>
  <div class="container">

      <div class="container" style="margin-top:15px">
          <input @change="onFileSelect" type="file">
          <button @click="onUpload" class="button is-primary">Upload</button>
          <button @click="testPromise" class="button is-danger">Test</button>
      </div>
      <div class="container">
            <p>Lorem Ipsum is simply dummy text of the printing and typesetting industry. 
              Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, 
              when an unknown printer took a galley of type and scrambled it to make a type 
              specimen book. It has survived not only five centuries, but also the leap into 
              electronic typesetting, remaining essentially unchanged. It was popularised in 
              the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, 
              and more recently with desktop publishing software like Aldus PageMaker 
              including versions of Lorem Ipsum.
            </p>

            <p class='box'>
                Response: {{ resData }}
            </p>
            <div id="columns">
                <div v-for="(obj,index) in postArray" :key="index" class="post">
                    <div class="box" style="margin-top:10px;margin-bottom:10px">
                        <article class="media">
                            <div class="media-left">
                            <figure class="image inbox">
                                <img :src="obj.imgURL" alt="Image" >
                            </figure>
                            </div>
                            <div class="media-content">
                            <div class="content">
                                <p>
                                <strong>John Smith</strong> <small>@johnsmith</small> <small>index: {{index}}</small>
                                <br>
                                Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean efficitur sit amet massa fringilla egestas. Nullam condimentum luctus turpis.
                                </p>
                            </div>
                            </div>
                        </article>
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
        width: 128px;
    }
    #columns {
        column-width: 26vw;
        -moz-column-width: 26vw;
        column-gap: 15px;
        width: 100%;
        /* max-width: 1100px; */
        margin: 10px auto;
    }
    .post{
        cursor: pointer;
    }
</style>


