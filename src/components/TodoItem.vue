<script setup lang="ts">
import { type ITodo } from './types';

const { todo } = defineProps<{ todo: ITodo }>();
</script>

<template>
	<div>
		<input type="checkbox" v-model="todo.completed" />
		<h3>{{ todo.name }}</h3>
		<p>{{ todo.description }}</p>
		<p :class="todo.overdue ? 'text-red-600' : 'text-black'">
			{{ todo.deadline }}
		</p>
	</div>
</template>
