<template>
  <div class="product-card">
    <router-link :to="`/product/${product.id}`" class="image-wrapper">
      <img :src="`/${product.image}`" :alt="product.name" />
    </router-link>

    <router-link :to="`/product/${product.id}`" class="name-link">
      <h2 class="product-name">{{ product.name }}</h2>
    </router-link>

    <!-- Rating + Reviews -->
    <div class="rating-section">
      <span class="stars">
        <span
          v-for="n in 5"
          :key="n"
          class="star"
          :class="{ filled: n <= Math.round(product.rating) }"
        >★</span>
      </span>
      <span class="reviews">({{ product.reviews }})</span>
    </div>

    <!-- Price + Discount -->
    <div class="price-section">
      <div class="price-wrapper">
        <p class="current-price">${{ product.price.toFixed(2) }}</p>

        <p v-if="product.discount > 0" class="old-price">
          ${{ (product.price + product.discount).toFixed(2) }}
        </p>
      </div>

      <p v-if="product.discount > 0" class="discount-tag">
        SAVE ${{ product.discount }}
      </p>
    </div>

    <!-- Controls -->
    <div class="product-controls">
      <input 
        type="number" 
        v-model="quantity" 
        min="1" 
        class="quantity-input" 
      />

      <button class="add-btn" @click="addToCart">
        Add to Cart
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductCard",
  props: ["product"],
  data() {
    return {
      quantity: 1,
    };
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
.product-card {
  background: #fff;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  transition: transform 0.15s ease, box-shadow 0.15s ease;
  margin: 12px;
  max-width: 320px;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 14px rgba(0,0,0,0.12);
}

/* IMAGE */
.image-wrapper img {
  width: 100%;
  height: 220px;
  object-fit: contain;
  background: #f7f7f7;
  border-radius: 6px;
}

.name-link {
  text-decoration: none;
}

.product-name {
  font-size: 18px;
  font-weight: bold;
  color: #0046be;
  margin: 12px 0 6px 0;
}

/* RATING */
.rating-section {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}

.stars .star {
  color: #d0d0d0;
  font-size: 16px;
}

.stars .star.filled {
  color: #ffb500;
}

.reviews {
  color: #555;
  margin-left: 6px;
  font-size: 14px;
}

/* PRICE */
.price-section {
  margin: 10px 0;
}

.price-wrapper {
  display: flex;
  align-items: center;
  gap: 10px;
}

.current-price {
  font-size: 20px;
  font-weight: bold;
  color: #1d252c;
}

.old-price {
  text-decoration: line-through;
  color: #888;
}

.discount-tag {
  font-weight: bold;
  font-size: 14px;
  color: #bb0628;
  margin-top: 4px;
}

/* CONTROLS */
.product-controls {
  margin-top: 16px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.quantity-input {
  width: 60px;
  padding: 6px;
  border: 1px solid #c5cbd5;
  border-radius: 4px;
  text-align: center;
}

.add-btn {
  background: #ffe000;
  color: #001e73;
  border: none;
  padding: 10px 16px;
  border-radius: 4px;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.2s;
}

.add-btn:hover {
  background: #ffea00;
}
</style>
