<template>
  <div class="detail-wrapper" v-if="productExists">

    <!-- IMAGE -->
    <div class="image-section">
      <img :src="`/${product.image}`" :alt="product.name" />
    </div>

    <!-- INFO -->
    <div class="info-section">
      <h1 class="title">{{ product.name }}</h1>

      <div class="rating-row">
        <span class="stars">
          <span
            v-for="n in 5"
            :key="n"
            class="star"
            :class="{ filled: n <= Math.round(product.rating) }"
          >★</span>
        </span>
        <span class="reviews">({{ product.reviews }} reviews)</span>
      </div>

      <p class="description">{{ product.description }}</p>

      <!-- Price -->
      <div class="price-box">
        <div class="price-row">
          <span class="current-price">${{ product.price.toFixed(2) }}</span>

          <span v-if="product.discount > 0" class="old-price">
            ${{ (product.price + product.discount).toFixed(2) }}
          </span>
        </div>

        <p v-if="product.discount > 0" class="discount">
          SAVE ${{ product.discount }}
        </p>
      </div>

      <!-- Cart Controls -->
      <div class="cart-controls">
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

  </div>

  <!-- NOT FOUND -->
  <div class="detail-wrapper" v-else>
    <img src="../assets/404.jpg" alt="Product not found" class="not-found-img" />
    <h3 class="not-found-msg">Oops! That product does not exist…</h3>
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
        (p) => p.id == this.$route.params.id
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
.detail-wrapper {
  display: flex;
  gap: 2rem;
  padding: 2rem;
  background: #fff;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  border-radius: 10px;
  margin: 2rem auto;
  max-width: 1200px;
}

.image-section img {
  width: 450px;
  height: auto;
  object-fit: contain;
  background: #f7f7f7;
  padding: 10px;
  border-radius: 8px;
}

/* INFO SECTION */
.info-section {
  flex: 1;
}

.title {
  font-size: 28px;
  color: #0046be;
  margin-bottom: 8px;
}

.rating-row {
  display: flex;
  align-items: center;
  margin-bottom: 15px;
}

.stars .star {
  color: #ccc;
  font-size: 20px;
  margin-right: 2px;
}

.stars .star.filled {
  color: #ffb500;
}

.reviews {
  margin-left: 8px;
  color: #555;
}

.description {
  color: #444;
  margin-bottom: 20px;
  font-size: 16px;
}

/* PRICE */
.price-box {
  margin-bottom: 20px;
}

.price-row {
  display: flex;
  align-items: center;
  gap: 10px;
}

.current-price {
  font-size: 28px;
  font-weight: bold;
}

.old-price {
  text-decoration: line-through;
  color: #777;
}

.discount {
  font-weight: bold;
  color: #bb0628;
}

/* CART */
.cart-controls {
  display: flex;
  align-items: center;
  gap: 12px;
}

.quantity-input {
  width: 70px;
  padding: 8px;
  border: 1px solid #c5cbd5;
  border-radius: 4px;
}

.add-btn {
  background: #ffe000;
  color: #001e73;
  padding: 12px 20px;
  border: none;
  border-radius: 4px;
  font-weight: bold;
  cursor: pointer;
}

.add-btn:hover {
  background: #ffea00;
}

/* RESPONSIVE */
@media (max-width: 900px) {
  .detail-wrapper {
    flex-direction: column;
    text-align: center;
  }

  .image-section img {
    width: 100%;
    max-width: 350px;
  }
}
</style>
