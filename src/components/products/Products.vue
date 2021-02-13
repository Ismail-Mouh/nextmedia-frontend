<template>
  <div>
    <h1>Products list</h1>
    <table class="table">
      <thead>
      <tr>
        <th scope="col" @click="sortBy('name')">Name {{sortingSymbol.name}}</th>
        <th scope="col">Description</th>
        <th scope="col" @click="sortBy('price')">Price {{sortingSymbol.price}}</th>
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
import _ from 'lodash'

export default {
  data() {
    return {
      products: [],
      sortingSymbol : {
        name: '',
        price: '',
      },
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
    sortBy(field) {
      this.products = _.sortBy(this.products, field)
      this.sortingSymbol[field] == '↓' ? _.reverse(this.products) : null
      this.handleSymbols(field)
    },
    handleSymbols(field) {
      this.sortingSymbol[field] = !this.sortingSymbol[field] ? '↓' : this.sortingSymbol[field] == '↓' ? '↑' : '↓'
    },
  },


}
</script>