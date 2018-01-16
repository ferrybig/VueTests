<template>
	<dragDropTemplate title="test" @action="evt => $emit('action', evt)">
		This is the test page!!
	</dragDropTemplate>
</template>

<script>
	import dragDropTemplate from "../base/index";

	export default {
		components: {
			dragDropTemplate,
		},
	}
</script>
