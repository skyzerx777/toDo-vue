<script setup lang="ts">
import { computed, ref, watchEffect } from 'vue';
import type { ITodo } from './types';

const { todo, todoArray } = defineProps<{
	todo: ITodo;
	todoArray: ITodo[];
}>();

defineEmits(['removeTodo']);

const isEditing = ref(false);
const isExpanded = ref(false);

const isExpandable = computed(() => todo.description.length > 164);

function toggleEditMode() {
	isEditing.value = !isEditing.value;
}

function isOverdue(deadline: string) {
	const today = new Date();
	today.setHours(0, 0, 0, 0);

	const due = new Date(deadline);
	due.setHours(0, 0, 0, 0);

	console.log(today, due, due < today);
	return due < today;
}

watchEffect(() => {
	todo.overdue = isOverdue(todo.deadline);
});

watchEffect(() => {
	localStorage.setItem('todos', JSON.stringify(todoArray) as string);
});
</script>

<template>
	<li
		class="grid grid-cols-[20px_1fr] gap-4 p-4 items-center hover:bg-gray-50"
	>
		<input
			type="checkbox"
			v-model="todo.completed"
			class="mt-1 w-5 h-5 accent-purple-600 cursor-pointer"
		/>
		<div class="flex flex-col min-w-0">
			<h3
				v-show="!isEditing"
				:class="[
					'text-[15px] font-medium',
					todo.completed ? 'line-through text-gray-400' : 'text-gray-900',
				]"
			>
				{{ todo.name }}
			</h3>
			<input
				v-show="isEditing"
				v-model="todo.name"
				class="w-full border border-gray-200 rounded px-2 py-1 text-sm text-gray-900 focus:outline-none focus:ring-2 focus:ring-purple-500"
			/>
			<div
				v-if="!isEditing"
				class="text-[13px] w-full wrap-break-word transition-all duration-200"
				:class="[
					todo.completed ? 'line-through text-gray-400' : 'text-gray-500',
					isExpanded ? '' : 'line-clamp-2',
				]"
			>
				{{ todo.description }}
			</div>
			<textarea
				v-show="isEditing"
				v-model="todo.description"
				rows="4"
				class="mt-1 w-full border border-gray-200 rounded px-2 py-1 text-sm text-gray-700 focus:outline-none focus:ring-2 focus:ring-purple-500 resize-none"
				placeholder="Description..."
			/>
			<div
				class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2 mt-2"
			>
				<p
					v-show="!isEditing"
					class="text-[12px] leading-none text-gray-400 order-1 sm:order-0"
					:class="{
						'text-red-500': todo.overdue && !todo.completed,
						'line-through text-gray-400': todo.completed,
					}"
				>
					&#128197 {{ new Date(todo.deadline).toLocaleDateString() }}
				</p>
				<div
					v-show="isEditing"
					class="flex items-center gap-2 text-sm text-gray-500"
				>
					<span>&#128197</span>
					<input
						type="date"
						v-model="todo.deadline"
						class="border border-gray-200 rounded px-2 py-1 focus:outline-none focus:ring-2 focus:ring-purple-500"
					/>
				</div>
				<button
					v-if="!isEditing && isExpandable"
					@click="isExpanded = !isExpanded"
					class="text-xs text-purple-500 hover:underline leading-none sm:ml-auto"
				>
					{{ isExpanded ? 'Show less' : 'Read more' }}
				</button>
			</div>
		</div>
		<div
			class="col-span-2 flex mt-2 text-sm border border-gray-200 rounded-md overflow-hidden"
		>
			<button
				@click="toggleEditMode"
				class="w-1/2 py-1.5 text-blue-500 hover:bg-gray-50 cursor-pointer"
			>
				{{ isEditing ? 'Confirm' : 'Edit' }}
			</button>

			<div class="w-px bg-gray-200"></div>
			<button
				@click="$emit('removeTodo', todo.id)"
				class="w-1/2 py-1.5 text-red-500 hover:bg-gray-50 cursor-pointer"
			>
				Remove
			</button>
		</div>
	</li>
</template>
