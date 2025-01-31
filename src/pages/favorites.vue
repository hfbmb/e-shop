<template>
  <v-container>
    <v-row>
      <v-col v-for="product in favoriteProducts" :key="product.id" cols="12" sm="4">
        <v-card>
          <v-img :src="product.image" height="200px"></v-img>
          <v-card-title>{{ product.name }}</v-card-title>
          <v-card-subtitle>{{ product.category }}</v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const favoriteProducts = ref([
      // example favorite products
      { id: 1, name: 'Product 1', image: 'image1.jpg', category: 'Category 1' },
      { id: 2, name: 'Product 2', image: 'image2.jpg', category: 'Category 2' },
    ]);

    return {
      favoriteProducts
    };
  }
};
</script>
