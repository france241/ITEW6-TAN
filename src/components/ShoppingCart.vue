<template>
    <div>
      <!-- Product Listings -->
      <div class="product-list">
        <div v-for="product in products" :key="product.id" class="product">
          <p>{{ product.name }} - ₱{{ product.price }}</p>
          <button @click="addToCart(product)">Add to Cart</button>
        </div>
      </div>
  
      <!-- Shopping Cart -->
      <div class="cart">
        <h2>Shopping Cart</h2>
        <div v-if="cart.length === 0">Cart is empty</div>
        <div v-else>
          <div v-for="(item, index) in cart" :key="index" class="cart-item">
            <p>{{ item.product.name }} - ₱{{ item.product.price }} x {{ item.quantity }}</p>
            <button @click="removeFromCart(index)">Remove</button>
            <input type="number" v-model="item.quantity" min="1" @input="updateQuantity(index)">
          </div>
          <p>Total: ₱{{ totalPrice }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        products: [
          { id: 1, name: 'Shawarma Rice', price: 65 },
          { id: 2, name: 'Sizzling Pork Sisig', price: 70 },
          { id: 3, name: 'Tapsilog', price: 55 },
          { id: 4, name: 'Bacsilog', price: 55 },
          { id: 5, name: 'Pares Bagnet', price: 60 },
          { id: 6, name: 'Siomai Rice', price: 35 },
          { id: 7, name: 'Turon', price: 15 },
          { id: 8, name: 'Iced Tea', price: 10 }
        ],
        cart: []
      };
    },
    computed: {
      totalPrice() {
        return this.cart.reduce((total, item) => {
          return total + (item.product.price * item.quantity);
        }, 0);
      }
    },
    methods: {
      addToCart(product) {
        const found = this.cart.find(item => item.product.id === product.id);
        if (found) {
          found.quantity++;
        } else {
          this.cart.push({ product: product, quantity: 1 });
        }
      },
      removeFromCart(index) {
        this.cart.splice(index, 1);
      },
      updateQuantity(index) {
        const quantity = parseInt(this.cart[index].quantity);
        if (quantity <= 0) {
          this.cart.splice(index, 1);
        }
      }
    }
  };
  </script>
  
  <style>
.product-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between; 
}

.product {
  flex: 0 0 calc(25% - 20px); 
  margin: 10px;
  padding: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); 
}

.product img {
  max-width: 100%;
  height: auto;
}

.product-title {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 5px;
}

.product-price {
  font-size: 14px;
  color: #555;
}

.product-description {
  font-size: 14px;
  color: #777;
  margin-top: 5px;
}

/* Cart Styling */
.cart {
  margin-top: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 15px;
}

.cart-item {
  margin-bottom: 10px;
}

.cart-item img {
  max-width: 60px;
  height: auto;
  margin-right: 10px;
}

.cart-item-details {
  display: flex;
  align-items: center;
}

.cart-item-title {
  font-size: 14px;
  font-weight: bold;
}

.cart-item-price {
  font-size: 14px;
  color: #555;
  margin-left: auto;
}

.cart-total {
  margin-top: 10px;
  font-size: 16px;
  font-weight: bold;
}

.cart-buttons {
  display: flex;
  justify-content: flex-end; 
  margin-top: 15px;
}

.cart-button {
  padding: 8px 15px;
  border-radius: 5px;
  background-color: #007bff;
  color: #fff;
  text-decoration: none;
  margin-left: 10px;
}

.cart-button:hover {
  background-color: #0056b3;
}

  </style>