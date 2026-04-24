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
	<div class="max-w-2xl mx-auto mt-6">
		<div
			class="flex justify-end items-center gap-6 mb-4 text-sm text-gray-600 me-2 sm:me-0"
		>
			<label class="flex items-center select-none">
				<input
					type="checkbox"
					v-model="hideExpired"
					class="mx-2 accent-purple-700 size-4"
				/>
				Hide expired
			</label>
			<select
				v-model="status"
				name="todosStatusSelector"
				class="border border-gray-200 rounded px-2 py-1 text-sm text-gray-700 bg-white focus:outline-none focus:ring-2 focus:ring-purple-500 cursor-pointer"
			>
				<option v-for="status in statuses" :key="status" class="text-sm">
					{{ status }}
				</option>
			</select>
		</div>
		<ul class="bg-white rounded-xl shadow-sm divide-y divide-gray-200">
			<TodoItem
				v-for="todo in reversedTodoArray"
				:key="todo.id"
				:todo
				@removeTodo="removeTodo"
				:todoArray
			/>
		</ul>
	</div>
</template>
