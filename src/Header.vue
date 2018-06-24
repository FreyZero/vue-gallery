<template>
  <div>
    <nav class="navbar is-transparent">
      <div class="container">
        <div class="navbar-brand">
          <a class="navbar-item" href="#">
            <img src="./assets/logo-bnk48.png" alt="BNK48" width="auto" height="35">
          </a>
        </div>
        
        <div id="navbarExampleTransparentExample" class="navbar-menu">
          <div class="navbar-start">
            <a class="navbar-item" href="#">
              Branch Dev
            </a>
            <div class="navbar-item has-dropdown is-hoverable">
              <a class="navbar-link" href="/documentation/overview/start/">
                Docs
              </a>
              <div class="navbar-dropdown is-boxed">
                <a class="navbar-item" href="/documentation/overview/start/">
                  Overview
                </a>
                <a class="navbar-item" href="https://bulma.io/documentation/modifiers/syntax/">
                  Modifiers
                </a>
                <a class="navbar-item" href="https://bulma.io/documentation/columns/basics/">
                  Columns
                </a>
                <a class="navbar-item" href="https://bulma.io/documentation/layout/container/">
                  Layout
                </a>
                <a class="navbar-item" href="https://bulma.io/documentation/form/general/">
                  Form
                </a>
                <hr class="navbar-divider">
                <a class="navbar-item" href="https://bulma.io/documentation/elements/box/">
                  Elements
                </a>
                <a class="navbar-item is-active" href="https://bulma.io/documentation/components/breadcrumb/">
                  Components
                </a>
              </div>
            </div>
          </div>
        
          <div class="navbar-end">
            <div class="navbar-item">
              <div class="field is-grouped">
                <p class="control">
                  <a class="bd-tw-button button" 
                    target="_blank" href="#">
                    <span class="icon">
                      <i class="fab fa-twitter"></i>
                    </span>
                    <span>
                      Tweet
                    </span>
                  </a>
                </p>
                <p class="control">
                  <a @click="addImage()" class="button is-primary" href="#">
                    <span class="icon">
                      <i class="fas fa-plus-circle"></i>
                    </span>
                    <span>Add Image</span>
                  </a>
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShow: false
    }
  },
  methods: {
    addImage() {
      this.isShow = !this.isShow
      this.$emit('toggleCard',this.isShow)
      console.log(this.isShow)
    }
  },
}
</script>

<style scoped>
  nav{
    background-color: thistle;
  }
  .container {
    border-radius: 0;
  }
</style>

