<style>
  @import '//maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css';
</style>

<template>
	<div class="container">
		<div id="app">
	    	<ShoppingCart/>
	  </div>
	</div>
  
</template>

<script>
import ShoppingCart from './components/ShoppingCart'

export default {
  name: 'App',
  components: {
    ShoppingCart
  }
}
</script>
