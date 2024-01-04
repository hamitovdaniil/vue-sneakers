<script setup>
import DrawerHead from "./DrawerHead.vue";
import CartListItem from "./CartListItem.vue";
</script>

<template>
	<div class="fixed top-0 left-0 w-full h-full bg-black z-10 opacity-70"></div>
	<div class="bg-white flex flex-col w-96 h-full fixed top-0 right-0 z-20 p-8">
		<DrawerHead />
		<CartListItem />
		<div class="flex flex-col gap-4 mt-7 ">
			<div class="flex g-2">
				<span>Итого:</span>
				<div class="border-b flex-1 border-dashed"></div>
				<b>12900 сум</b>
			</div>
			<div class="flex g-2">
				<span>Налог 5%:</span>
				<div class="border-b flex-1 border-dashed"></div>
				<b>1290 сум</b>
			</div>
            <button
                disabled
                class="bg-lime-500 mt-4 w-full rounded-xl py-3 text-white hover:bg-lime-600 transition active:bg-lime-700 disabled:bg-slate-300 cursor-pointer disabled:cursor-not-allowed"
            >
                Оформить заказ
            </button>
		</div>
	</div>
</template>
