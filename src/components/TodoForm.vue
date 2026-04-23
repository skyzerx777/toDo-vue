<script setup lang="ts">
import { ref } from 'vue';

const { todoArray } = defineProps(['todoArray']);
const taskNameInput = ref('');
const taskDescriptionInput = ref('');

const todayDateString = new Date().toISOString().slice(0, 10);
const taskDeadline = ref(todayDateString);

function addTask() {
	todoArray.push({
		id: Date.now(),
		name: taskNameInput.value,
		description: taskDescriptionInput.value,
		deadline: taskDeadline.value,
		completed: false,
		overdue: new Date(taskDeadline.value).getDate() < new Date().getDate(),
	});
	taskNameInput.value = '';
	taskDescriptionInput.value = '';
	taskDeadline.value = todayDateString;

	localStorage.setItem('todos', JSON.stringify(todoArray));
}
</script>

<template>
	<form
		class="bg-white rounded-xl shadow-sm p-4 flex flex-col gap-4"
		@submit.prevent="addTask"
	>
		<input
			type="text"
			v-model="taskNameInput"
			placeholder="Task name..."
			required
			class="w-full border border-gray-200 rounded px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-purple-500"
		/>
		<textarea
			v-model="taskDescriptionInput"
			placeholder="Task description..."
			rows="5"
			class="w-full border border-gray-200 rounded px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-purple-500"
		></textarea>
		<div
			class="flex flex-col sm:flex-row sm:justify-between sm:items-center gap-3"
		>
			<div class="flex items-center gap-2">
				<span class="text-gray-900 text-sm">Deadline date:</span>
				<input
					type="date"
					v-model="taskDeadline"
					class="w-full sm:w-auto border border-gray-200 rounded px-2 py-1 text-sm text-gray-900 focus:outline-none focus:ring-2 focus:ring-purple-500"
				/>
			</div>
			<div class="flex flex-col sm:flex-row gap-2 sm:items-center">
				<button
					:disabled="taskNameInput.length === 0"
					class="w-full sm:w-auto px-3 py-1.5 text-sm rounded-md text-white bg-purple-600 hover:bg-purple-700 disabled:bg-gray-300 transition"
				>
					Add task
				</button>
				<button
					type="reset"
					@click.prevent="
						taskNameInput = '';
						taskDescriptionInput = '';
						taskDeadline = todayDateString;
					"
					class="w-full sm:w-auto px-3 py-1.5 text-sm rounded-md border border-gray-200 text-gray-500 hover:bg-gray-100 transition"
				>
					Reset
				</button>
			</div>
		</div>
	</form>
</template>
