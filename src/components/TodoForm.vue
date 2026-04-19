<script setup lang="ts">
import { ref, watchEffect } from 'vue';

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
}

watchEffect(() => console.log(taskDeadline.value));
</script>

<template>
	<form class="flex flex-col items-center gap-4" @submit.prevent="addTask">
		<input
			type="text"
			v-model="taskNameInput"
			placeholder="Enter your task"
			required
		/>
		<textarea
			v-model="taskDescriptionInput"
			placeholder="Task description..."
		></textarea>
		<label>Deadline: <input type="date" v-model="taskDeadline" /></label>
		<button
			:disabled="taskNameInput.length === 0"
			:style="[taskNameInput.length === 0 ? 'color:gray' : 'color:black']"
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
		>
			Clean fields
		</button>
	</form>
</template>
