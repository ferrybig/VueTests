<template>
	<drag-drop-template title="textarea" @action="evt => $emit('action', evt)">
		<textarea :value="value" @input="$emit('input', $event.target.value)"></textarea>
	</drag-drop-template>
</template>

<script>
	import dragDropTemplate from "../base/index";

	export default {
		components: {
			dragDropTemplate,
		},
		props: {
			value: {
				type: String,
				required: true,
			},
		}
	}
</script>

