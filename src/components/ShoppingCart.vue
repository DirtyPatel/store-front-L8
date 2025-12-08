<template>
  <div class="cart-container">
    <h2 class="cart-title">Your Cart</h2>

    <div v-if="hasCartItems" class="cart-content">
      <table class="shopping-cart-table">
        <thead>
          <tr>
            <th class="left">Item</th>
            <th>Qty</th>
            <th>Price</th>
            <th>Total</th>
            <th></th>
          </tr>
        </thead>

        <tbody>
          <tr 
            v-for="item in cartItems" 
            :key="item.product.id"
          >
            <td class="left product-name">
              {{ item.product.name }}
            </td>

            <td>{{ item.quantity }}</td>

            <td>\${{ item.product.price.toFixed(2) }}</td>

            <td class="bold">\${{ getItemTotal(item) }}</td>

            <td>
              <button class="remove-btn" @click="removeFromCart(item)">
                Remove
              </button>
            </td>
          </tr>
        </tbody>
      </table>

      <button class="checkout-btn" @click="submitOrder">
        Checkout
      </button>
    </div>

    <div v-else class="empty-state">
      <h3>Your cart is empty</h3>
      <p>Browse products and add something you like!</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "ShoppingCart",
  props: ["cartItems"],
  computed: {
    hasCartItems() {
      return this.cartItems.length > 0;
    }
  },
  methods: {
    getItemTotal(item) {
      return (item.quantity * item.product.price).toFixed(2);
    },
    removeFromCart(item) {
      const index = this.cartItems.indexOf(item);
      if (index > -1) {
        this.$emit("removeFromCart", index);
      }
    },
    submitOrder() {
      this.$emit("submitOrder");
    }
  }
};
</script>

<style scoped>
.cart-container {
  max-width: 900px;
  margin: 40px auto;
  padding: 16px;
}

.cart-title {
  font-size: 28px;
  margin-bottom: 24px;
  text-align: center;
  font-weight: 700;
}

.shopping-cart-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 24px;
}

.shopping-cart-table th,
.shopping-cart-table td {
  padding: 12px;
  border-bottom: 1px solid #ddd;
  text-align: center;
}

.left {
  text-align: left !important;
}

.bold {
  font-weight: 700;
}

.product-name {
  width: 45%;
}

.remove-btn {
  background-color: #e53935;
  color: white;
  border: none;
  padding: 8px 14px;
  border-radius: 4px;
  cursor: pointer;
}

.remove-btn:hover {
  background-color: #c62828;
}

.checkout-btn {
  display: block;
  margin: 20px auto 0;
  padding: 12px 28px;
  font-size: 18px;
  font-weight: 600;
  background-color: #ffe000;
  color: #001e73;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.checkout-btn:hover {
  background-color: #fff200;
}

.empty-state {
  text-align: center;
  margin-top: 40px;
  color: #444;
}
</style>
