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
函数式编程是一种方案简单、功能独立、对作用域外没有任何副作用的编程范式：INPUT -> PROCESS -> OUTPUT。  
函数式编程：  
1）功能独立——不依赖于程序的状态（比如可能发生变化的全局变量）；  
2）纯函数——同一个输入永远能得到同一个输出；  
3）有限的副作用——可以严格地限制函数外部对状态的更改。

- 函数式编程术语

\`Callbacks\` 是被传递到另一个函数中调用的函数。  
函数就像其他正常值一样，可以赋值给变量、传递给另一个函数，或从其它函数返回，这种函数叫做\`头等 (first-class) 函数\`。 在 JavaScript 中，所有函数都是头等函数。  
将函数为参数或返回值的函数叫做\`高阶 (higher order) 函数\`。
当函数被传递给另一个函数或从另一个函数返回时，那些传入或返回的函数可以叫做 \`lambda\`。

- 命令式编程的危害

命令式编程常常改变程序状态，例如更新全局变量。一个典型的例子是编写 for 循环，它为一个数组的索引提供了准确的迭代方向。
函数式编程是声明式编程的一种形式。JavaScript 提供了许多处理常见任务的方法，所以你无需写出计算机应如何执行它们。 例如，你可以用 map 函数替代上面提到的 for 循环来处理数组迭代。

- 函数式编程的核心原则

1. 不要更改变量或对象 - 创建新变量和对象，并在需要时从函数返回它们。  

变化会导致错误。如果一个函数不改变传入的参数、全局变量等数据，那么它造成问题的可能性就会小很多。  

2. 声明函数参数 - 函数内的任何计算仅取决于参数，而不取决于任何全局对象或变量。  

这样做会有很多好处：  

1）其中一点是让函数更容易测试，因为你确切地知道参数是什么，并且这个参数也不依赖于程序中的任何其他内容。  
2）其次，这样做可以让你更加自信地更改，删除或添加新代码。 因为你很 清楚哪些是可以改的，哪些是不可以改的，这样你就知道哪里可能会有潜在的 陷阱。  
3）最后，无论代码的哪一部分执行它，函数总是会为同一组输入生成相同的输出。

- 在原型上实现 map 方法

\`\`\` javascript
Array.prototype.myMap = function(callback) {
  var newArray = [];
  for (let i = 0; i < this.length; i++) {
    newArray.push(callback(this[i]));
  }
  return newArray;
};
\`\`\`

- arr.filter()

\`filter()\` 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。

- 在原型上实现 filter 方法

\`\`\` javascript
Array.prototype.myFilter = function(callback) {
  var newArray = [];
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i]) === true) {
      newArray.push(this[i]);
    }
  }
  return newArray;
};
\`\`\`

- arr.slice()

\`slice()\` 方法返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括end）。原始数组不会被改变。

- arr.concat()

\`concat()\` 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

- arr.reduce()

\`reduce()\` 方法对数组中的每个元素按序执行一个由您提供的 reducer 函数，每一次运行 reducer 会将先前元素的计算结果作为参数传入，最后将其结果汇总为单个返回值。

- arr.sort()

\`sort()\` 方法用原地算法对数组的元素进行排序，并返回数组。

- str.split()

\`split()\` 方法使用指定的分隔符字符串将一个String对象分割成子字符串数组，以一个指定的分割字串来决定每个拆分的位置。

- arr.join()

\`join()\` 方法将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串。如果数组只有一个项目，那么将返回该项目而不使用分隔符。

- arr.every()

\`every()\` 方法测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值。

- arr.some()

\`some()\` 方法测试数组中是不是至少有1个元素通过了被提供的函数测试。它返回一个布尔值。

- 函数柯里化和局部调用

arity（参数个数）是函数所需的形参的数量。 函数柯里化（Currying）意思是把接受多个 arity 的函数变换成接受单一 arity 的函数。  
换句话说，就是重构函数让它接收一个参数，然后返回接收下一个参数的函数，依此类推。

\`\`\` javascript
function unCurried(x, y) {
  return x + y;
}

function curried(x) {
  return function(y) {
    return x + y;
  }
}

const curried = x => y => x + y
curried(1)(2)  //3
\`\`\`

柯里化在不能一次为函数提供所有参数情况下很有用。 因为它可以将每个函数的调用保存到一个变量中，该变量将保存返回的函数引用，该引用在下一个参数可用时接受该参数。  
柯里化 是一种转换，将 \`f(a,b,c)\` 转换为可以被以 \`f(a)(b)(c)\` 的形式进行调用。JavaScript 实现通常都保持该函数可以被正常调用，并且如果参数数量不足，则返回偏函数。
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
