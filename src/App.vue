<template>
  <div id="app">
    <Products/>
  </div>
</template>

<script>
import Products from './components/products/Products.vue'

export default {
  name: 'App',
  components: {
    Products
  }
}
</script>

<style>
#app {
  padding: 50px;
}
</style>
