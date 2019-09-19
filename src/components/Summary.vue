<template>
  <main>
    <h1>Order Summary</h1>
    <div class='productList'>
      <div class='productDetails' v-for="product in cart">
        <h3 class='productDescription'><img :src="product.image" :alt="product.name"> {{product.name}}</h3>
        <h3 class='quantity'> Quantity: {{product.quantity}}</h3>
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
      this.total= this.cart.reduce((acc,product)=> product.quantity>0 ? acc+product.quantity*product.price : acc, 0)
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
		display: inline-flex;
	    align-items: center;
	}
	.productDetails{
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

</style>
