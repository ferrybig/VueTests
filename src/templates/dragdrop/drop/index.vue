<template>
	<draggable
		class="list"
		v-model="pageModel"
		:options="options"
	>
		<template v-for="(element, index) in pageModel" >
			<component
				:key="element.id"
				:is="element.is"
				:value="element.value"
				@action="evt => onAction(index, evt)"
				@input="evt => element.value = evt"
			/>
		</template>
	</draggable>
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
			/**
			 * Value is the value received by v-model, and will contain our
			 * page data
			 */
			value: {
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
			/**
			 * Page model is a copy of page, so we can receive set events on it,
			 * and rewrite its internal variables if needed
			 */
			pageModel:{
				get() {
					const arr = [];
					for (let i = 0; i < this.page.length; i++) {
						if(!this.page[i].id) {
							this.page[i].id = this.newId();
						}
						arr.push(this.page[i]);
					}
					return arr;
				},
				set(newValue) {
					for(let i = 0; i < newValue.length; i++) {
						if(!newValue[i].id) {
							// Copy the new value, so we don't edit the component itself
							newValue[i] = {...newValue[i]};
							newValue[i].id = this.newId();
						}
					}
					this.$emit('input', newValue);
				},
			},
			options() {
				return {
					forceFallback: true,
					group: this.group,
					handle: '.handle',
					animation: 300,
					ghostClass: 'pagepart--ghost',
					chosenClass: 'pagepart--chosen',
					dragClass: 'pagepart--drag',
				};
			},
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

			newId() {
				return ++this.lastId;
			}
		}
	}
</script>

<style>
	.list {
		min-height: 100px;
	}
</style>
