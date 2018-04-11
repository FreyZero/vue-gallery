<template>
  <div>
    <app-header :isShow="isShow" @toggleCard="isShow=$event"></app-header>
    <app-gallery :isShow="isShow"></app-gallery>
    <app-footer></app-footer>
  </div>
</template>

<script>
  import Header from './Header.vue'
  import Gallery from './Gallery'
  import Footer from './Footer'

  export default {
    data() {
      return {
        isShow: false
      }
    },
    components : {
      appHeader : Header,
      appGallery : Gallery,
      appFooter : Footer
    }
  }
</script>

<style scoped>

</style>
