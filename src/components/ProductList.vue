<template>
  <div class="product-list">
    <ProductCard
      v-for="product in products"
      :key="product.id"
      :product="product"
      @addToCart="addToCart"
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
    addToCart({ productId, quantity }) {
      this.$emit("addToCart", { productId, quantity });
    },
  },
};
</script>

<style scoped>
.product-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 20px;
  padding: 20px;
}
</style>
