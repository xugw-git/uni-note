<template>
	<view class="content">
		<uni-card margin="0">
			<mp-html :content="html" />
		</uni-card>
	</view>
</template>

<script>
	import tabbar from '../../components/tabbar.vue';
	import {
		marked
	} from 'marked';
	import hljs from 'highlight.js';
	import 'highlight.js/styles/atom-one-light.css';

	export default {
		components: {
			tabbar
		},
		data() {
			return {
				html: '',
				text: `
- arr.push()

\`push()\` 方法将一个或多个元素添加到数组的末尾，并返回该数组的新长度。

- arr.pop()

\`pop()\`方法从数组中删除最后一个元素，并返回该元素的值。此方法更改数组的长度。

- arr.shift()

\`shift()\` 方法从数组中删除第一个元素，并返回该元素的值。此方法更改数组的长度。

- arr.unshift()

\`unshift()\` 方法将一个或多个元素添加到数组的开头，并返回该数组的新长度(该方法修改原有数组)。

- 生成随机数

\`Math.random()\` 函数返回一个浮点型伪随机数字，在0（包括0）和1（不包括）之间。  
\`Math.floor()\` 返回小于或等于一个给定数字的最大整数。
\`Math.floor(Math.random() * (max - min + 1)) + min\` 生成某个范围内的随机整数。

- parseInt()

\`parseInt(string, radix)\` 解析一个字符串并返回指定基数的十进制整数， \`radix\` 是2-36之间的整数，表示被解析字符串的基数。

- 三元运算符 \`?\`

\`\`\` javascript
let result = condition ? value1 : value2;
\`\`\`

计算条件结果，如果结果为真，则返回 \`value1\`，否则返回 \`value2\`。

- 使用递归来创建一个数字序列

\`\`\` javascript
function rangeOfNumbers(startNum, endNum) {
  if (endNum - startNum === 0) {
    return [startNum];
  } else {
    var numbers = rangeOfNumbers(startNum, endNum - 1);
    numbers.push(endNum);
    return numbers;
  }
}
\`\`\`
`
			}
		},
		mounted() {
			this.highlight()
			this.html = marked(this.text)
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
