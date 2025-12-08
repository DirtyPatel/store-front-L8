<template>
  <div class="product-list">
    <ProductCard
      v-for="product in products"
      :key="product.id"
      :product="product"
      @addToCart="forwardAddToCart"
    />
  </div>
</template>

<script>
import ProductCard from "./ProductCard.vue";

export default {
  name: "ProductList",
  props: ["products"],
  components: {
    ProductCard,
  },
  methods: {
    forwardAddToCart(payload) {
      this.$emit("addToCart", payload);
    },
  },
};
</script>

<style scoped>
.product-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 24px;
  padding: 24px;
  max-width: 1300px;
  margin: 0 auto;
}
</style>
