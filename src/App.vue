<script setup>
import { ref, onMounted } from "vue";
import VirtualList from "@/components/VirtualList.vue";

const data = ref();
onMounted(() => {
	data.value = Array.from({ length: 1000 }).map((_, i) => {
		return {
			id: i,
			value: i,
		};
	});
});
</script>

<template>
	<VirtualList :list-Data="data" :itemSize="100" />
</template>

<style>
@import "@/assets/base.css";
html,
body,
#app {
	height: 100%;
	margin: 0;
	padding: 0;
}
</style>
