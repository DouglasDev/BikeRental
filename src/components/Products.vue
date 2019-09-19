<template>
  <main>
    <h1>Products</h1>
    <div>
      <button 
        v-for="productType in Object.keys(productTypes)" 
        @click='changeProductType(productType)' 
        :class='currentProductType===productType? "button_secondary":""'>
        {{productTypes[productType]}} 
      </button>
    </div>
    <div class='productList' >
      <div class='productDetails' 
      :class="product.quantity>0?'selectedProduct':''" 
      v-for="product in products" 
      v-show="product.product_type===currentProductType || currentProductType==='all'">
        <h2>{{product.name}}</h2>
        <img :src="product.image" :alt="product.name">
        <h3>$ {{product.price}}</h3>
        <div class="quantity">
          <span>Quantity: </span>
          <button @click='changeQuantity(product.id,-1)'>-</button>
          <span class="lgText"> {{product.quantity}} </span>
          <button @click='changeQuantity(product.id,1)'>+</button>
        </div>
      </div>
      <br>
    </div>
    <div class="orderInfo">
      <h3 v-for="warning in warnings" class="warning">{{warning}}</h3>
      <h2 class="total">
        <div>Total: $ {{total}}  &nbsp; </div>
        <button class="button_primary" @click="checkout">Proceed to Checkout</button>
      </h2>
    </div>
  </main>
</template>

<script>
export default {
  name: 'Products',
  props: {
    products: Array
  },
  data(){
    return {
      warnings:[],
      currentProductType:'all',
      productTypes:{all:'All',bike:'Bikes',accessory:'Accessories',addon:'Add-ons'}
    }
  },
  created(){
    this.products.forEach((product,index)=>{
      if(!product.quantity)this.$set(this.products[index], 'quantity', 0)
    })
  },
  computed:{
    total(){
      return this.products.reduce((acc,product)=> product.quantity>0 ? acc+product.quantity*product.price : acc, 0).toFixed(2);
    }
  },
  methods:{
    changeProductType(productType){
      this.currentProductType=productType
    },
    checkout(){
      const cart = this.products.filter(product=>product.quantity>0)
      this.warnings=[]
      if (!cart.some(product=>product.product_type=='bike'))this.warnings.push('Please Select At Least One Bike.')
      if (this.warnings.length) return
      else this.$emit('viewSummary', cart)
    },
    changeQuantity(id,val){
      const index = this.products.findIndex(product=> product.id===id)
      const product= this.products[index]
      this.$set(product, 'quantity', product.quantity+val)
      if (product.quantity<0)this.$set(product, 'quantity',0)
    }
  }
}

//https://css-tricks.com/styling-based-on-scroll-position/
const debounce = (fn) => {
  let frame;
  return (...params) => {
    if (frame) { 
      cancelAnimationFrame(frame);
    }
    frame = requestAnimationFrame(() => {
      fn(...params);
    });

  } 
};
function getScrollPercent() {
    var h = document.documentElement, 
        b = document.body,
        st = 'scrollTop',
        sh = 'scrollHeight';
    return parseInt((h[st]||b[st]) / ((h[sh]||b[sh]) - h.clientHeight) * 100,10)
}

const storeScroll = () => {
  document.documentElement.dataset.scroll = getScrollPercent();
}
document.addEventListener('scroll', debounce(storeScroll), { passive: true });

storeScroll();

</script>

<style scoped>
  input{
    width: 2rem;
    height: 1.5rem;
    font-size: large;
    text-align: center;
  }
  main{
    padding-bottom: 8rem
  }
  .productList{
    margin: auto;
    display: flex;
    max-width: 70rem;
    flex-wrap: wrap;
    justify-content: center;
  }
  .productDetails{
    margin:1rem;
    padding:1rem;
    border-radius: 10px;
  }
  .selectedProduct{
    background: oldlace
  }
  .selectedProductType{
    border: 5px solid lightgray;
  }
  .orderInfo{
    width:100vw;
    position: fixed;
    bottom: 0;
    left: 0;
    transition: all linear .1s;
    background: white;
  }
  .quantity{
    display: inline-flex;
    align-items: center;
  }
  .quantity > * {
    margin-right: 5px;
    margin-left: 5px;
  }
  .total{
    align-items: center;
    display: inline-flex;
    margin-top: 0;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 1rem;
  }
  .warning{
    margin-bottom: 0;
  }
  .lgText{
    font-size: 1.5rem
  }

  html:not([data-scroll='100']) .orderInfo{
    box-shadow: 0px -2px 13px -5px rgba(0,0,0,0.75);
  }

</style>
