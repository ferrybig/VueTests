<template>
	<div id="main">
		<h1>Vue Draggable</h1>
		<div class="drag">
			<div class="main">
				<h2>
					page
				</h2>
				<div class="list">
					<template v-for="(element, index) in this.pageModel" >
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
			</div>
			<div class="sidebar">
				<h2>
					sidebar
				</h2>
				<draggable
						v-model="this.pageComponents"
						class="dragArea"
						:options="{forceFallback: true, group:{ name:'people',  pull:'clone', put:false }, }"
				>
					<div v-for="(element, index) in this.pageComponents" :key="index" class="component">{{element.name}}</div>
				</draggable>
			</div>
		</div>
	</div>
</template>

<script>
import draggable from 'vuedraggable';
import testComponent from "./templates/test/index";
import h1Component from "./templates/h1/index";
import textAreaComponent from "./templates/textarea/index";
import Vue from "vue";

export default {
	components: {
		draggable,
		testComponent,
		textAreaComponent,
		h1Component
	},
	data () {
		return {
			showDragTargets: false,
			pageComponents: [
				{
					is: "testComponent",
					name: "test"
				}, {
					is: "h1Component",
					name: "h1",
				}, {
					is: "textAreaComponent",
					name: "textArea",
					value: 'default text',
					allowDupes: true,
				}
			],
			page: [
				{
					is: "testComponent",
					name: "test"
				}
			],
			lastId: 0,
		}
	},
	computed: {
		pageModel() {
			const arr = [];
			for(let i = 0; i <= this.page.length; i++) {
				arr.push({
					type: 'draggable',
					index: i,
					target: [],
				});
				if(i != this.page.length) {
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
		onAction (index, evt) {
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

		moveUp (index) {
			if (index !== 0) {
				let lastObj = this.page[index];
				Vue.set(this.page, index, this.page[index - 1]);
				Vue.set(this.page, index - 1, lastObj);
			} else {
				console.log('nope');
			}
		},

		moveDown (index) {
			if (index !== this.page.length - 1) {
				let lastObj = this.page[index];
				Vue.set(this.page, index, this.page[index + 1]);
				Vue.set(this.page, index + 1, lastObj);
			} else {
				console.log('nope');
			}
		},

		delete (index) {
			this.page.splice(index, 1);
		},

		onAdd(index){
			const element = this.pageModel[index].target[0];
			this.pageModel[index].target.pop();
			let found = false;
			if (!element.allowDupes) {
				for(let i = 0; i < this.page.length; i++) {
					if(this.page[i].name === element.name) {
						found = true
					}
				}
			}
			//Clone element
			const newElement = JSON.parse(JSON.stringify(element));
			newElement.id = this.lastId++;
			if(!found)
				this.page.splice(index / 2, 0, newElement);
		}
	}
}
</script>

<style lang="scss">
	.drag {
		display: flex;
	}
	.main {
		width: 66%;
	}
	.sidebar {
		width: 33%;
	}

	.dragArea {
		min-height: 30px;
	}
	.pagepart {
		border: 1px solid black;
	}
	.dragTarget {
		display: flex;
		position: relative;
		justify-content: center;
		align-content: center;
	}
	.dragTarget::after {
		position: absolute;
		content: '';
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 10px;
		height: 0;
		border: 2px solid black;
	}
	.dragTarget::before {
		position: absolute;
		content: '';
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 0;
		height: 10px;
		border: 2px solid black;
	}
	.component {
		background: white;
		display: inline-block;
		border: 1px solid black;
		padding: 3px;
		position: relative;
		z-index: 1
	}

</style>
