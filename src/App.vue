<script setup>
import { onMounted, provide, reactive, ref, watch } from "vue";
import axios, { Axios } from "axios";
import Header from "./components/Header.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";

const items = ref([]);

const filters = reactive({
	sortBy: "name",
	searchQuery: "",
});

const onChangeSelect = (event) => {
	filters.sortBy = event.target.value;
};
const onChangeSearch = (event) => {
	filters.searchQuery = event.target.value;
};

const fetchFav = async () => {
	try {
		const { data: favourites } = await axios.get("https://1c9b339bc80a6e5d.mokky.dev/fav");
		items.value = items.value.map((item) => {
			const favourite = favourites.find((favourite) => favourite.parentId === item.id);
			if (!favourite) {
				return item;
			}
			return {
				...item,
				isFav: true,
				favouriteId: favourite.id,
			};
		});

		console.log(items.value);
	} catch (error) {
		console.log(error);
	}
};

const addToFav = async (item) => {
	item.isFav = !item.isFav;
	console.log(item);
};

const fetchItems = async () => {
	try {
		const params = {
			sortBy: filters.sortBy,
		};
		if (filters.searchQuery) {
			params.name = `*${filters.searchQuery}*`;
		}
		const { data } = await axios.get("https://1c9b339bc80a6e5d.mokky.dev/sneakers", {
			params,
		});
		items.value = data.map((obj) => ({
			...obj,
			isFav: false,
			isAdded: false,
		}));
	} catch (error) {
		console.log(error);
	}
};

onMounted(async () => {
	await fetchItems();
	await fetchFav();
});

watch(filters, fetchFav);

provide("addToFav", addToFav);
</script>

<template>
	<!-- <Drawer/> -->
	<div class="w-4/5 bg-white m-auto rounded-xl shadow-xl mt-14">
		<Header />
		<div class="p-10">
			<div class="flex justify-between items-center">
				<h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
				<div class="flex items-center gap-4">
					<select
						@change="onChangeSelect"
						class="py-2 px-3 border rounded-md outline-none"
					>
						<option value="name">По названию</option>
						<option value="price">По цене (дешевые)</option>
						<option value="-price">По цене (дорогие)</option>
					</select>
					<div class="relative">
						<img src="/search.svg" alt="search" class="absolute left-4 top-3" />
						<input
							@input="onChangeSearch"
							type="text"
							placeholder="Поиск..."
							class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
						/>
					</div>
				</div>
			</div>
			<div class="mt-10">
				<CardList :items="items" />
			</div>
		</div>
	</div>
</template>

<style scoped></style>
