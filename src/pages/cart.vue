<template>
  <v-container>
    <v-row>
      <v-col v-for="(item, index) in cartItems" :key="item.id" cols="12" sm="4">
        <v-card>
          <v-img :src="item.image" height="200px"></v-img>
          <v-card-title>{{ item.name }}</v-card-title>
          <v-card-subtitle>{{ item.price }} USD</v-card-subtitle>
          <v-btn @click="removeItem(index)">Remove</v-btn>
          <v-btn @click="changeQuantity(index, 1)">+</v-btn>
          <v-btn @click="changeQuantity(index, -1)">-</v-btn>
          <v-btn>{{ item.quantity }}</v-btn>
        </v-card>
      </v-col>
    </v-row>
    <v-divider></v-divider>
    <v-row>
      <v-col>
        <v-btn @click="checkout">Checkout</v-btn>
      </v-col>
      <v-col class="text-right">
        <v-btn>{{ totalPrice }} USD</v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const cartItems = ref([
      { id: 1, name: 'Product 1', price: 100, image: 'image1.jpg', quantity: 1 },
      { id: 2, name: 'Product 2', price: 200, image: 'image2.jpg', quantity: 2 },
    ]);

    const totalPrice = computed(() => {
      return cartItems.value.reduce((sum, item) => sum + item.price * item.quantity, 0);
    });

    const changeQuantity = (index, delta) => {
      if (cartItems.value[index].quantity + delta > 0) {
        cartItems.value[index].quantity += delta;
      }
    };

    const removeItem = (index) => {
      cartItems.value.splice(index, 1);
    };

    const checkout = () => {
      // Handle checkout logic
      console.log('Proceed to checkout');
    };

    return {
      cartItems,
      totalPrice,
      changeQuantity,
      removeItem,
      checkout
    };
  }
};
</script>
