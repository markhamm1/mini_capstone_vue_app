<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Name: <input v-model="newProductName" type="text"></p>
    <p>Description: <input v-model="newProductDescription" type="text"></p>
    <p>Price: <input v-model="newProductPrice" type="text"></p>
    <p>Image Url: <input v-model="newProductImageUrl" type="text"></p>
    <button v-on:click="addProduct">Add Product</button>
    <!-- <h1>{{ name }}</h1> -->

    <div v-for="product in products">
      <p>name: {{ product.name }}</p>
      <p>price: {{ product.price }}</p>
      <!-- <p>image_url: {{ product.image_url }}</p> -->
      <img v-bind:src="product.image_url" style="width:300px;">
      <br>
      <button v-on:click="showInfo(product)">Show Info</button>
      <hr>
    </div>

    <dialog id="product-details">
      <form method="dialog">
        <h1>{{ currentProduct.name }}: info</h1>
        <p>{{ currentProduct.description }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <img v-bind:src="currentProduct.image_url" style="width:600px;">
        <button>Close</button>
      </form>
    </dialog>

  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      name: "Mark",
      products: [],
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: "",
      currentProduct: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      console.log("This is the Products Index");
      axios.get("api/products").then((response) => {
        console.log(response);
        this.products = response.data;
      });
    },
    addProduct: function () {
      console.log("Adding products now...");

      // console.log(this.newProductName);

      var params = {
        name: this.newProductName,
        description: this.newProductDescription,
        price: this.newProductPrice,
        image_url: this.newProductImageUrl,
      };

      axios
        .post("api/products", params)
        .then((response) => {
          console.log(response.data);
          this.products.push(response.data);
        })
        .catch((errors) => {
          console.log(errors.response);
          this.errors = errors.response;
        });
    },

    showInfo: function (product) {
      console.log("more info...");
      console.log(product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
  },
};
</script>