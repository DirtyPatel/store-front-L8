<template>
  <nav class="nav-bar">
    <div class="logo-area">
      <router-link to="/">
        <img src="/bestbuy-logo.png" alt="BestBuy Logo" class="logo-img" />
      </router-link>
    </div>

    <button class="hamburger" @click="toggleNav">
      <span class="hamburger-icon"></span>
    </button>

    <ul class="nav-links" :class="{ 'open': isNavOpen }">
      <li>
        <router-link to="/" @click="closeNav">Products</router-link>
      </li>
      <li>
        <router-link to="/cart" @click="closeNav">
          🛒 Cart 
          <span class="cart-count">{{ cartItemCount }}</span>
        </router-link>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: "TopNav",
  props: ["cartItemCount"],
  data() {
    return { isNavOpen: false };
  },
  methods: {
    toggleNav() {
      this.isNavOpen = !this.isNavOpen;
    },
    closeNav() {
      this.isNavOpen = false;
    }
  }
};
</script>

<style scoped>
/* NAV BAR */
.nav-bar {
  background-color: #0046be;
  color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 20px;
  position: fixed;
  width: 100%;
  top: 0;
  z-index: 5000;
  box-shadow: 0 2px 6px rgba(0,0,0,0.1);
}

.logo-img {
  width: 120px;
  height: auto;
}

/* LINKS */
.nav-links {
  display: flex;
  list-style: none;
  gap: 24px;
  font-weight: bold;
  font-size: 17px;
}

.nav-links a {
  color: white;
  text-decoration: none;
}

.nav-links a:hover {
  text-decoration: underline;
}

/* CART COUNT BADGE */
.cart-count {
  background: #ffe000;
  color: #001e73;
  padding: 2px 8px;
  border-radius: 50px;
  font-weight: bold;
  margin-left: 6px;
}

/* MOBILE */
.hamburger {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
}

.hamburger-icon {
  width: 25px;
  height: 3px;
  background: white;
  position: relative;
}

.hamburger-icon::before,
.hamburger-icon::after {
  content: "";
  width: 25px;
  height: 3px;
  background: white;
  position: absolute;
  left: 0;
}

.hamburger-icon::before {
  top: -7px;
}

.hamburger-icon::after {
  top: 7px;
}

@media (max-width: 768px) {
  .hamburger {
    display: block;
  }

  .nav-links {
    position: absolute;
    top: 64px;
    right: 0;
    left: 0;
    background: #003a9d;
    flex-direction: column;
    padding: 1rem;
    gap: 1.25rem;
    display: none;
  }

  .nav-links.open {
    display: flex;
  }
}
</style>
