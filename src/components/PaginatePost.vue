<script setup>
defineProps(['inicio', 'fin', 'maxLength']);

const emit = defineEmits(['next', 'prev']);
</script>

<template>
	<div class="btn-group mb-2" role="group" aria-label="Basic example">
		<button
			type="button"
			class="btn btn-outline-primary"
			@click="emit('prev')"
			:disabled="inicio === 0"
		>
			Previus {{ inicio }}
		</button>
		<button
			type="button"
			class="btn btn-outline-primary"
			@click="emit('next')"
			:disabled="fin === maxLength"
		>
			Next {{ fin }}
		</button>
	</div>
</template>
