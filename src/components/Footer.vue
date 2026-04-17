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
	<footer class="bg-[#1f2937] py-3 px-4 w-full fixed bottom-0">
		<p class="text-[14px] text-[#e5e7eb] text-center">
			{{ currentDateFormatted }}
		</p>
	</footer>
</template>
