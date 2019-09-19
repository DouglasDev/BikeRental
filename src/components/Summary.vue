<template>
  <main>
    <h1>Order Summary</h1>
    <div class='productList'>
      <div class='productDetails' v-for="product in cart">
        <img :src="product.image" :alt="product.name">
        <div class='product'>
          <h3 class='productDescription'> {{product.name}}</h3>
          <h3 class='productDescription'> Quantity: {{product.quantity}}</h3>
        </div>
      </div><br>
    </div>
    <h2>Total: $ {{total}}</h2>
    <button @click="changeOrder" class="button_secondary">Change Order</button>
    <button @click="confirmPurchase" class="button_primary">Confirm Purchase</button>
  </main>
</template>

<script>


export default {
  name: 'Summary',
  data(){
  	return{
	    total:0,
  	} 
  },
  props: {
    cart: Array,
  },
  mounted(){
      this.total= this.cart.reduce((acc,product)=> product.quantity>0 ? acc+product.quantity*product.price : acc, 0).toFixed(2);
  },
  methods:{
  	confirmPurchase(){
  		this.$emit('changePage', 'checkout')
  	},
  	changeOrder(){
  		this.$emit('changePage', 'products')
  	},
  }
}

</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	img{
		height: 50px;
		margin-right: 1rem;
	}
	.productList{
 	    flex-direction: column;
		display: inline-flex;
	}
	.productDescription{
/*		display: inline-flex;
	    align-items: center;
*/	
    margin:0;
    padding:0;
  }
	.productDetails{
        margin: 1rem;
		display: inline-flex;
	    align-items: center;
	    flex-wrap: wrap;
	    justify-content: center;
   		width: auto;
	    justify-content: space-between;
   	}
	.quantity{
		background: light-grey;
		padding:25px;
	}
  .product{
    text-align: right;
  }
  @media only screen and (min-width: 500px) {
    .product {
      display: flex;
      justify-content: space-between;
      min-width: 20rem;
    }
  }

</style>
