<script setup>
	import { ref, defineEmits } from 'vue';

	let searchValue = ref("");

	let emits = defineEmits(['searchPressed'])

</script>

<template>
	<input type="text" name="search" id="search" v-model="searchValue" placeholder="Student name">
	<button class="search_button" id="search_button" @click="$emit('searchPressed', searchValue)">Search</button>
</template>

<style scoped>
	input,button {
		padding: 5px;
		margin: 5px;
		border-radius: 10px;
	}

	button {
		background-color: rgb(196, 167, 205);
	}
</style>