<template>
  <div class="products-show">
    <h2>Title: {{ product.name }}</h2>
    <p>Name: {{ product.name }}</p>
    <p>Price: {{ product.price }}</p>
    <p>Description: {{ product.description }}</p>
    <div>
      <h4>Edit this</h4>
      Name:
      <input type="text" v-model="product.name" />
      Price:
      <input type="text" v-model="product.price" />
      <input type="text" v-model="product.description" />
      <button v-on:click="destroyProduct(product)">delete for good</button>
      <button v-on:click="updateProduct(product)">Update product</button>
    </div>
    <router-link to="/">Back to all products</router-link>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      product: {}
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      this.product = response.data;
    });
  },
  methods: {
    updateProduct: function(product) {
      var params = {
        name: product.name,
        price: product.price,
        description: product.description
      };
      axios.patch("/api/products/" + product.id, params).then(response => {
        console.log("success", response.data);
        product = response.data;
      });
    },
    destroyProduct: function(product) {
      axios.delete("/api/products/" + product.id).then(response => {
        console.log("success deleted", response.data);
        this.$router.push("/");
      });
    }
  }
};
</script>
