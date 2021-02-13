<template>
  <div>
    <h1>Products list</h1>
    <table class="table">
      <thead>
      <tr>
        <th scope="col">Name</th>
        <th scope="col">Description</th>
        <th scope="col">Price</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="product in products" :key="product.id">
        <td>{{ product.name }}</td>
        <td>{{ product.description }}</td>
        <td>{{ product.price }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from '@/utils/axios'

export default {
  data() {
    return {
      products: [],
    }
  },
  computed: {},

  created() {
    this.getProducts()
  },

  methods: {
    getProducts() {
      axios.get('/products').then((response) => {
        this.products = response.data
      })
    },
  },


}
</script>