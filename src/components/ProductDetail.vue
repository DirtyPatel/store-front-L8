<template>
  <div class="product-detail" v-if="productExists">
    <!-- Left: Image -->
    <div class="image-box">
      <img :src="product.image" :alt="product.name" />
    </div>

    <!-- Right: Info Panel -->
    <div class="info-box">
      <h1 class="title">{{ product.name }}</h1>

      <!-- ⭐ Rating + Reviews -->
      <div v-if="product.rating" class="rating-row">
        <span class="stars">
          <span v-for="n in 5" :key="n" class="star" :class="{ filled: n <= Math.round(product.rating) }">★</span>
        </span>
        <span class="reviews">({{ product.reviews }} reviews)</span>
      </div>

      <!-- 💰 Pricing -->
      <div class="pricing-box">
        <p class="current-price">${{ product.price.toFixed(2) }}</p>

        <!-- Discount -->
        <p v-if="product.discount > 0" class="discount">
          Save ${{ product.discount.toFixed(0) }}
        </p>
      </div>

      <p class="description">
        {{ product.description }}
      </p>

      <!-- Quantity + Add to cart -->
      <div class="controls">
        <input
          type="number"
          v-model="quantity"
          min="1"
          class="quantity-input"
        />
        <button class="primary-btn" @click="addToCart">
          Add to Cart
        </button>
      </div>
    </div>
  </div>

  <!-- ❌ Product Not Found -->
  <div class="not-found" v-else>
    <img src="/404.jpg" alt="Product Not Found" />
    <h3>Oops! Product not found.</h3>
  </div>
</template>

<script>
export default {
  name: "ProductDetail",
  props: ["products"],
  data() {
    return {
      quantity: 1,
    };
  },
  computed: {
    product() {
      return this.products.find(
        (product) => product.id == this.$route.params.id
      );
    },
    productExists() {
      return !!this.product;
    },
  },
  methods: {
    addToCart() {
      this.$emit("addToCart", {
        productId: this.product.id,
        quantity: this.quantity,
      });
    },
  },
};
</script>

<style scoped>
/* Layout */
.product-detail {
  display: flex;
  gap: 2rem;
  padding: 2rem;
  justify-content: center;
  align-items: flex-start;
}

.image-box {
  flex: 1;
  max-width: 400px;
}

.image-box img {
  width: 100%;
  border-radius: 8px;
}

.info-box {
  flex: 1;
}

/* Title */
.title {
  font-size: 28px;
  font-weight: 700;
  margin-bottom: 10px;
}

/* Rating */
.rating-row {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

.stars {
  color: #ffc700;
  font-size: 20px;
}

.star {
  opacity: 0.3;
}

.star.filled {
  opacity: 1;
}

/* Pricing */
.pricing-box {
  margin: 10px 0;
}

.current-price {
  font-size: 26px;
  font-weight: bold;
  color: #1d252c;
}

.discount {
  font-size: 15px;
  color: #bb0628;
  font-weight: bold;
}

/* Description */
.description {
  margin: 20px 0;
  font-size: 16px;
  line-height: 1.5;
  color: #4a4a4a;
}

/* Controls */
.controls {
  display: flex;
  align-items: center;
  gap: 15px;
}

.quantity-input {
  width: 60px;
  padding: 6px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

.primary-btn {
  background-color: #ffe000;
  color: #001e73;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  font-weight: 700;
  cursor: pointer;
}

.primary-btn:hover {
  background-color: #fff455;
}

/* Not Found */
.not-found {
  text-align: center;
  margin-top: 3rem;
}

.not-found img {
  width: 300px;
}
</style>
