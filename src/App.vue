<template>
  <div>
    <app-header></app-header>
    <app-gallery id="ss"></app-gallery>
    <app-footer></app-footer>
  </div>
</template>

<script>
  import Header from './Header.vue'
  import Gallery from './Gallery'
  import Footer from './Footer'

  export default {
    components : {
      appHeader : Header,
      appGallery : Gallery,
      appFooter : Footer
    }
  }
</script>

<style scoped>

</style>
