<template>
  <div id="app" class="container mt-5">
    <checkout       
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      @delete="deleteItem"
      @add="addItem"></checkout>
    <products 
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      @toggle="toggleSliderStatus"
      @delete="deleteItem"
      :slider-status="sliderStatus" 
      :maximum.sync="maximum"
      :products="products"
      @add="addItem"></products>
  </div>
</template>

<script>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import Products from "./components/Products.vue";
import Checkout from "./components/Checkout.vue";

export default {
  name: "app",
  data: function() {
    return {
      maximum : 99,
      products: null,
      cart:[],
      sliderStatus: true,
    };
  },
  methods: {
    toggleSliderStatus: function() {
      this.sliderStatus = !this.sliderStatus;
    },
    deleteItem: function(id) {
      if (this.cart[id].qty > 1) {
        this.cart[id].qty--;
      } else {
        this.cart.splice(id, 1);
      }
    },
    addItem: function(product) {
      console.log("here");
      var whichProduct;
      var existing = this.cart.filter(function(item, index) {
        if (item.product.id == Number(product.id)) {
          whichProduct = index;
          return true;
        } else {
          return false;
        }
      });
      if (existing.length) {
        this.cart[whichProduct].qty++;
      } else {
        this.cart.push({ product: product, qty: 1 });
      }
    }
  },
  components: {
    Products,
    Checkout
  },
  computed: {
    cartTotal: function() {
      let sum = 0;
      var key;
      for (key in this.cart) {
        sum = sum+(this.cart[key].product.price * this.cart[key].qty);
      }
      return sum;
      },
    cartQty: function() {
      let qty = 0;
      var key;
      for (key in this.cart) {
        qty = qty + this.cart[key].qty;
      }
      return qty;
    }
  },
  mounted: function() {
    fetch('https://hplussport.com/api/products/order/price')
    .then(response => response.json())
    .then(data => {
      this.products = data;
    })
  }
};
</script>
