<script setup lang="ts">
import { ref } from 'vue';
import Footer from './components/Footer.vue';
import Header from './components/Header.vue';
import TodoForm from './components/TodoForm.vue';
import TodoList from './components/TodoList.vue';
import { type ITodo } from './components/types';
import '/src/App.css';

const todoArray = ref<ITodo[]>([]);
</script>

<template>
	<Header />
	<main>
		<TodoForm :todoArray />
		<TodoList :todoArray />
	</main>
	<Footer />
</template>
