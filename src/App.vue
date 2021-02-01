<template>
  <div id="app">
    <Navbar  id="nav"/>
    <carcart  id="Production"/>
    <Footer />
  </div>
</template>
<script>
import Navbar from './components/Navbar.vue'
import carcart from './components/carcart.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'app',
  data: function () {
    return {
    }
  },
  components: {
    Navbar,
    carcart,
    Footer
  },
}
</script>

<style>
@import './css/container.css';
@import './css/css-variable.css';
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--text-color);
  background-color: var(--product-bg-color);
}
a:link{
  color:var(--text-color);
}
/* 已連結過 */
a:visited{
  color: var(--text-color);
}
</style>
