<template>
  <div>
    <h1>Products list</h1>
    <button class="btn btn-success" @click="showForm(true)">Create product</button>
    <Form v-show="formVisibility" :showForm="showForm" :getProducts="getProducts"/>
    <table class="table">
      <thead>
      <tr>
        <th scope="col">Image</th>
        <th scope="col" @click="sortBy('name')" class="clickable">Name {{ sortingSymbol.name }}</th>
        <th scope="col">Description</th>
        <th scope="col" @click="sortBy('price')" class="clickable">Price {{ sortingSymbol.price }}</th>
        <th scope="col">
          <select class="form-select" v-model="selectedCategory">
            <option v-for="category in categories" :value="category.id" :key="category.id">{{category.name}}</option>
          </select>
        </th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="product in productsTemp" :key="product.id">
        <td><img :src="baseURL + '/' + product.image" alt="" v-if="product.image" class="product-image"/></td>
        <td>{{ product.name }}</td>
        <td>{{ product.description }}</td>
        <td>{{ product.price }}</td>
        <td><span v-for="category in product.categories" :key="category.id">{{category.name}}<br></span></td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from '@/utils/axios'
import _ from 'lodash'
import Form from "@/components/products/includes/Form";
import './products.css'

export default {
  components: {
    Form
  },
  data() {
    return {
      products: [],
      sortingSymbol: {
        name: '',
        price: '',
      },
      formVisibility: false,
      categories: [{
        id: 0,
        name: 'Categories'
      }],
      selectedCategory: 0,
      productsTemp: [],
      baseURL: process.env.VUE_APP_BASE_API
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
        this.productsTemp = response.data
        this.populateCategories()
      })
    },
    sortBy(field) {
      this.productsTemp = _.sortBy(this.productsTemp, field)
      this.sortingSymbol[field] == '↓' ? _.reverse(this.productsTemp) : null
      this.handleSymbols(field)
    },
    handleSymbols(field) {
      this.sortingSymbol[field] = !this.sortingSymbol[field] ? '↓' : this.sortingSymbol[field] == '↓' ? '↑' : '↓'
    },
    showForm(value) {
      this.formVisibility = value
    },
    populateCategories() {
      this.products.forEach((product) => {
        product.categories.forEach((category) => {
          this.categories.push(category)
        })
      })

      this.categories = _.uniqBy(this.categories,'id')
    }
  },

  watch : {
    selectedCategory() {
      if (this.selectedCategory) {
        this.productsTemp = this.products.filter((product) => {
          let found = false;
          product.categories.forEach((category) => {
            if (category.id == this.selectedCategory) {
              found = true
            }
          })
          return found
        })
      } else {
        this.productsTemp = this.products
      }
    }
  },


}
</script>