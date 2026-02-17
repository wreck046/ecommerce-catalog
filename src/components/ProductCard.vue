<template>
  <div :class="sectionClass" class="container">
    <div class="card">
      <!-- Loader -->
      <div v-if="loading" class="content skeleton-wrapper">
        <div class="image"></div>
        <div class="details">
          <div class="title"></div>
          <div class="text"></div>
          <div class="text short"></div>
          <div class="price"></div>
          <div class="button"></div>
        </div>
      </div>

      <!-- Content -->
      <div v-else class="content">
        <template v-if="isAvailable">
          <div class="left">
            <img :src="product.image" class="product-image" />
          </div>

          <div class="right">
            <h2 class="title">{{ product.title }}</h2>

            <div class="category-rating">
              <span class="category">
                {{ product.category }}
              </span>
              <span class="rating">
                {{ product.rating?.rate }}/5
                <span class="dots">
                  <span
                    v-for="n in 5"
                    :key="n"
                    :class="[
                      'dot',
                      n <= Math.round(product.rating?.rate) ? 'active' : '',
                    ]"
                  ></span>
                </span>
              </span>
            </div>
            <hr />
            <p class="description">{{ product.description }}</p>
            <hr />
            <div class="bottom">
              <h3 class="price">$ {{ product.price }}</h3>

              <div class="buttons">
                <button class="buy">Buy now</button>
                <button class="next" @click="nextProduct">Next product</button>
              </div>
            </div>
          </div>
        </template>

        <template v-else>
          <div class="unavailable">
            <h2>This product is unavailable to show</h2>
            <button class="next" @click="nextProduct">Next product</button>
          </div>
        </template>
      </div>
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
        .then((response) => {
          const data = response.data;

          // hanya simpan jika men / women
          if (
            data.category === "men's clothing" ||
            data.category === "women's clothing"
          ) {
            this.product = data;
          } else {
            this.product = data; // tetap simpan supaya category bisa dibaca
          }
        })
        .catch((error) => {
          console.log(error);
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
