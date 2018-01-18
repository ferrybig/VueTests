<template>
	<div>
		<draggable
				v-model="pageComponents"
				class="dragArea"
				:options="{forceFallback: true, group:{ name:'people',  pull:'clone', put:false }, }"
		>
			<div
					v-for="(element, index) in pageComponents"
					:key="index"
					class="component"
			>{{element.name}}</div>
		</draggable>
	</div>
</template>

<script>
	import draggable from 'vuedraggable';

	export default {
		components: {
			draggable,
		},
		model: {
			prop: 'pageComponents',
			event: 'input'
		},
		props: {
			pageComponents: {
				type: Array,
				required: true
			}
		},
	}
</script>

<style>

</style>
