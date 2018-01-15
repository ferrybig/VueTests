<template>
	<dragDropTemplate title="h1" @action="evt => $emit('action', evt)">
		<h1>I'm a header</h1>
	</dragDropTemplate>
</template>

<script>
	import dragDropTemplate from "../index";

	export default {
		components: {
			dragDropTemplate,
		},
	}
</script>
