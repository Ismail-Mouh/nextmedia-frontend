<template>
  <div>
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
    <button type="submit" class="btn btn-primary" @click="storeProduct">Submit</button>
  </div>
</template>

<script>
import axios from '@/utils/axios'

export default {
  props: ['showForm', 'getProducts'],
  data() {
    return {
      product: {
        name: '',
        description: '',
        price: '',
      },
    }
  },

  methods : {
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
    }
  },

}
</script>