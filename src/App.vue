<template>
  <TopNav :cartItemCount="cartItemCount"/>
  <router-view
    :products="products"
    :cartItems="cartItems"
    @addToCart="addToCart"
    @removeFromCart="removeFromCart"
    @submitOrder="submitOrder"
  ></router-view>
</template>

<script>
import TopNav from './components/TopNav.vue'

export default {
  name: 'App',
  components: {
    TopNav
  },
  data() {
    return {
      cartItems: [],
      products: [],
    }
  },
  computed: {
    cartItemCount() {
      return this.cartItems.reduce((total, item) => {
        return total + item.quantity
      }, 0)
    }
  },
  mounted() {
    this.getProducts()
  },
  methods: {
    getProducts() {
      fetch('/products')
        .then(response => response.json())
        .then(products => {
          console.log('success getting proxy products')
          this.products = products
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while fetching products')
        })
    },
    addToCart({ productId, quantity }) {
      // check if the product is already in the cart
      const existingCartItem = this.cartItems.find(
        item => item.product.id == productId
      )
      if (existingCartItem) {
        // if it is, increment the quantity
        existingCartItem.quantity += quantity
      } else {
        // if not, find the product, and add it with quantity to the cart
        const product = this.products.find(product => product.id == productId)
        this.cartItems.push({ product, quantity })
      }
    },
    removeFromCart(index) {
      this.cartItems.splice(index, 1)
    },
    submitOrder() {
      // get the order-service URL from an environment variable
      // const orderServiceUrl = process.env.VUE_APP_ORDER_SERVICE_URL;

      // create an order object
      const order = {
        customerId: Math.floor(Math.random() * 10000000000).toString(),
        items: this.cartItems.map(item => {
          return {
            productId: item.product.id,
            quantity: item.quantity,
            price: item.product.price
          }
        })
      }

      console.log(JSON.stringify(order));

      // call the order-service using fetch
      fetch(`/order`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(order)
      })
        .then(response => {
          console.log(response)
          if (!response.ok) {
            alert('Error occurred while submitting order')
          } else {
            this.cartItems = []
            alert('Order submitted successfully')
          }
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while submitting order')
        })
    }
  },
}
</script>

<style>
/* ===== Global BestBuy-Themed Styles ===== */

body {
  margin: 0;
  padding: 0;
  background-color: #f4f6f9; /* BestBuy clean gray */
  font-family: 'Inter', Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  text-align: center;
  color: #1f1f1f;
  margin-top: 100px; /* space for sticky top nav */
}

/* Remove old green footer */
footer {
  display: none;
}

/* Buttons (BestBuy Blue) */
button {
  padding: 10px 16px;
  background-color: #0046be; /* BestBuy blue */
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s ease;
}

button:hover {
  background-color: #003a9d;
}

/* PRODUCT GRID */
.product-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1.5rem;
  padding: 1rem 2rem;
}

/* PRODUCT CARD */
.product-card {
  background: #fff;
  border-radius: 6px;
  padding: 1rem;
  box-shadow: 0 2px 6px rgba(0,0,0,0.06);
  transition: transform 0.15s ease;
}

.product-card:hover {
  transform: translateY(-4px);
}

.product-card img {
  max-width: 85%;
  height: 180px;
  object-fit: contain;
  margin-bottom: 1rem;
}

.product-card h2 {
  font-size: 1.1rem;
  font-weight: 600;
  min-height: 48px;
}

.product-price {
  font-size: 1.3rem;
  font-weight: bold;
  color: #0046be;
}

/* Shopping Cart Table */
.shopping-cart {
  width: 90%;
  margin: 0 auto;
  background: white;
  padding: 1rem;
  border-radius: 6px;
}

.shopping-cart-table {
  width: 100%;
  border-collapse: collapse;
}

.shopping-cart-table th {
  background: #f2f4f7;
  font-weight: bold;
}

.shopping-cart-table td, .shopping-cart-table th {
  padding: 12px;
  border-bottom: 1px solid #ddd;
}

.checkout-button {
  margin-top: 20px;
  width: 200px;
  font-size: 1rem;
}

</style>
