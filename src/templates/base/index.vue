<template>
	<div class="pagepart">
		<h3>{{title}}</h3>
		<button @click="$emit('action', 'moveUp')">^</button>
		<button @click="$emit('action', 'moveDown')">V</button>
		<button @click="$emit('action', 'delete')">X</button>
		<div>
			<slot/>
		</div>
	</div>
</template>

<script>
	export default {
		props: {
			title: {
				type: String,
			}
		},
	};
</script>

<style>
	.pagepart {
		border: 1px solid black;
	}
</style>
