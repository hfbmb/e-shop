<template>
	<v-app-bar class="px-3">
		<v-spacer />

		<v-btn
			:to="'/'"
			text="Home"
			slim
			class="ms-3"
		/>

		<v-btn
			:to="'favorites'"
			variant="text"
  		class="d-flex align-center ms-2"
		>
			<v-icon color="black" size="24">mdi-cards-heart-outline</v-icon>
			<span class="text-subtitle-1 ms-1 font-weight-medium">0</span>
		</v-btn>

    <v-btn
     	:to="'cart'"
     	variant="text"
  		class="d-flex align-center ms-2"
    >
    	<v-icon color="black" size="24">mdi-cart</v-icon>
			<span class="text-subtitle-1 ms-1 font-weight-medium">0</span>
		</v-btn>

		<v-btn
			:to="'table'"
			text="Table"
			slim
			class="ms-3"
		/>
	</v-app-bar>
</template>

<script setup>
//
</script>
