<template>
  <div class="container">
    <!-- <div class="upload-field">
      <div class="input-area">
        <div @click="triggerDropdown()" :class="activeClass" class="dropdown">
          <div class="dropdown-trigger">
            <button class="button" aria-haspopup="true" aria-controls="dropdown-menu">
              <span>{{selectedMember}}</span>
            </button>
          </div>
          <div class="dropdown-menu" id="dropdown-menu" role="menu">
            <div class="dropdown-content">
              <a v-for="(i, index) in dropdownArray" :key=index @click="selectedMember=i" :class="activeItem" class="dropdown-item">
                {{i}}
              </a>
            </div>
          </div>
        </div>
        <div class="custom-textarea">
          <input type="text" class="input">
        </div>
        <div class="custom-textarea">
          <textarea placeholder="Picture content..." rows=2 class="textarea"></textarea>
        </div>
      </div>
      <div @click="$refs.fileInput.click()" style="margin-top:1rem; float:left; cursor:pointer;">
        <a class="fas fa-images icon is-medium"></a>
      </div>
      <button @click="onUpload" class="button is-primary" style="margin-top:1rem; float:right">Post</button>
    </div> -->

    <section class="section">
      <div class="container">
        <input id="inputImg" @change="onFileSelect" type="file" style="display:none" ref="fileInput">
      </div>
    </section>
    <div class="container">
      <div id="columns">
        <transition name="bounce">
          <div v-if="isShow" class="post shadow">
            <div class="card ">
              <div class="card-image">
                <figure class="image inbox">
                  <img id="uploadImg" src="./assets/Add-Icon-5.png" @click="$refs.fileInput.click()" alt="Placeholder image" style="cursor:pointer">
                </figure>
              </div>
              <div class="card-content">
                <div class="media">
                  <div class="">
                    <input v-model="post.inputTopic" class="title is-4 input" placeholder="TOPIC">
                    <div @click="triggerDropdown()" :class="activeClass" class="dropdown" style="width:100%">
                      <div class="dropdown-trigger" style="width:100%">
                        <button class="button" aria-haspopup="true" aria-controls="dropdown-menu" style="width:100%">
                          <span>{{post.selectedMember}}</span>
                        </button>
                      </div>
                      <div class="dropdown-menu" id="dropdown-menu" role="menu">
                        <div class="dropdown-content">
                          <a v-for="(i, index) in dropdownArray" :key=index @click="post.selectedMember=i" :class="activeItem" class="dropdown-item">
                            {{i}}
                          </a>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="content">
                  <textarea v-model="post.inputContent" rows="5" class="textarea"></textarea>
                  <br>
                  <time datetime="2016-1-1">{{ dateandtime }}</time>
                  <br><br>
                  <button @click="onUpload" class="button is-primary" style="width:100%">Post</button>
                </div>
              </div>
            </div>
          </div>
        </transition>
        <div v-for="(post,index) in postArray" :key="index" class="post shadow">
          <div class="card ">
            <div class="card-image">
              <figure class="image inbox">
                <img :src="post.imgURL" alt="Placeholder image">
              </figure>
            </div>
            <div class="card-content">
              <div class="media">
                <div class="">
                  <p class="title is-4">{{post.inputTopic}}</p>
                  <p class="subtitle is-6">{{post.selectedMember}}@{{index}}</p>
                </div>
              </div>
              <div class="content">
                <p>{{post.inputContent}}</p>
                <a href="#">#{{post.inputTopic}}</a>
                <a href="#">#{{post.selectedMember}}</a>
                <br><br>
                <time datetime="2016-1-1">{{post.datetime}}</time>
              </div>
            </div>
          </div>
        </div>
      </div>
      <a href="#" class="float">
        <i class="fas fa-plus-circle my-float"></i>
      </a>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
  import _ from 'lodash'

  export default {
    props: ['isShow'],
    data() {
      return {
        resData: 'Waiting for response data',
        resImgURL: 'https://bulma.io/images/placeholders/128x128.png',
        selectedFile: null,
        post: {
          inputTopic:"BNK48",
          inputContent: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus nec iaculis mauris.",
          selectedMember: "Select Member",
          imgURL: '',
          datetime: null
        },
        postArray: [],
        dropdownArray: ["Can", "Cherprang", "Izurina", "Jaa", "Jane", "Jennis", "Jib", "Kaew", "Kaimook", "Orn"],
        activeClass: "",
        activeItem: "",
        dateandtime: new Date().toLocaleString(),
        filePath: './assets/',
        fileName: 'Add-Icon-5.png',
        imgb: 'Add-Icon-5.png'
      }
    },
    methods: {
      onFileSelect(event) {
        this.selectedFile = event.target.files[0]
        this.readURL(event)
        //console.log(event.target)
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
            this.post.datetime = new Date().toLocaleString()
            return res.data.downloadURL
          }).then(URL => {
            axios.post('https://ratchaponproject.firebaseio.com/data.json', this.post)
              .then(response => {
                console.log(response, URL)
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
            for (var key in data) {
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
      },
      triggerDropdown() {
        if (this.activeClass == "")
          this.activeClass = "is-active"
        else
          this.activeClass = ""
      },
      readURL(input) {
        var reader = new FileReader();
        reader.onload = function(e) {
          $('#uploadImg').attr('src', e.target.result);
        }
        reader.readAsDataURL(input.target.files[0]);
      }
    },
    created() {
      this.getDataArray()
    }
  }
</script>


<style>
  body {
    font-size: 62.5%;
  }

  .inbox {
    height: auto;
    width: 100%;
  }

  .post {
    margin-bottom: 1rem;
    -webkit-column-break-inside: avoid;
    /* Chrome, Safari */
    page-break-inside: avoid;
    /* Theoretically FF 20+ */
    break-inside: avoid-column;
    /* IE 11 */
    display: block;
    /* Actually FF 20+ */
  }

  .content {
    word-wrap: break-word;
  }

  .shadow {
    border-color: thistle;
    filter: drop-shadow(0px 0px .3rem thistle);
  }

  .shadow:hover {
    border-color: #00d1b2;
    filter: drop-shadow(0px 0px .3rem #00d1b2);
  }

  .card {
    border-radius: .7rem;
  }

  .upload-field {
    margin: 2rem;
  }

  .input-area::after {
    content: "";
    clear: both;
    display: table;
  }

  .custom-dropdown {
    display: inline-block;
  }

  .custom-textarea {
    display: inline-block;
    width: auto;
    vertical-align: top;
    font-size: 1rem;
    height: 4.5em;
  }

  .float{
    position:fixed;
    width:60px;
    height:60px;
    bottom:40px;
    right:40px;
    background-color:#0C9;
    color:#FFF;
    border-radius:50px;
    text-align:center;
    box-shadow: 2px 2px 3px #999;
  }

  .my-float{
    margin-top:22px;
  }

  @media only screen and (max-width: 600px) {
    body {
      font-size: 75%;
    }
    #columns {
      column-width: 100vw;
      -moz-column-width: 100vw;
      -webkit-column-width: 100vw;
      column-gap: 0;
      width: 97%;
      /* max-width: 600px; */
      margin: 1rem auto;
    }
  }

  @media only screen and (min-width: 601px) {
    body {
      font-size: 100%;
    }
    #columns {
      column-width: 25vw;
      -moz-column-width: 25vw;
      -webkit-column-width: 25vw;
      column-gap: 1.5rem;
      width: 95%;
      /* max-width: 1100px; */
      margin: 0 auto;
    }
  }

  @media only screen and (min-width: 1089px) {
    body {
      font-size: 100%;
    }
    #columns {
      column-width: 17vw;
      -moz-column-width: 17vw;
      -webkit-column-width: 17vw;
      column-gap: 1.5rem;
      width: 95%;
      /* max-width: 1100px; */
      margin: 0 auto;
    }
  }

  @media only screen and (min-width: 1700px) {
    body {
      font-size: 100%;
    }
    #columns {
      column-width: 15vw;
      -moz-column-width: 15vw;
      -webkit-column-width: 15vw;
      column-gap: 1.5rem;
      width: 95%;
      /* max-width: 1100px; */
      margin: 0 auto;
    }
  }

</style>

<style scoped>
  img {
    border-top-left-radius: 7px;
    border-top-right-radius: 7px;
  }
  .bounce-enter-active {
  animation: bounce-in .5s;
}
.bounce-leave-active {
  animation: bounce-in .5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
    
  }
  100% {
    transform: scale(1);
  }
}
</style>
