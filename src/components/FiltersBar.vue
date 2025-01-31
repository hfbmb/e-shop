<template>
  <v-container class="py-2">
    <v-row justify="center">
      <v-col v-for="category in categories" :key="category" cols="auto">
        <v-btn
          :color="selectedCategory === category ? 'blue' : 'grey'"
          @click="updateCategory(category)"
          depressed
        >
          {{ category }}
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { ref } from "vue";

export default {
  props: {
    modelValue: String, // Используем v-model
  },
  emits: ["update:modelValue"], // Позволяет передавать изменения наверх
  setup(props, { emit }) {
    const categories = ["Все", "Одежда", "Обувь", "Аксессуары", "Гаджеты", "Мебель"];
    const selectedCategory = ref(props.modelValue || "Все");

    const updateCategory = (category) => {
      selectedCategory.value = category;
      emit("update:modelValue", category);
    };

    return { categories, selectedCategory, updateCategory };
  },
};
</script>
