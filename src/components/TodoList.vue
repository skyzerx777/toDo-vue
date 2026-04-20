<script setup lang="ts">
import { computed } from 'vue';
import TodoItem from './TodoItem.vue';
import type { ITodo } from './types';

const { todoArray } = defineProps<{ todoArray: ITodo[] }>();

function removeTodo(index: number) {
	todoArray.splice(index, 1);
}

const reversedTodoArray = computed<ITodo[]>(() => [...todoArray].reverse());
</script>

<template>
	<div>
		<TodoItem
			v-for="(todo, index) in reversedTodoArray"
			:key="todo.id"
			:todo
			@removeTodo="removeTodo"
			:index
		/>
	</div>
</template>
