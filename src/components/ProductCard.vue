<template>
  <div class="product-card">
    <img :src="`/${product.image}`" :alt="product.name" class="product-image" />

    <router-link :to="`/product/${product.id}`" class="product-title">
      <h2>{{ product.name }}</h2>
    </router-link>

    <p class="description">{{ product.description }}</p>

    <div class="product-details">

      <!-- ⭐ Rating (only if exists) -->
      <div v-if="product.rating" class="rating">
        <span class="stars">
          ★★★★★
        </span>
        <span class="rating-value">{{ product.rating }}</span>
        <span class="reviews">({{ product.reviews }} reviews)</span>
      </div>

      <!-- 💸 Price + Discount -->
      <div class="product-price">
        <p class="price">${{ product.price.toFixed(2) }}</p>

        <p v-if="product.discount > 0" class="discount">
          Save ${{ product.discount.toFixed(0) }}
        </p>
      </div>

      <!-- 🛒 Add to cart -->
      <div class="product-controls">
        <input 
          type="number"
          v-model="quantity"
          min="1"
          class="quantity-input"
        />
        <button class="bby-btn" @click="addToCart">Add to Cart</button>
      </div>
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
  background: #ffffff;
  padding: 16px;
  border-radius: 8px;
  border: 1px solid #d1d5db;
  display: flex;
  flex-direction: column;
  text-align: left;
  transition: box-shadow 0.2s ease;
}

.product-card:hover {
  box-shadow: 0px 4px 12px rgba(0,0,0,0.12);
}

.product-image {
  width: 100%;
  border-radius: 6px;
  margin-bottom: 10px;
}

.product-title h2 {
  font-size: 18px;
  font-weight: 600;
  color: #0046be;
}

.description {
  color: #444;
  font-size: 14px;
  margin-bottom: 8px;
}

/* ⭐ Rating Section */
.rating {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-bottom: 6px;
}

.stars {
  color: #fbbf24;
}

.rating-value {
  font-weight: bold;
}

.reviews {
  font-size: 13px;
  color: #666;
}

/* 💸 Pricing */
.product-price {
  margin-bottom: 12px;
}

.price {
  font-size: 20px;
  font-weight: 700;
  color: #1d252c;
}

.discount {
  color: #d82a2a;
  font-size: 14px;
  font-weight: 600;
}

/* 🛒 Add to Cart */
.product-controls {
  display: flex;
  gap: 10px;
  align-items: center;
}

.quantity-input {
  width: 60px;
  padding: 6px;
  border-radius: 4px;
  border: 1px solid #c5cbd5;
  text-align: center;
}

.bby-btn {
  background: #ffe000;
  border: none;
  color: #001e73;
  padding: 8px 16px;
  font-weight: 700;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s ease;
}

.bby-btn:hover {
  background: #fff200;
}
</style>
