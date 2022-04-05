<script>
import axios from 'axios'
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProductParams: {},
      currentProduct: {}
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get('http://localhost:3000/products')
        .then(
          response => {
            console.log(response.data);
            this.products = response.data;
          }
        );
    },
    createProduct: function () {
      axios.post('http://localhost:3000/products', this.newProductParams)
        .then(
          response => {
            console.log(response);
            this.newProductParams = {};
            this.products.push(response.data)
          },
        );
    },
    showProduct: function (product) {
      this.currentProduct = product;
      console.log(this.currentProduct);
      document.querySelector("#product-details").showModal();
    }
  }
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>Name:
    <input v-model="newProductParams.name" />
    <br />Description:
    <input v-model="newProductParams.description" />
    <br />Price:
    <input v-model="newProductParams.price" />
    <br />Image URL:
    <input v-model="newProductParams.image_url" />
    <br />
    <button v-on:click="createProduct()">Create Product</button>
    <div v-for="product in products">
      {{ product.name }}, {{ product.formatted.price }}
      <button
        v-on:click="showProduct(product)"
      >Show Info</button>
      <br />
      <img v-bind:src="product.image_url" />
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h2>Product Info</h2>
        <p>Name: {{ currentProduct.name }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <p>Price: ${{ currentProduct.price }}</p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
img {
  width: 60%;
}
</style>