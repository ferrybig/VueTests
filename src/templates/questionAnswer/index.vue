<template>
	<drag-drop-template title="textarea" @action="evt => $emit('action', evt)">
		<div>
			Question:
			<input :value="question" @input="changed($event.target.value, answer)"/>
		</div>
		<div>
			Answer:
			<textarea :value="answer" @input="changed(question, $event.target.value)"/>
		</div>
	</drag-drop-template>
</template>

<script>
	import dragDropTemplate from "../base/index";

	export default {
		components: {
			dragDropTemplate,
		},
		data() {
			return {
				question: '',
				answer: '',
			}
		},
		created() {
			this.question = this.value.question;
			this.answer = this.value.answer;
		},
		watch: {
			value() {
				this.question = this.value.question;
				this.answer = this.value.answer;
			}
		},
		methods: {
			changed(question, answer) {
				this.question = question;
				this.answer = answer;
				this.$emit('input', {
					question,
					answer
				});
			}
		},
		props: {
			value: {
				type: Object,
				required: true,
			},
		},
	};
</script>

<style scoped>
	textarea, input {
		display: block;
		border: 1px solid black;
		margin: 3px;
		width: calc(100% - 8px);
	}
</style>

