<script setup lang="ts">
import { onMounted, ref } from 'vue';
import Footer from './components/Footer.vue';
import Header from './components/Header.vue';
import TodoForm from './components/TodoForm.vue';
import TodoList from './components/TodoList.vue';
import type { ITodo } from './components/types';
import '/src/App.css';

const todoArray = ref<ITodo[]>([]);

onMounted(async () => {
	const raw = await localStorage.getItem('todos');

	if (!raw) return;

	todoArray.value = await JSON.parse(raw);
});
</script>

<template>
	<Header />
	<main class="max-w-4xl m-auto pb-14">
		<TodoForm :todoArray />
		<TodoList :todoArray />
	</main>
	<Footer />
</template>
