<template>
	<div>
		<draggable
			v-model="pageComponents"
			class="dragArea"
			:options="options"
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
			/**
			 * pageComponents: is the value received by v-model, and will
			 * contain our components
			 */
			pageComponents: {
				type: Array,
				required: true,
				validator: function (value) {
					for(let i = 0; i < value.length; i++) {
						if(typeof value[i] !== 'object') {
							return false;
						}
						if(!value[i].is) {
							return false;
						}
						if(!value[i].name) {
							return false;
						}
					}
					return true;
				},
			},
			/**
			 * Group is used when there are multiple ragables on the same page,
			 * when the name is differend, they cannot communicate
			 */
			group: {
				type: String,
				default: 'components',
			},
		},
		computed: {
			options() {
				return {
					forceFallback: true,
					group: {
						name: this.group,
						pull: 'clone',
						put: false,
						revertClone: true,
					},
					ghostClass: 'component--ghost',
					chosenClass: 'component--chosen',
					dragClass: 'component--drag',
				};
			},
		},
	};
</script>

<style>

</style>
