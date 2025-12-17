<template>
	<component
		:is="tag"
		:style="computedStyle"
		@pointerenter="isHover = true"
		@pointerleave="isHover = false; isActive = false"
		@pointerdown="isActive = true"
		@pointerup="isActive = false"
		@pointercancel="isActive = false"
		@focus="isFocus = true"
		@blur="isFocus = false"
		@focusin="isFocusWithin = true"
		@focusout="isFocusWithin = false"
	>
		<slot>
			Text
		</slot>
	</component>
</template>
<script>
	import {
		parse,
		getStyle
	} from '@componentor/adaptive';
	export default {
		wysiwyg: true,
		inject: {
			theme: {
				default: ''
			},
			breakpoint: {
				default: ''
			}
		},
		data() {
			return {
				isHover: false,
				isActive: false,
				isFocus: false,
				isFocusWithin: false
			};
		},
		props: {
			tag: {
				type: String,
				default: 'span'
			},
			adapt: {
				type: [String, Object, Array],
				default: ''
			},
			state: {
				type: [String, Array],
				default: ''
			},
			disabled: {
				type: Boolean,
				default: false
			},
			themeName: {
				type: String,
				default: ''
			},
			breakpointName: {
				type: String,
				default: ''
			},
			breakpointStrategy: {
				type: String,
				default: 'mobile-first',
				validator: v => ['exact', 'mobile-first', 'desktop-first'].includes(v)
			},
			themeStrategy: {
				type: String,
				default: 'fallback',
				validator: v => ['strict', 'fallback'].includes(v)
			}
		},
		computed: {
			adaptString() {
				if (!this.adapt) return '';
				if (typeof this.adapt === 'string') return this.adapt;
				if (Array.isArray(this.adapt)) {
					return this.adapt.map(item => {
							if (typeof item === 'string') return item;
							return Object.entries(item)
								.map(([key, value]) => `${key}:${value}`)
								.join('; ');
						})
						.join('; ');
				}
				return Object.entries(this.adapt)
					.map(([key, value]) => `${key}:${value}`)
					.join('; ');
			},
			stateArray() {
				const native = [];
				if (this.isHover) native.push('hover');
				if (this.isActive) native.push('active');
				if (this.isFocus) native.push('focus');
				if (this.isFocusWithin) native.push('focus-within');
				if (this.disabled) native.push('disabled');
				if (!this.state) return native;
				const custom = Array.isArray(this.state) ? this.state : this.state.split(':');
				return [...native, ...custom];
			},
			computedStyle() {
				if (!this.adaptString) return '';
				const parsed = parse(this.adaptString);
				return getStyle(parsed, {
					theme: this.themeName || this.theme,
					breakpoint: this.breakpointName || this.breakpoint,
					states: this.stateArray,
					breakpointStrategy: this.breakpointStrategy,
					themeStrategy: this.themeStrategy
				});
			}
		}
	};

</script>