<script setup lang="ts">
import { ref, watchEffect } from 'vue';
import { type ITodo } from './types';

const { todo, index } = defineProps<{ todo: ITodo; index: number }>();
defineEmits(['removeTodo']);

const isEditing = ref(false);
function toggleEditMode() {
	isEditing.value = !isEditing.value;
}

watchEffect(() => {
	todo.overdue = new Date(todo.deadline).getDate() < new Date().getDate();
});
</script>

<template>
	<div>
		<input type="checkbox" v-model="todo.completed" />
		<h3
			:class="[todo.completed ? 'line-through text-gray-500' : '']"
			:hidden="isEditing"
		>
			{{ todo.name }}
		</h3>
		<input type="text" :hidden="!isEditing" v-model="todo.name" />
		<p
			:class="todo.completed ? 'line-through text-gray-500' : ''"
			:hidden="isEditing"
		>
			{{ todo.description }}
		</p>
		<input type="text" :hidden="!isEditing" v-model="todo.description" />
		<p
			:hidden="isEditing"
			:class="[
				todo.overdue ? 'text-red-600' : 'text-black',
				todo.completed ? 'line-through text-gray-500' : '',
			]"
		>
			{{ todo.deadline }}
		</p>
		<input type="date" :hidden="!isEditing" v-model="todo.deadline" />
		<button class="cursor-pointer size-8" @click="toggleEditMode">Edit</button>
		<button class="cursor-pointer size-4" @click="$emit('removeTodo', index)">
			Remove
		</button>
	</div>
</template>
