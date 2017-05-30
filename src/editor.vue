<template>
	<div ref="editor"></div>
</template>

<script>
	import Quill from 'quill';
	import 'quill/dist/quill.snow.css';

	export default {
		watch: {
			value: 'valueUpdated'
		},

		props: {
			value: {
				default: ''
			},

			toolbar: {
				default() {
					return [
						[{ 'header': [1, 2, false] }],
						['bold', 'italic', 'underline', 'strike'],
						[{ 'color': [] }, { 'background': [] }],
						[{ 'list': 'ordered'}, { 'list': 'bullet' }],
						[{ 'align': [] }],
						['link', 'image', 'video'],
						['clean']
					]
				}
			},

			placeholder: {
				default: ''
			}
		},

		watch: {
			value(newValue, oldValue) {
				let editorValue = this.quill.root.innerHTML;

				if (editorValue !== this.value) {
					this.quill.pasteHTML(this.value);
				}
			}
		},

		mounted() {
			this.setup();
		},

		beforeDestroy() {
			this.quill.off('text-change', this.emitInput);
		},

		methods: {
			setup() {
				this.quill = new Quill(this.$refs.editor, {
					theme: 'snow',
					placeholder: this.placeholder,
					modules: {
						toolbar: this.toolbar
					}
				});

				this.quill.pasteHTML(this.value);

				this.quill.on('text-change', this.emitInput);
			},

			emitInput() {
				let text = this.quill.getText().trim();
				let value = this.quill.root.innerHTML;

				if (text === '') {
					value = '';
				}

				this.$emit('input', value);
			},

			valueUpdated() {
				if (this.quill) {
					this.quill.pasteHTML(this.value);
				}
			}
		}
	}
</script>

<style scoped>
	.ql-container {
		font-family: inherit;
	}
</style>
