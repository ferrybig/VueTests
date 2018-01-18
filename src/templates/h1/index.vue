<template>
	<drag-drop-template title="h1" @action="evt => $emit('action', evt)">
		<h1>I'm a header</h1>
	</drag-drop-template>
</template>

<script>
	import dragDropTemplate from "../base/index";

	export default {
		components: {
			dragDropTemplate,
		},
	}
</script>
