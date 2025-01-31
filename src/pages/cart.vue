<template>
  <v-container>
    <v-row justify="center">
      <!-- Если корзина пустая -->
      <v-col v-if="cartItems.length === 0" cols="12" class="text-center">
        <v-alert type="info" variant="outlined">Корзина пуста</v-alert>
      </v-col>

      <!-- Список товаров в корзине -->
      <v-col v-for="(item, index) in cartItems" :key="item.id" cols="12">
        <v-card class="d-flex align-center pa-3">
          <!-- Изображение -->
          <v-img :src="item.image" max-height="150" max-width="150" contain class="rounded-lg"></v-img>

          <!-- Информация о товаре -->
          <div class="ms-4">
            <v-card-title class="text-h6">{{ item.name }}</v-card-title>
            <v-card-subtitle>{{ item.price }} Tenge</v-card-subtitle>
          </div>

          <v-spacer></v-spacer>

          <!-- Количество товара -->
          <div class="d-flex align-center">
            <v-btn variant="text" icon @click="changeQuantity(index, -1)">
              <v-icon>mdi-minus</v-icon>
            </v-btn>
            <div class="mx-2 text-h6">{{ item.quantity }}</div>
            <v-btn variant="text" icon @click="changeQuantity(index, 1)">
              <v-icon>mdi-plus</v-icon>
            </v-btn>
          </div>

          <v-spacer></v-spacer>

          <!-- Кнопка удаления -->
          <v-btn color="red" variant="text" icon @click="removeItem(index)">
            <v-icon>mdi-trash-can-outline</v-icon>
          </v-btn>
        </v-card>
      </v-col>

      <!-- Разделитель -->
      <v-col cols="12">
        <v-divider></v-divider>
      </v-col>

      <!-- Итог и кнопки -->
      <v-row class="mt-4" justify="space-between">
        <v-col cols="6">
          <v-btn color="red" variant="outlined" @click="clearCart" v-if="cartItems.length > 0">
            <v-icon start>mdi-cart-remove</v-icon> Очистить корзину
          </v-btn>
        </v-col>
        <v-col cols="6" class="text-right">
          <v-btn color="blue" variant="flat" @click="checkout">
            <v-icon start>mdi-credit-card</v-icon> Оформить заказ ({{ totalPrice }} Tenge)
          </v-btn>
        </v-col>
      </v-row>
    </v-row>
  </v-container>
</template>

<script>
import { ref, computed, onMounted } from "vue";

export default {
  setup() {
    const cartItems = ref([]);

    // Загрузка корзины из localStorage
    const loadCart = () => {
      cartItems.value = JSON.parse(localStorage.getItem("cart")) || [];
    };

    // Подсчет общей стоимости
    const totalPrice = computed(() => {
      return cartItems.value.reduce((sum, item) => sum + item.price * item.quantity, 0);
    });

    // Изменение количества товара
    const changeQuantity = (index, delta) => {
      if (cartItems.value[index].quantity + delta > 0) {
        cartItems.value[index].quantity += delta;
        saveCart();
      }
    };

    // Удаление товара
    const removeItem = (index) => {
      cartItems.value.splice(index, 1);
      saveCart();
    };

    // Очистка корзины
    const clearCart = () => {
      cartItems.value = [];
      saveCart();
    };

    // Сохранение корзины в localStorage
    const saveCart = () => {
      localStorage.setItem("cart", JSON.stringify(cartItems.value));
    };

    // Оформление заказа
    const checkout = () => {
      console.log("Оформление заказа:", cartItems.value);
      alert("Заказ оформлен!");
      clearCart();
    };

    onMounted(() => {
      loadCart();
    });

    return {
      cartItems,
      totalPrice,
      changeQuantity,
      removeItem,
      clearCart,
      checkout,
    };
  },
};
</script>
