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
- arr.splice()

\`splice()\` 方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。此方法会改变原数组。
语法：\`array.splice(start[, deleteCount[, item1[, item2[, ...]]]])\`  

\`\`\` javascript
const numbers = [10, 11, 12, 12, 15];
numbers.splice(3, 1, 13, 14);
console.log(numbers);  //[ 10, 11, 12, 13, 14, 15 ]
\`\`\`

- arr.slice()

\`arr.slice([begin[, end]])\` 方法返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括end）。原始数组不会被改变。

\`\`\` javascript
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]
console.log(animals.slice(-2));
// expected output: Array ["duck", "elephant"]
console.log(animals.slice());
// expected output: Array ["ant", "bison", "camel", "duck", "elephant"]
\`\`\`

展开运算符（spread operator）\`[...arr]\`可以实现和\`arr.slice()\`一样的复制数组的效果。

- arr.indexOf()

\`arr.indexOf(searchElement[, fromIndex])\` 方法返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。

\`\`\` javascript
const beasts = ['ant', 'bison', 'camel', 'duck', 'bison'];
console.log(beasts.indexOf('bison'));
// expected output: 1
console.log(beasts.indexOf('bison', 2));
// expected output: 4
console.log(beasts.indexOf('giraffe'));
// expected output: -1
\`\`\`

- delete 操作符

\`delete\` 操作符用于删除对象的某个属性；如果没有指向这个属性的引用，那它最终会被释放。
\`delete expression\`  
\`expression\` 的计算结果应该是某个属性的引用，例如：

\`\`\` javascript
delete object.property
delete object['property']
\`\`\`

- 使用 for...in 语句遍历对象

\`\`\` javascript
for (let user in users) {
  console.log(user);
}
\`\`\`

将在控制台打印 Alan、Jeff、Sarah 和 Ryan - 每个值占一行。

- obj.hasOwnProperty()

\`obj.hasOwnProperty(prop)\` 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键）。

\`\`\` javascript
const object1 = {};
object1.property1 = 42;
console.log(object1.hasOwnProperty('property1'));
// expected output: true
console.log(object1.hasOwnProperty('property2'));
// expected output: false
\`\`\`

- Object.keys()

\`Object.keys(obj)\` 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 。

\`\`\` javascript
// simple array
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']
// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']
// array like object with random key ordering
var anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']
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
