<template>
	<div></div>
</template>

<script>
	import $ from 'jquery';
	import 'bootstrap';
	import 'summernote';
	import 'summernote/dist/summernote.css';

	export default {
		props: {
			lang: {
				default: 'en-US'
			},

			placeholder: {
				default: ''
			},

			fontNames: {
				default() {
					return $.summernote.options.fontNames;
				}
			},

			toolbar: {
				default() {
					return $.summernote.options.toolbar;
				}
			}
		},

		mounted() {
			this.setup();

			// populate with v-model value, if exists
			if (this.$vnode.data.model.value) {
				$(this.$el).summernote('code', this.$vnode.data.model.value);
			}

			// set a event listener to update v-model
			$(this.$el).on('summernote.change', (e, content) => this.emitInput(content));
		},

		beforeDestroy() {
			$(this.$el).off('summernote.change', this.emitInput);
		},

		methods: {
			setup() {
				// set language
				if (this.lang !== 'en-US') {
					require('summernote/dist/lang/summernote-' + this.lang);
				}

				// start summernote
				$(this.$el).summernote({
					lang: this.lang,
					fontNames: this.fontNames,
					placeholder: this.placeholder,
					toolbar: this.toolbar
				});
			},

			emitInput(value) {
				this.$emit('input', value);
			}
		}
	}
</script>