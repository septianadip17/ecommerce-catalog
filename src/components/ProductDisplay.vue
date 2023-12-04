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
import '../assets/style/MensClothing.css';
import '../assets/style/WomensClothing.css';

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

<style>
.mens-clothing {
  --main-color: #002772;
  --secondary-color: #d6e6ff;
}

.womens-clothing {
  --main-color: #720060;
  --secondary-color: #fde2ff;
}

.default-category {
  --main-color: #000;
  --secondary-color: #d8d7d7;
}

.empty-state {
  background-image: url("../assets/images/sad-face.png");
  background-size: cover;
  background-position: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  height: 60vh;
  width: 100%;
}

.product-menu,
.title,
.card h3,
.price,
.buy,
.next,
.empty-next {
  color: var(--main-color);
}

.buy,
.dot.active {
  background-color: var(--main-color);
}

.next,.empty-next,.dot {
  border: 2px solid var(--main-color);
}

.spinner {
  border: 4px solid rgba(0, 0, 0, 0.1);
  width: 200px;
  height: 200px;
  border-radius: 50%;
  border-left-color: #09f;
  animation: spin 1s ease infinite;
  margin: auto;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.product-image {
  width: 30vh;
  max-height: 40vh;
  height: auto;
  object-fit: contain;
  display: block;
  margin: auto 0;
}

.outer-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.top-background {
  background-color: var(--secondary-color);
  width: 100%;
  height: 70vh;
}

.bottom-background {
  background-color: white;
  width: 100%;
  height: 30vh;
}

.container {
  padding: 0 40px;
  background-color: white;
  position: absolute;
  width: 80%;
  border: 1px;
  border-radius: 12px;
  display: flex;
  grid-template-columns: auto auto auto;
  gap: 20px;
  height: 70vh;
  align-items: center;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
.image {
  grid-column: span 1;
}
.product-menu {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: start;
  height: 40vh;
  text-align: start;
  background: white;
}
.title {
  font-family: Inter;
  font-size: 28px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
}
.empty-title{
  color: #000;
  font-family: Inter;
  font-size: 20px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
}
.empty-button{
  border-radius: 4px;
  border: 3px solid #000;
  background: #FFF;
}
.categories {
  color: #3f3f3f;
  font-family: Inter;
  font-size: 18px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
}
.rate {
  color: #3f3f3f;
  font-family: Inter;
  font-size: 18px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
}
hr {
  width: 100%;
  color: #dcdcdc;
}
.rating {
  display: flex;
  align-items: center;
}
.dot-rating {
  display: flex;
  gap: 4px;
}
.description {
  color: #1e1e1e;
  font-family: Inter;
  font-size: 20px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
}
.dot {
  height: 25px;
  width: 25px;
  background-color: white;
  border-radius: 50%;
  display: inline-block;
}
.top-card {
  width: 100%;
  display: flex;
  justify-content: space-between;
}
.price {
  font-family: Inter;
  font-size: 28px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
}
.buy {
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  font-weight: 700;
  cursor: pointer;
  width: 100%;
  border-radius: 8px;
}

.product-button {
  gap: 20px;
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.next {
  background-color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  font-weight: 700;
  cursor: pointer;
  width: 100%;
  border-radius: 8px;
}

.empty-next {
  background-color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  width: 60%;
  font-size: 16px;
  font-weight: 700;
  cursor: pointer;
  border-radius: 8px;
}
</style>
