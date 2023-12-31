<template>
  <div>
    <div class="bg" :class="theme.bg"></div>
    <section class="wrapper">
      <div class="product-box">
        <!-- Loader -->
        <div v-if="fetchStatus" class="loader-wrapper">
          <div class="loader"></div>
        </div>
        <div v-if="!fetchStatus && !isAvailable">
          <div class="img-container">
            <img class="unv-img" src="../assets/unavailable.svg" alt="Unavailable :(" />
          </div>
          <div class="container">
            <p class="unv-text">This product is unavailable to show</p>
            <button class="unv-button" @click="fetchNextProduct">Next Product</button>
          </div>
        </div>
        <div v-if="!fetchStatus && isAvailable" class="product-wrapper">
          <div class="img-wrapper">
            <img :src="product.image" :alt="product.title" />
          </div>
          <div class="wrapper-col">
            <div class="detail-wrapper">
              <h1 :class="theme.text">{{ product.title }}</h1>

              <div>
                <div class="category">
                  <span>{{ product.category }}</span>
                  <div class="rating">
                    <span>{{ product?.rating?.rate }}/5</span>
                    <div class="rating-circle">
                      <div v-for="index in 5" :key="index" class="circle" :class="[
                        theme.rating,
                        {
                          [theme.filled]: index <= product?.rating?.rate,
                        },
                      ]"></div>
                    </div>
                  </div>
                </div>
                <hr />
              </div>

              <p class="description">{{ product.description }}</p>
            </div>
            <div class="detail-wrapper">
              <div>
                <hr />
                <span :class="theme.text" class="price">${{ product.price }}</span>
              </div>
              <div class="btns-wrapper">
                <button :class="theme.buttonBuy">Buy Now</button>
                <button :class="theme.buttonNext" @click="fetchNextProduct">
                  Next Product
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'ProductDisplay',
  data() {
    return {
      product: {},
      currentId: 1,
      fetchStatus: false,
      theme: {},
      isAvailable: true,
      availableCategories: ["men's clothing", "women's clothing"],
    };
  },
  created() {
    this.fetchStatus = true;
    this.fetchProducts();
  },
  methods: {
    async fetchProducts() {
      const res = await fetch(
        `https://fakestoreapi.com/products/${this.currentId}`
      );
      const productData = await res.json();
      this.product = productData;
      this.changeTheme();

      const { category } = this.product;

      if (
        category === this.availableCategories[0] ||
        category === this.availableCategories[1]
      ) {
        this.isAvailable = true;
      } else {
        this.isAvailable = false;
      }

      this.fetchStatus = false;
    },
    fetchNextProduct() {
      this.fetchStatus = true;
      this.currentId === 20 ? (this.currentId = 1) : ++this.currentId;
      this.fetchProducts();
    },
    changeTheme() {
      switch (this.product.category) {
        case "men's clothing":
          this.theme = {
            bg: 'bg-men',
            text: 'text-men',
            rating: 'circle-men',
            filled: 'filled-men',
            buttonBuy: 'btn-buy-men',
            buttonNext: 'btn-next-men',
          };
          break;
        case "women's clothing":
          this.theme = {
            bg: 'bg-women',
            text: 'text-women',
            rating: 'circle-women',
            filled: 'filled-women',
            buttonBuy: 'btn-buy-women',
            buttonNext: 'btn-next-women',
          };
          break;
        default:
          this.theme = {
            bg: 'bg-default',
          };
          break;
      }
    },
  },
};
</script>

<style>
.loader-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100px;
}

.loader {
  border: 10px solid rgba(0, 0, 0, 0.3);
  border-top: 10px solid #1E1E1E;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
</style>
