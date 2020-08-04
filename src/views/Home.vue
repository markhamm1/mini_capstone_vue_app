<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <button v-on:click="addProduct">Add Product</button>
    <h1>{{ name }}</h1>

    <div v-for="product in products">
      <p>name: {{ product.name }}</p>
      <p>price: {{ product.price }}</p>
      <!-- <p>image_url: {{ product.image_url }}</p> -->
      <img v-bind:src="product.image_url">
      <hr>
    </div>
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

      var params = {
        name: "Applesauce",
        description: "saucy apples",
        price: 2,
        image_url:
          "https://st.depositphotos.com/1027198/4199/i/450/depositphotos_41993527-stock-photo-bowl-of-apple-sauce.jpg",
      };

      axios.post("api/products", params).then((response) => {
        console.log(response.data);
        this.products.push(response.data);
      });
    },
  },
};
</script>