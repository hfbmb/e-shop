<template>
  <v-container>
    <v-row>
      <v-col
        v-for="product in filteredProducts"
        :key="product.id"
        cols="12"
        sm="6"
        md="4"
        lg="3"
      >
        <v-card elevation="3" class="position-relative">
          <!-- Изображение товара -->
          <v-img
            :src="product.image"
            height="220px"
            cover
            class="d-flex justify-center align-center"
          >
            <template v-slot:placeholder>
              <v-row class="fill-height ma-0" align="center" justify="center">
                <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
              </v-row>
            </template>
          </v-img>

          <!-- Иконка "Избранное" справа -->
          <v-btn
          	icon
          	variant="text"
          	class="position-absolute top-0 end-0 ma-2"
            @click="toggleFavorite(product)"
          >
            <v-icon :color="isFavorite(product.id) ? 'red' : 'black'">mdi-cards-heart-outline</v-icon>
          </v-btn>

          <!-- Контент карточки -->
          <v-card-title class="text-center">{{ product.name }}</v-card-title>
          <v-card-subtitle class="text-center">{{ product.category }}</v-card-subtitle>

          <v-card-text class="text-center font-weight-bold text-primary">
            {{ product.price.toLocaleString() }} ₸
          </v-card-text>

          <!-- Кнопка "Добавить в корзину" / "Перейти в корзину" -->
          <v-card-actions>
            <v-btn
              block
              :color="isInCart(product.id) ? 'green' : 'blue'"
              @click="handleCartAction(product)"
            >
              <v-icon left>{{ isInCart(product.id) ? 'mdi-cart' : 'mdi-cart-plus' }}</v-icon>
              {{ isInCart(product.id) ? 'Перейти в корзину' : 'Добавить в корзину' }}
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { computed, ref } from "vue";
import { useRouter } from "vue-router";
import useProducts from "@/composables/useProducts";

export default {
  props: {
    selectedCategory: String,
  },
  setup(props) {
    const { products, getAllProducts } = useProducts();
    const favorites = ref([]);
    const cart = ref([]);
    const router = useRouter();

    getAllProducts();

    // Фильтрация товаров по категории
    const filteredProducts = computed(() => {
      return props.selectedCategory === "Все"
        ? products.value
        : products.value.filter((product) => product.category === props.selectedCategory);
    });

    // Добавление/удаление из избранного
    const toggleFavorite = (product) => {
      const index = favorites.value.findIndex((item) => item.id === product.id);
      if (index === -1) {
        favorites.value.push(product);
      } else {
        favorites.value.splice(index, 1);
      }
    };

    // Проверка, в избранном ли товар
    const isFavorite = (id) => favorites.value.some((item) => item.id === id);

    // Проверка, есть ли товар в корзине
    const isInCart = (id) => cart.value.some((item) => item.id === id);

    // Действие по кнопке (добавить или перейти в корзину)
    const handleCartAction = (product) => {
      if (isInCart(product.id)) {
        router.push("/cart");
      } else {
        addToCart(product);
      }
    };

    // Добавление товара в корзину
    const addToCart = (product) => {
      const existingProduct = cart.value.find((item) => item.id === product.id);
      if (existingProduct) {
        existingProduct.quantity++;
      } else {
        cart.value.push({ ...product, quantity: 1 });
      }
    };

    return {
      filteredProducts,
      toggleFavorite,
      isFavorite,
      isInCart,
      handleCartAction,
    };
  },
};
</script>
