<template>
	<div>
	    <div class="row">
	        <div class="col-sm-12">
	            <div class="banner">
	                <h1>{{title}}</h1>
	            </div>
	        </div>
	    </div>

	    <div class="items">
	        <div class="row">
	        	<div class="col-sm-10">
	        		<div class="row">
	        			<div class="col-sm-3" v-for="(product, index) in products">
			                <img v-bind:src="product.image" v-bind:alt="product.name" class="img-responsive"/>

			                <h3>{{product.name}}</h3>

			                <h3>Price: {{product.price_with_symbol}}</h3>

			                <h3>Total: {{product.quantity}}</h3>

			                <button @click="addCart(index)" class="btn btn-success" :disabled="product.quantity == 0">Add To Cart</button>

	            		</div>
	        		</div>	        		 
	        	</div>
	        	<aside class="col-sm-2">
	        		<h2>Filter Products</h2>
	        		<div class="form-group">
	        			<label>Search</label>
	        			<input type="text" class="form-control" v-model="search" v-on:keyup="filterProducts" placeholder="search product">
	        		</div>
	        		<div class="form-group">
	        			<label>Price:</label>
	        			<range-slider class="slider"min="0"max="300"step="5"v-model="price"v-on:change="filterProducts"> </range-slider>
	        			<small class="help-block"> {{currency}} {{price}}</small>
	        		</div>
	        	</aside>
	           
	        
	        </div>
	    </div>

    <div class="row">
        <div class="col-md-12">
            <div class="banner">
                <h1>Cart</h1>
            </div>
        </div>

        <div class="row">
            <div class="col-md-8 col-md-offset-2 cart-items">

                <div class="row cart-item" v-for="item in cartItems">

                    <div class="col-md-2 item-action">
                        <a class="btn btn-danger remove" @click="removeProduct(item.id)">X</a>
                    </div>
                    <div class="col-md-6 item-name">
                        {{item.name}}
                    </div>
                    <div class="col-md-2 item-amount">
                        {{item.quantity}}
                    </div>
                    <div class="col-md-2 item-total">
                        {{item.price_with_symbol}}
                    </div>

                </div>

                <div class="row cart-total">

                    <div class="col-md-6">
                        Total
                    </div>
                    <div class="col-md-4">
                        &nbsp;
                    </div>
                    <div class="col-md-2 total">
                        {{cartTotal}}
                    </div>

                </div>

            </div>
        </div>
    </div>

</div>
	
</template>

<style scoped>
.banner{
  margin-bottom:50px;
}
.banner h1{
  font-size:24px;
  line-height:50px;
  padding-left:20px;
  color: #fff;
}

h2{
  font-size:20px;
}

h3{
  font-size:16px;
}
.banner{
  height:52px;
  background-color: #4CAF50;
}

.items{
  margin-top:50px;
  margin-bottom:50px;
}

.cart-item {
  font-size:18px;
  line-height:50px;
}

.cart-total {
  margin-top:40px;
  font-size:18px;
  font-weight:600;
  padding-top:20px;
  border-top:1px solid #bbb;
}
.slider {
  width: 200px;
}
</style>


<script>
import RangeSlider from 'vue-range-slider'
import 'vue-range-slider/dist/vue-range-slider.css'

export default{
	name: 'ShoppingCart',
	components: {
    	RangeSlider
  	},
	data () {
		let currency = '$ ';
	    return {	    	
	    	title: 'Vue Shopping Cart',
	      	cartItems: [],
	      	cartTotal: '$ 0.00',
	      	currency: currency,
	      	search: '',
	      	price: '300',	      	
	      	products:[
	      		{
	      			id:1,
	      			image: 'http://st.depositphotos.com/1001092/4768/i/450/depositphotos_47684611-Delicious--portion-of--fresh-salmon-fillet--with-aromatic-herbs.jpg',
	      			name: 'Salmon Fillet',
	      			quantity: '10',
	      			price: '250',
	      			price_with_symbol: currency + '250'	      		
	      		},
	      		{	      		
	      			id: 2,
	      			image: 'http://static3.depositphotos.com/1003814/174/i/450/depositphotos_1747408-Grilled-salmon.jpg',
	      			name: 'Salmon Grill',
	      			quantity: '4',
	      			price: '200',
	      			price_with_symbol: currency + '200'	      		
	      		},
	      		{	      		
	      			id: 3,
	      			image: 'http://static8.depositphotos.com/1290614/1019/i/450/depositphotos_10190735-Prawn-skewers.jpg',
	      			name: 'Prawn with Rice',
	      			quantity: '15',
	      			price: '300',
	      			price_with_symbol: currency + '300'	      		
	      		},
	      		{	      		
	      			id: 4,
	      			image: 'http://static8.depositphotos.com/1001944/923/i/450/depositphotos_9238445-Chicken-soup-with-vegetables.jpg',
	      			name: 'vegetable Soup',
	      			quantity: '8',
	      			price: '150',
	      			price_with_symbol: currency + '150'	      		
	      		}
	      	]
	    }
	},
	methods:{
		addCart(product){
			let myProduct = this.products[product];
			myProduct['quantity'] = myProduct['quantity'] - 1;			
			if (_.size(this.cartItems) == 0) {
				this.cartItems = [{
					id: myProduct['id'],
					name: myProduct['name'],
	      			quantity: '1',
	      			price: myProduct['price'],
	      			price_with_symbol: this.currency + (myProduct['price'] * 1)
				}]				
			}else{				
			
				if (_.size(_.find(this.cartItems, ['id', myProduct['id']])) == 0) {				
					let item = {
						id: myProduct['id'],
						name: myProduct['name'],
		      			quantity: '1',
		      			price: myProduct['price'],
		      			price_with_symbol: this.currency + (myProduct['price'] * 1)
					}
					this.cartItems = _.concat(this.cartItems, item)
				}else{
					let item = _.find(this.cartItems, ['id', myProduct['id']]);
					item.quantity = parseInt(item.quantity) + 1;
					item.price_with_symbol= this.currency + (item.price * item.quantity)
				}
			}
			this.getTotal();
		},
		removeProduct(product){					
			let products = _.find(this.products, ['id', product]);
			let item = _.find(this.cartItems, ['id', product]);
			products.quantity = parseInt(products.quantity) + parseInt(item.quantity); 
			_.remove(this.cartItems, o => o.id === product);
			this.getTotal()	
		},
		getTotal(){						
			this.cartTotal = this.currency+_.sumBy(this.cartItems, function(o){ return o.price * o.quantity });
		},
		filterProducts(){
			let search = this.search
			let price = this.price	

		    if (search != '') {
		    	this.products =  this.products.filter(item =>_.toLower(item.name).includes(_.toLower(search)) );
		    }

		    if (price != '') {
		    	this.products =  this.products.filter(item => item.price <= price );
		    }
		}
	}	
}
</script>