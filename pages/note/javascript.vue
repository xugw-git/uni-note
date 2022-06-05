<template>
	<view class="content">
		<u--text text="javascript\nfunction(){\n\tconsole.log(123)\n}\n"></u--text>
		<mp-html :content="html" />
		<tabbar :tabValue="1"></tabbar>
	</view>
</template>

<script>
import tabbar from '../../components/tabbar.vue';
import { marked } from 'marked';
import hljs from 'highlight.js';
import 'highlight.js/styles/atom-one-light.css';

const marktext = `
\`\`\` javascript
let result = condition ? value1 : value2;
console.log('hello')
\`\`\`
`;
export default {
	components: { tabbar },
	data() {
		return {
			html: '<div>Hello World!</div>',
			markdown: marktext
		};
	},
	mounted() {
		this.highlight();
		this.html = marked(this.markdown);
	},
	methods: {
		highlight() {
			hljs.configure({
				useBR: true,
				tabReplace: ' '
			})
			marked.setOptions({
				renderer: new marked.Renderer(),
				langPrefix: 'hljs language-',
				gfm: true,
				tables: true,
				breaks: false,
				pedantic: false,
				highlight: function(code, lang) {
					if (lang && hljs.getLanguage(lang)) {
						return hljs.highlight(lang, code, true).value;
					} else {
						return hljs.highlightAuto(code).value;
					}
				}
			})
		}
	}
};
</script>
