<template>
  <div id="app">
    <Index />
  </div>
</template>

<script>
import Index from './components/Index.vue'
export default {
  name: 'App',
  components: {
    Index
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: var(--main-white);
  width: 100%;
}

:root {
  --main-primary-dark: #02111B;
  --main-primary-light: #30292F;
  --main-secondary-dark: #3F4045;
  --main-secondary-light: #5D737E;
  --main-accent-success: #509e49;
  --main-accent-failure: #c43d3d;
  --main-white: #FCFCFC;
  --border-radius: 5px;
  background-color: var(--main-secondary-dark)
  
}
button {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .3s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
