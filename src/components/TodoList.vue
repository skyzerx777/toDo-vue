<script setup lang="ts">
import TodoItem from './TodoItem.vue';
import type { ITodo } from './types';

const { todoArray } = defineProps<{ todoArray: ITodo[] }>();

function removeTodo(index: number) {
	todoArray.splice(index, 1);
}
</script>

<template>
	<div>
		<TodoItem
			v-for="(todo, index) in todoArray"
			:key="todo.id"
			:todo
			@removeTodo="removeTodo"
			:index
		/>
	</div>
</template>
