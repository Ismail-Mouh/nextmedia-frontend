<template>
  <div>
    <div class="mb-3">
      <label class="form-label">Product Image</label>
      <br>
      <UploadImage @updateImage="updateImage"/>
    </div>
    <div class="mb-3">
      <label class="form-label">Product Name</label>
      <input type="text" v-model="product.name" class="form-control">
    </div>
    <div class="mb-3">
      <label class="form-label">Product Description</label>
      <input type="text" v-model="product.description" class="form-control" >
    </div>
    <div class="mb-3">
      <label class="form-label">Product Price</label>
      <input type="number" v-model="product.price" step="0.1" class="form-control" >
    </div>
    <div class="mb-3">
      <label class="form-label">Categories</label>
      <select class="form-select" v-model="product.categories" multiple>
        <option v-for="category in categories" :value="category.id" :key="category.id">{{category.name}}</option>
      </select>
    </div>
    <button type="submit" class="btn btn-primary" @click="storeProduct">Submit</button>
  </div>
</template>

<script>
import axios from '@/utils/axios'
import UploadImage from "@/utils/images/UploadImage";

export default {
  props: ['showForm', 'getProducts'],

  components: {
    UploadImage
  },

  data() {
    return {
      product: {
        name: '',
        description: '',
        price: '',
        categories: [],
        image: ''
      },
      categories: [],

    }
  },

  mounted() {
    this.getCategories()
  },

  methods : {
    getCategories() {
      axios.get('/categories').then((response) => {
        this.categories = response.data
      })
    },
    storeProduct() {
      if (this.validateForm()) {
        axios.post('/products',this.product).then(() => {
          alert("Product Created")
          this.getProducts()
          this.showForm(false)
        })
      }
    },
    validateForm() {
      if (!this.product.name || !this.product.price) {
        alert("Name and price are required")
        return false
      }
      return true
    },
    updateImage(value) {
      this.product.image = value
    },
  }

}
</script>