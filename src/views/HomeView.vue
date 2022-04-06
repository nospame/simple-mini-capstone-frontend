<script>
import axios from 'axios'
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProductParams: {},
      currentProduct: {},
      editProductParams: {}
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
            console.log(response.data);
            this.newProductParams = {};
            this.products.push(response.data)
          },
        );
    },
    showProduct: function (product) {
      this.currentProduct = product;
      this.editProductParams = product;
      console.log(this.currentProduct);
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function () {
      axios.patch(`http://localhost:3000/products/${this.editProductParams.id}`, this.editProductParams)
        .then(
          response => {
            console.log(response.data);
          }
        )
    },
    destroyProduct: function (product) {
      axios.delete(`http://localhost:3000/products/${this.editProductParams.id}`)
        .then(
          response => {
            console.log(response.data);
            this.products.splice(
              this.products.indexOf(product), 1
            )
          }
        )
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
    <div v-for="product in products" v-bind:key="product.id">
      <p>
        <img v-bind:src="product.image_url" />
        <br />
        {{ product.name }}
        <button v-on:click="showProduct(product)">Show Info</button>
      </p>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h2>Product Info</h2>
        <p>Name: {{ currentProduct.name }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <p>Price: ${{ currentProduct.price }}</p>
        <p>
          Name:
          <input v-model="editProductParams.name" />
          <br />Description:
          <input v-model="editProductParams.description" />
          <br />Price:
          <input v-model="editProductParams.price" />
          <br />Image URL:
          <input v-model="editProductParams.image_url" />
          <br />
        </p>
        <button v-on:click="updateProduct()">Save Changes</button>
        <button v-on:click="destroyProduct(currentProduct)">Delete Product</button>
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