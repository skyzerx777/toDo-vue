<script setup lang="ts">
import { computed, ref } from 'vue';
import TodoItem from './TodoItem.vue';
import type { ITodo } from './types';

const { todoArray } = defineProps<{ todoArray: ITodo[] }>();
type StatusesType = 'All' | 'Completed' | 'Incompleted';
const statuses: StatusesType[] = ['All', 'Completed', 'Incompleted'];
const hideExpired = ref(false);
const status = ref<StatusesType>('All');

function removeTodo(id: number) {
	let indexForRemoving = 0;
	for (let todo of todoArray) {
		if (todo.id == id) {
			break;
		}
		indexForRemoving++;
	}
	todoArray.splice(indexForRemoving, 1);

	localStorage.setItem('todos', JSON.stringify(todoArray));
}

const filteredTodoArray = computed(() => {
	return filterTodoArray(todoArray);
});

function filterTodoArray(array: ITodo[]) {
	let filteredArray = [...array];

	if (hideExpired.value === true) {
		filteredArray = filteredArray.filter(item => !item.overdue);
	}
	if (status.value === 'Completed') {
		filteredArray = filteredArray.filter(item => item.completed);
	}
	if (status.value === 'Incompleted') {
		filteredArray = filteredArray.filter(item => !item.completed);
	}
	return filteredArray;
}
const reversedTodoArray = computed<ITodo[]>(() =>
	[...filteredTodoArray.value].reverse(),
);
</script>

<template>
	<div>
		<label><input type="checkbox" v-model="hideExpired" />Hide expired</label>
		<select v-model="status">
			<option v-for="status in statuses">{{ status }}</option>
		</select>
		<TodoItem
			v-for="todo in reversedTodoArray"
			:key="todo.id"
			:todo
			@removeTodo="removeTodo"
			:todoArray
		/>
	</div>
</template>
