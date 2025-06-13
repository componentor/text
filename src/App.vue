<template>
	<component
		:is="styleOptions?.type || 'div'"
		:style="style"
		@pointerover="hover=true"
		@pointerleave="hover=false"
	>
		<slot>
			Text
		</slot>
	</component>
</template>
<script>
	export default {
		wysiwyg: true,
		inject: ['theme', 'breakpoint'],
		props: {
			type: {
				type: String,
				options: [{
					key: 'div',
					value: 'div'
				}, {
					key: 'span',
					value: 'span'
				}, {
					key: 'p',
					value: 'p'
				}],
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			fontSize: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			fontWeight: {
				type: String,
				default: '',
				control: 'slider',
				unit: '',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			lineHeight: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			color: {
				type: String,
				default: '',
				control: 'color',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			backgroundColor: {
				type: String,
				default: '',
				control: 'color',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			backgroundImage: {
				type: String,
				default: '',
				control: 'media',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			padding: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			paddingTop: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			paddingRight: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			paddingBottom: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			paddingLeft: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			margin: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			marginTop: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			marginRight: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			marginBottom: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			},
			marginLeft: {
				type: String,
				default: '',
				control: 'slider',
				unit: 'px',
				breakpoints: ['xs', 'sm', 'md', 'lg', 'xl', '2xl'],
				themes: ['light', 'dark'],
				groups: ['default', 'hover']
			}
		},
		data: () => ({
			hover: false,
			windowWidth: typeof global !== 'undefined' ? global?.windowWidth || 1280 : 1280,
			colorSchemeMediaQuery: null,
			darkmode: false
		}),
		mounted() {
			this.windowWidth = window.innerWidth;
			document.cookie = `windowWidth=${window.innerWidth}; path=/; max-age=3600; Secure; SameSite=None`;
			this.colorSchemeMediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
			this.darkmode = window.matchMedia('(prefers-color-scheme: dark)')
				.matches;
			this.colorSchemeMediaQuery.addEventListener('change', this.handleColorSchemeChange);
			window.addEventListener('resize', this.handleResize);
		},
		beforeUnmount() {
			this.colorSchemeMediaQuery.removeEventListener('change', this.handleColorSchemeChange);
			window.removeEventListener('resize', this.handleResize);
		},
		methods: {
			handleColorSchemeChange(event) {
				this.darkmode = event.matches;
			},
			handleResize() {
				this.windowWidth = window.innerWidth;
			}
		},
		computed: {
			themeComputed() {
				if (this.theme) return this.theme;
				return this.darkmode ? 'dark' : 'light';
			},
			bpoint() {
				if (this.breakpoint) return this.breakpoint;
				if (this.windowWidth > 1280) return '2xl';
				if (this.windowWidth > 1024) return 'xl';
				if (this.windowWidth > 768) return 'lg';
				if (this.windowWidth > 640) return 'md';
				if (this.windowWidth > 480) return 'sm';
				return 'xs';
			},
			group() {
				if (this.hover) return 'hover';
				return 'default';
			},
			styleOptions() {
				const style = {};
				const props = ['type', 'fontSize', 'fontWeight', 'lineHeight', 'color', 'backgroundColor', 'backgroundImage', 'padding', 'paddingTop', 'paddingRight', 'paddingBottom', 'paddingLeft', 'margin', 'marginTop', 'marginRight', 'marginBottom', 'marginLeft'];
				const groups = ['default', 'hover'];
				const breakpoints = ['xs', 'sm', 'md', 'lg', 'xl', '2xl'];
				const themes = ['light', 'dark'];
				for (const prop of props) {
					let priority = {};
					if (this[prop]) {
						try {
							priority = JSON.parse(this[prop].replaceAll('`', '"'));
						} catch (e) {}
					}
					const merge = {};
					for (const group of Object.keys(priority)) {
						const pri = priority?.[group] || {};
						merge[group] = {};
						for (const breakpoint of Object.keys(pri)) {
							const p = pri?.[breakpoint] || {};
							merge[group][breakpoint] = {};
							for (const theme of Object.keys(p)) {
								const value = p?.[theme]?.toString() || null;
								merge[group][breakpoint][theme] = value;
							}
						}
					}
					const groups = Object.keys(merge);
					if (groups.length) {
						style[prop] = merge?.['default']?.['xs']?.['light'];
						let limitReached = false;
						let limit = this.bpoint || 'xs';
						let match = false;
						for (const breakpoint of breakpoints) {
							if (!limitReached) {
								const firstPriority = merge?.[this.group]?.[breakpoint]?.[this.themeComputed]?.toString();
								const secondPriority = merge?.[this.group]?.[breakpoint]?.['light']?.toString();
								const value = firstPriority || secondPriority;
								if (value) {
									style[prop] = value;
									match = true;
								}
								limitReached = breakpoint === limit;
							}
						}
						if (!match && this.group !== 'default') {
							limitReached = false;
							limit = this.bpoint || 'xs';
							for (const breakpoint of breakpoints) {
								if (!limitReached) {
									const firstPriority = merge?.['default']?.[breakpoint]?.[this.themeComputed]?.toString();
									const secondPriority = merge?.['default']?.[breakpoint]?.['light']?.toString();
									const value = firstPriority || secondPriority;
									if (value) {
										style[prop] = value;
									}
									limitReached = breakpoint === limit;
								}
							}
						}
					}
				}
				return style;
			},
			style() {
				const ignore = ['type'];
				const obj = {};
				const keys = Object.keys(this.styleOptions)
					.filter(key => !ignore.includes(key));
				for (const key of keys) {
					obj[key] = this.styleOptions[key];
				}
				return obj;
			}
		}
	};

</script>