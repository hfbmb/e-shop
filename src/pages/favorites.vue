<template>
  <v-container>
    <v-row justify="center">
      <!-- Если избранных товаров нет -->
      <v-col v-if="favoriteProducts.length === 0" cols="12" class="text-center">
        <v-alert type="info" variant="outlined">Нет избранных товаров</v-alert>
      </v-col>

      <!-- Список избранных товаров (по одному в линии) -->
      <v-col v-for="product in favoriteProducts" :key="product.id" cols="12">
        <v-card class="d-flex flex-row align-center pa-3">
          <!-- Изображение товара -->
          <v-img
            :src="product.image"
            class="rounded-lg"
            max-height="200"
            max-width="200"
            contain
          ></v-img>

          <!-- Информация о товаре -->
          <v-card-title class="text-h6 ms-4">{{ product.name }}</v-card-title>
          <v-card-subtitle class="ms-4">{{ product.category }}</v-card-subtitle>
          <div class="text-h5 font-weight-bold ms-4">{{ product.price }} ₽</div>

          <v-spacer></v-spacer>

          <!-- Кнопки управления -->
          <v-card-actions class="d-flex flex-column">
            <v-btn color="red" variant="text" @click="removeFromFavorites(product.id)">
              <v-icon start>mdi-heart-broken</v-icon> Удалить
            </v-btn>

            <v-btn :color="isInCart(product.id) ? 'green' : 'blue'" @click="handleCartAction(product)">
              <v-icon start>{{ isInCart(product.id) ? 'mdi-cart' : 'mdi-cart-plus' }}</v-icon>
              {{ isInCart(product.id) ? "В корзине" : "Добавить" }}
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  setup() {
    const favoriteProducts = ref([]);
    const cart = ref([]);

    // Загрузка избранных товаров из localStorage
    const loadFavorites = () => {
      favoriteProducts.value = JSON.parse(localStorage.getItem("favorites")) || [];
    };

    // Проверка, есть ли товар в корзине
    const isInCart = (id) => cart.value.some((item) => item.id === id);

    // Действие по кнопке (добавить в корзину)
    const handleCartAction = (product) => {
      if (!isInCart(product.id)) {
        cart.value.push({ ...product, quantity: 1 });
        localStorage.setItem("cart", JSON.stringify(cart.value));
      }
    };

    // Удаление товара из избранного
    const removeFromFavorites = (id) => {
      favoriteProducts.value = favoriteProducts.value.filter((product) => product.id !== id);
      localStorage.setItem("favorites", JSON.stringify(favoriteProducts.value));
    };

    onMounted(() => {
      loadFavorites();
      cart.value = JSON.parse(localStorage.getItem("cart")) || [];
    });

    return {
      favoriteProducts,
      isInCart,
      handleCartAction,
      removeFromFavorites,
    };
  },
};
</script>
