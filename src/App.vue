<template>
  <div id="app">
    <Products v-if='page==="products"' :products='products' @viewSummary="viewSummary"></Products>
    <Summary v-if='page==="summary"' :cart='cart' @changePage="changePage"></Summary>
    <Checkout v-if='page==="checkout"' @changePage="changePage"></Checkout>
  </div>
</template>

<script>
import Products from './components/Products.vue'
import Summary from './components/Summary.vue'
import Checkout from './components/Checkout.vue'
import * as productsJSON from './bikerentals.json'
export default {
  name: 'app',
  components: {
    Products,
    Summary,
    Checkout
  },
  data(){
    return{
      products: productsJSON.default.products,
      cart:[],
      page:'products'
    }
  },
  methods:{
    viewSummary(cart){
      this.cart=cart
      this.page='summary'
    },
    changePage(newPage){
      this.page=newPage
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
/*https://codepen.io/cbracco/pen/ywdbm*/

button {
  display: inline-block;
  margin: 3px 5px;
  padding: 1px 14px;
  border: 0;
  border-radius: 0.317rem;
  background-color: #aaa;
  color: #fff;
  text-decoration: none;
  font-weight: 700;
  font-size: 1.5rem;
  line-height: 1.5;
  cursor: pointer;
  -webkit-appearance: none;
  -webkit-font-smoothing: antialiased;
}

button:hover {
  opacity: 0.85;
}

button:active {
  box-shadow: inset 0 3px 4px hsla(0, 0%, 0%, 0.2);
}

button:focus {
  outline: thin dotted #444;
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}

.button_primary {
  background-color: #1fa3ec;
}

.button_secondary {
  background-color: #e98724;
}

.button-icon {
  display: inline-block;
  position: relative;
  top: -0.1em;
  vertical-align: middle;
  margin-right: 0.317rem;
}
</style>
