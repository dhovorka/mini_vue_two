<template>
  <div class="home">
    <a href="/signup">Signup</a> or
    <router-link to="/signup">Signup</router-link>
    <a href="/login">Login</a> or
    <router-link to="/login">Login</router-link>
    <a href="/logout">Logout</a> or
    <router-link to="/logout">Logout</router-link>
    <h1>New Product</h1>
    <div>
      Name: <input type="text" v-model="newProductName" /> Price:
      <input type="text" v-model="newProductPrice" /> Description: <input type="text" v-model="newProductDescription" />
    </div>
    <button v-on:click="createProduct()">Create Product</button>

    <h1>All Products</h1>
    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.url" />
      <router-link v-bind:to="`/products/${product.id}`">more info</router-link>
      <button v-on:click="showProduct(product)">Show More Info</button>
      <div v-if="currentProduct === product">
        <p>Name: {{ product.name }}</p>
        <p>Price: {{ product.price }}</p>
        <p>Description: {{ product.description }}</p>
        <div>
          Name: <input type="text" v-model="product.name" /> Price:
          <input type="text" v-model="product.price" /> Description: <input type="text" v-model="product.description" />
          <button v-on:click="updateProduct(product)">Update Product</button>
          <button v-on:click="destroyProduct(product)">Destroy Product</button>
        </div>
        <!--  <img alt="Vue logo" src="../assets/logo.png" /> -->
        <!--  <HelloWorld msg="Welcome to Your Vue.js App" /> -->
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";

export default {
  data: function() {
    return {
      products: [],
      currentProduct: {},
      newProductName: "",
      newProductPrice: "",
      newProductDescription: ""
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },
  methods: {
    createProduct: function() {
      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription
      };
      axios.post("/api/products", params).then(response => {
        this.products.push(response.data);
        this.newProductName = "";
        this.newProductPrice = "";
        this.newProductDescription = "";
      });
    },
    showProduct: function(product) {
      if (this.currentProduct === product) {
        this.currentProduct = {};
      } else {
        this.currentProduct = product;
      }
    },
    updateProduct: function(product) {
      var params = {
        name: product.name,
        price: product.price,
        description: product.description
      };
      axios.patch("/api/products/" + product.id, params).then(response => {
        this.currentProduct = {};
      });
    },
    destroyProduct: function(product) {
      axios.delete("/api/products/" + product.id).then(response => {
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
      });
    }
  }
};
</script>
