<template>
	<div class="list">
		<template v-for="(element, index) in pageModel" >
			<template v-if="element.type === 'draggable'">
				<draggable
						v-model="element.target"
						class="dragArea dragTarget"
						:options="{forceFallback: true, group:'people'}"
						@add="evt => onAdd(index, evt)"
				>
					<div
							v-for="(element, index) in element.target"
							:key="index"
							class="component"
					>
						{{element.name}}
					</div>
				</draggable>
			</template>
			<component
					v-if="element.type === 'component'"
					:key="element.id"
					v-bind="element.data"
					@action="evt => onAction(element.index, evt)"
					@input="evt => element.data.value = evt"
			/>
		</template>
	</div>
</template>

<script>
	import draggable from 'vuedraggable';
	import testComponent from "../../test/index";
	import h1Component from "../../h1/index";
	import textAreaComponent from "../../textarea/index";
	import questionAnswerComponent from '../../questionAnswer/index';
	import Vue from "vue";

	export default {
		components: {
			draggable,
			testComponent,
			textAreaComponent,
			h1Component,
			questionAnswerComponent,
		},
		model: {
			prop: 'value',
			event: 'input'
		},
		props: {
			value: {
				type: Array,
				required: true,
			}
		},
		created() {
			this.page = this.value;
		},
		watch: {
			value(newValue) {
				this.page = newValue;
			},
		},
		data() {
			return {
				page: [],
				lastId: 0,
			};
		},
		computed: {
			pageModel() {
				const arr = [];
				for (let i = 0; i <= this.page.length; i++) {
					arr.push({
						type: 'draggable',
						index: i,
						target: [],
					});
					if (i != this.page.length) {
						arr.push({
							type: 'component',
							index: i,
							data: this.page[i],
						});
					}
				}
				return arr;
			}
		},
		methods: {
			onAction(index, evt) {
				switch (evt) {
					case 'moveUp':
						this.moveUp(index);
						break;
					case 'moveDown':
						this.moveDown(index);
						break;
					case 'delete':
						this.delete(index);
						break;
					default:
						console.log('I don\'t know how but you messed it up anyways')
						break;
				}
			},

			moveUp(index) {
				if (index !== 0) {
					let lastObj = this.page[index];
					Vue.set(this.page, index, this.page[index - 1]);
					Vue.set(this.page, index - 1, lastObj);
					this.$emit('input', this.page)
				} else {
					console.log('nope');
				}
			},

			moveDown(index) {
				if (index !== this.page.length - 1) {
					let lastObj = this.page[index];
					Vue.set(this.page, index, this.page[index + 1]);
					Vue.set(this.page, index + 1, lastObj);
					this.$emit('input', this.page)
				} else {
					console.log('nope');
				}
			},

			delete(index) {
				this.page.splice(index, 1);
				this.$emit('input', this.page)
			},

			onAdd(index, evt) {
				console.log('add', index, evt);
				const element = this.pageModel[index].target[0];
				this.pageModel[index].target.pop();
				let found = false;
				if (!element.allowDupes) {
					for (let i = 0; i < this.page.length; i++) {
						if (this.page[i].name === element.name) {
							found = true
						}
					}
				}
				//Clone element
				const newElement = JSON.parse(JSON.stringify(element));
				newElement.id = this.lastId++;
				if (!found) {
					this.page.splice(index / 2, 0, newElement);
					this.$emit('input', this.page)
				}
			}
		}
	}
</script>

<style>

</style>
