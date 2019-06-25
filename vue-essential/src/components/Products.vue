<template>
    <div>
        <h1>My shop</h1>
        <navbar
        :cart="cart"
        :cartQty="cartQty"
        :cartTotal="cartTotal"></navbar>
        <price-slider :slider-status="sliderStatus" :maximum.sync="maximum"></price-slider>
        <product-list :maximum="maximum" :products="products"></product-list>
    </div>
</template>

<script>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import ProductList from "./ProductList.vue";
import PriceSlider from "./PriceSlider.vue";
import Navbar from "./Navbar.vue";

export default {
    name: 'products',
    props: ["products", "maximum", "cart", "cartQty", "cartTotal", "sliderStatus", "sliderState"],
    components: {
        ProductList,
        PriceSlider,
        Navbar
    }
}
</script>

<style>

</style>
