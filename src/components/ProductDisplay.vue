<template>
  <div :class="currentCategoryClass">
    <div class="outer-container">
      <div class="top-background"></div>
      <div class="bottom-background"></div>
      <div class="container" style="">
        <div v-if="isLoading" class="spinner"></div>
        <div v-else-if="currentProduct" style="display: flex; gap: 40px">
          <img
            :src="currentProduct.image"
            @load="imageLoaded"
            class="product-image"
          />
          <div class="product-menu">
            <h3 class="title" v-if="currentProduct">
              {{ currentProduct.title }}
            </h3>

            <div class="top-card" v-if="currentProduct">
              <h4 class="categories">{{ currentProduct.category }}</h4>

              <div class="rating">
                <h4 class="rate" style="margin-right: 10px">
                  {{ currentProduct.rating.rate }}/ 5
                </h4>

                <div class="dot-rating">
                  <span
                    class="dot"
                    v-for="n in ratingArray"
                    :key="n"
                    :class="{ active: n <= currentProduct.rating.rate }"
                  >
                  </span>
                </div>
              </div>
            </div>

            <hr />

            <p class="description" v-if="currentProduct">
              {{ currentProduct.description }}
            </p>

            <hr />

            <h2 class="price">${{ currentProduct.price }}</h2>
            <div class="product-button">
              <button class="buy">Buy now</button>
              <button class="next" @click="nextProduct">Next product</button>
            </div>
          </div>
        </div>
        <div v-else class="empty-state">
          <h3 class="empty-title">This product is unavailable to show</h3>
          <button class="empty-next" @click="nextProduct">Next product</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import '../assets/style/Page.css';

export default {
  data() {
    return {
      currentProduct: null, 
      currentIndex: 1, 
      isLoading: true, 
      currentCategoryClass: "",
    };
  },
  mounted() {
    this.fetchProducts(); 
  },
  methods: {
    fetchProducts() {
      this.isLoading = true;
      const url = `https://fakestoreapi.com/products/${this.currentIndex}`;
      axios
        .get(url)
        .then((response) => {
          if (response.data.category === "men's clothing") {
            this.currentProduct = response.data;
            this.currentCategoryClass = "mens-clothing";
            this.isLoading = false;
          } else if (response.data.category === "women's clothing") {
            this.currentProduct = response.data;
            this.currentCategoryClass = "womens-clothing";
            this.isLoading = false;
          } else {
            this.currentProduct = null;
            this.currentCategoryClass = "default-category";
            this.isLoading = false;
          }
        })
        .catch((error) => {
          console.error(error); 
          console.error("error"); 
        });
    },
    nextProduct() {
      if (this.currentIndex === 20) {
        this.currentIndex = 1;
      } else this.currentIndex++;
      this.fetchProducts();
    },
    imageLoaded() {
      this.isLoading = false;
    },
  },
  computed: {
    ratingArray() {
      return this.currentProduct
        ? Array.from({ length: 5 }, (_, i) => i + 1)
        : [];
    },
  },
  name: "ProductDisplay",
};
</script>

<style></style>
