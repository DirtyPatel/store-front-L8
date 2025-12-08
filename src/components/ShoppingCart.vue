<template>
  <div class="cart-wrapper">

    <h1>Your Cart</h1>

    <table v-if="hasCartItems" class="cart-table">
      <thead>
        <tr>
          <th>Product</th>
          <th>Qty</th>
          <th>Price</th>
          <th>Total</th>
          <th></th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="(item, index) in cartItems" :key="item.product.id">
          <td>{{ item.product.name }}</td>
          <td>{{ item.quantity }}</td>
          <td>${{ item.product.price.toFixed(2) }}</td>
          <td>${{ getItemTotal(item) }}</td>

          <td>
            <button class="remove-btn" @click="$emit('removeFromCart', index)">
              Remove
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <p v-else class="empty-msg">
      Your cart is empty.
    </p>

    <button 
      v-if="hasCartItems"
      class="checkout-btn"
      @click="$emit('submitOrder')"
    >
      Checkout
    </button>

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
    }
  }
};
</script>

<style scoped>
.cart-wrapper {
  padding: 2rem;
  background: white;
  margin: 2rem auto;
  max-width: 900px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
}

h1 {
  margin-bottom: 1rem;
  color: #0046be;
}

/* TABLE */
.cart-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 2rem;
}

.cart-table th {
  background: #f4f6f8;
  padding: 12px;
  text-align: left;
  font-size: 14px;
  border-bottom: 2px solid #d9dde3;
}

.cart-table td {
  padding: 12px;
  border-bottom: 1px solid #eee;
}

.remove-btn {
  background: #bb0628;
  color: white;
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  font-size: 13px;
  cursor: pointer;
}

.remove-btn:hover {
  background: #a00522;
}

/* EMPTY */
.empty-msg {
  text-align: center;
  font-size: 18px;
  color: #555;
}

/* CHECKOUT BUTTON */
.checkout-btn {
  background: #ffe000;
  color: #001e73;
  padding: 14px 24px;
  font-weight: bold;
  font-size: 16px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  display: block;
  margin: 0 auto;
}

.checkout-btn:hover {
  background: #ffea00;
}
</style>
