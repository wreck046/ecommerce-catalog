<template>
  <div :class="sectionClass">
    <div v-if="loading" class="loader"></div>

    <div v-else>
      <div v-if="isAvailable">
        <img :src="product.image" width="200" />
        <h2>{{ product.title }}</h2>
        <p>{{ product.description }}</p>
        <h3>$ {{ product.price }}</h3>
      </div>

      <div v-else>
        <h2>Product Unavailable</h2>
      </div>

      <button @click="nextProduct">Next Product</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      index: 1,
      product: {},
      loading: false,
    };
  },
  computed: {
    isAvailable() {
      return (
        this.product.category === "men's clothing" ||
        this.product.category === "women's clothing"
      );
    },
    sectionClass() {
      if (this.product.category === "men's clothing") {
        return "men-section";
      } else if (this.product.category === "women's clothing") {
        return "women-section";
      } else {
        return "unavailable-section";
      }
    },
  },
  methods: {
    fetchProduct() {
      this.loading = true;

      axios
        .get(`https://fakestoreapi.com/products/${this.index}`)
        .then((res) => {
          this.product = res.data;
        })
        .finally(() => {
          this.loading = false;
        });
    },
    nextProduct() {
      this.index++;
      if (this.index > 20) {
        this.index = 1;
      }
      this.fetchProduct();
    },
  },
  mounted() {
    this.fetchProduct();
  },
};
</script>
