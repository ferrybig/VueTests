<template>
	<div>
		<draggable
				v-model="this.pageComponents"
				class="dragArea"
				:options="{forceFallback: true, group:{ name:'people',  pull:'clone', put:false }, }"
		>
			<div
					v-for="(element, index) in this.pageComponents"
					:key="index"
					class="component"
			>{{element.name}}</div>
		</draggable>
	</div>
</template>

<script>
	import draggable from 'vuedraggable';
	import testComponent from "../../test/index";
	import h1Component from "../../h1/index";
	import textAreaComponent from "../../textarea/index";

	export default {
		components: {
			draggable,
			testComponent,
			textAreaComponent,
			h1Component,
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
