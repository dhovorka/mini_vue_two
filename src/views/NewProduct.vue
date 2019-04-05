<template>
  <div class="products-new">
    <h1>Create New Product</h1>
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>
    <div>
      Name: <input type="text" v-model="newProductName" /> Price:
      <input type="text" v-model="newProductPrice" /> Description: <input type="text" v-model="newProductDescription" />
    </div>
    <button v-on:click="createProduct()">Create Product</button>
  </div>
</template>

<script>
var axios = require("axios");

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
  methods: {
    createProduct: function() {
      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription
      };
      axios
        .post("/api/products", params)
        .then(response => {
          this.$router.push("/");
        })
        .catch(error => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
