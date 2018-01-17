<template>
	<dragDropTemplate title="h1" @action="evt => $emit('action', evt)">
		<h1>I'm a header</h1>
	</dragDropTemplate>
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
		},
	}
</script>
