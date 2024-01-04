<script setup>
import { inject } from "vue";
import Card from "@/components/Card.vue";
defineProps({
	items: Array,
});
const addToFav = inject("addToFav");

const onClickAdd = () => {
	console.log("cvasasvas");
};
</script>

<template>
	<div class="grid grid-cols-4 gap-5">
		<Card
			v-for="item in items"
			:id="item.id"
			:key="item.id"
			:price="item.price"
			:title="item.name"
			:imageUrl="item.img"
			@onClickAdd="onClickAdd"
			@ClickFavorite="()=> addToFav(item)"
			:isFavorite="item.isFav"
		/>
	</div>
</template>
