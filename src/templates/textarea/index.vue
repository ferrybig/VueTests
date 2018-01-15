<template>
	<dragDropTemplate title="textarea" @action="evt => $emit('action', evt)">
		<textarea :value="value" @input="$emit('input', $event.target.value)"></textarea>
	</dragDropTemplate>
</template>

<script>
	import dragDropTemplate from "../index";

	export default {
		components: {
			dragDropTemplate,
		},
		props: {
			value: {
				type: String,
			}
		}
	}
</script>

