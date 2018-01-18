<template>
	<drag-drop-template title="test" @action="evt => $emit('action', evt)">
		This is the test page!!
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
