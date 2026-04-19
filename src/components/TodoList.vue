<script setup lang="ts">
import TodoItem from './TodoItem.vue';

const { todoArray } = defineProps(['todoArray']);
</script>

<template>
	<div>
		<TodoItem v-for="todo in todoArray" :key="todo.id" :todo />
	</div>
</template>
