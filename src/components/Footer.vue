<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from 'vue';

const currentDate = ref(new Date());
let timeoutId: number;

function tick() {
	currentDate.value = new Date();

	timeoutId = window.setTimeout(tick, 1000);
}

onMounted(() => tick());

onUnmounted(() => clearTimeout(timeoutId));

const currentDateFormatted = computed(() => {
	return currentDate.value.toLocaleString([], {
		hour: '2-digit',
		minute: '2-digit',
		second: '2-digit',
		day: 'numeric',
		month: 'numeric',
		year: 'numeric',
	});
});
</script>

<template>
	<footer
		class="fixed bottom-4 left-1/2 -translate-x-1/2 bg-white border border-gray-200 shadow-sm rounded-full px-4 py-1"
	>
		<p class="text-xs text-gray-500 font-medium">
			{{ currentDateFormatted }}
		</p>
	</footer>
</template>
