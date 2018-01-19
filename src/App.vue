<template>
	<div>
		<div style="display: flex;">
			<div style="width: 66%;" >
				<h2>
					page
				</h2>
				<drop v-model="page"/>
			</div>
			<div style="width: 33%; align-self: flex-start; position: sticky; top: 0">
				<h2>
					sidebar
				</h2>
				<drag v-model="pageComponents"/>
			</div>
		</div>
		<pre>{{ page }}</pre>
	</div>
</template>

<script>
import drag from "./templates/dragdrop/drag/index";
import drop from "./templates/dragdrop/drop/index";

export default {
	components: {
		drag,
		drop
	},
	data() {
		return {
			pageComponents: [
				{
					is: "test-component",
					name: "test",
					value: '',
					allowDupes: false,
				}, {
					is: "h1-component",
					name: "h1",
					value: '',
					allowDupes: false,
				}, {
					is: "textArea-component",
					name: "textarea",
					value: 'Default text',
					allowDupes: true,
				}, {
					is: 'question-answer-component',
					name: 'Q&A',
					allowDupes: true,
					value: {
						question: 'default question',
						answer: 'default answer',
					}
				}
			],

			page: [],
		};
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
