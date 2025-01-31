<template>
  <v-container class="h-100">

    <!-- Фильтр по категориям -->
    <FiltersBar v-model="selectedCategory" />

    <!-- Список товаров -->
    <v-row>
      <v-col cols="12">
        <ProductList :selectedCategory="selectedCategory" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { ref } from "vue";
import FiltersBar from "@/components/FiltersBar.vue";
import ProductList from "@/components/ProductList.vue";

export default {
  components: { FiltersBar, ProductList },
  setup() {
    const selectedCategory = ref("Все"); // Выбранная категория

    return { selectedCategory };
  },
};
</script>
