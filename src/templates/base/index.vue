<template>
	<div class="pagepart">
		<h3>{{title}}</h3>
		<div class="handle">Drag me</div>
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
			},
		},
	};
</script>

<style>
	.pagepart {
		border: 1px solid black;
		padding: 3px;
		background-color: white;
	}
	/* Class name for the dragging item */
	.pagepart--drag {

	}
	/* Class name for the drop placeholder */
	.pagepart--ghost {
		border-color: blue;
		outline: 2px solid lightblue;
	}
	/* Class name for the drop placeholder */
	.pagepart--chosen {

	}
	.handle {
		border: 1px solid black;
		padding: 3px;
		display: inline-block;
	}
</style>
