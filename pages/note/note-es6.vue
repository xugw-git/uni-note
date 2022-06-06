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
- 变量声明的三种方式

\`let\` \`const\` \`var\`

- Object.freeze()

\`Object.freeze(obj)\` 方法可以冻结一个对象，被冻结对象自身的所有属性都不可能以任何方式被修改。

- 箭头函数

\`\`\` javascript
let func = (arg1, arg2, ..., argN) => expression;
\`\`\`

等同于匿名函数：

\`\`\`javascript
let func = function(arg1, arg2, ..., argN) {
  return expression;
};
\`\`\`

具体有两种：
不带花括号：\`(...args) => expression\`
— 右侧是一个表达式：函数计算表达式并返回其结果。
带花括号：\`(...args) => { body }\`  
— 花括号允许我们在函数中编写多个语句，但是我们需要显式地 \`return\` 来返回一些内容。

- Rest 参数 \`...\`

Rest 参数可以通过使用三个点 ... 并在后面跟着包含剩余参数的数组名称，来将它们包含在函数定义中。这些点的字面意思是“将剩余参数收集到一个数组中”。  
Rest 参数用于创建可接受任意数量参数的函数。

- Spread 语法

当在函数调用中使用 \`...arr\` 时，它会把可迭代对象 \`arr\` “展开”到参数列表中。\`arr\` 不仅仅是数组，任何可迭代对象都可以。  
Spread 语法用于将数组传递给通常需要含有许多参数的列表的函数。

- 解构赋值

解构赋值可以立即将一个对象或数组映射到多个变量上。

\`\`\` javascript
//对象解构
const user = { name: 'John Doe', age: 34 };
const { name, age } = user;
//解构数组
const [a, b,,, c] = [1, 2, 3, 4, 5, 6];
console.log(a, b, c);  //1, 2, 5
const [a, b, ...arr] = [1, 2, 3, 4, 5, 7];
console.log(arr);  //[3, 4, 5, 7]
\`\`\`

- 模板字符串

\`\`\` javascript
const person = {
  name: "Zodiac Hasbro",
  age: 56
};
const greeting = \`Hello, my name is \${person.name}!
I am \${person.age} years old.\`;
console.log(greeting);
\`\`\`

- 简洁的函数声明

\`\`\` javascript
// const person = {
//   name: "Taylor",
//   sayHello: function() {
//     return \`Hello! My name is \${this.name}.\`;
//   }
// };
const person = {
  name: "Taylor",
  sayHello() {
    return \`Hello! My name is \${this.name}.\`;
  }
};
\`\`\`

- 简洁的对象字面量声明

\`\`\` javascript
// const getMousePosition = (x, y) => ({x: x, y: y});
const getMousePosition = (x, y) => ({ x, y });
\`\`\`

- “class” 语法

\`\`\` javascript
class MyClass {
  // class 方法
  constructor() { ... }
  method1() { ... }
  method2() { ... }
  method3() { ... }
  ...
}
\`\`\`

然后使用 \`new MyClass()\` 来创建具有上述列出的所有方法的新对象。
\`new\` 会自动调用 \`constructor()\` 方法，因此我们可以在 \`constructor()\` 中初始化对象。

- getters 和 setters

你可以从对象中获得一个值，也可以给对象的属性赋值。  
这些操作通常被称为 getters 以及 setters。  
Getter 函数的作用是可以让对象返回一个私有变量，而不需要直接去访问私有变量。  
Setter 函数的作用是可以基于传进的参数来修改对象中私有变量。这些修改可以是计算，或者是直接替换之前的值。

\`\`\` javascript
class Book {
  constructor(author) {this._author = author;}
  // getter
  get writer() {return this._author;}
  // setter
  set writer(updatedAuthor) {this._author = updatedAuthor;}
}

const novel = new Book('anonymous');
console.log(novel.writer);  //anonymous
novel.writer = 'newAuthor';
console.log(novel.writer);  //newAuthor
\`\`\`

- 导出和导入

\`\`\` javascript
// 导出数组
export let months = ['Jan', 'Feb', 'Mar','Apr', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
// 导出 const 声明的变量
export const MODULES_BECAME_STANDARD_YEAR = 2015;
// 导出类
export class User {
  constructor(name) {
    this.name = name;
  }
}
\`\`\`

\`\`\` javascript
// 📁 say.js
function sayHi(user) {
  alert(\`Hello, \${user}!\`);
}
tion sayBye(user) {
  alert(\`Bye, \${user}!\`);
}
export {sayHi, sayBye}; // 导出变量列表
\`\`\`

\`\`\` javascript
import {sayHi, sayBye} from './say.js';
\`\`\`

\`export default\` 用于为模块或文件声明一个返回值，在每个文件或者模块中应当只默认导出一个值。不能将 export default 与 var、let 或 const 同时使用。

\`\`\` javascript
// 📁 user.js
export default class User { // 只需要添加 "default" 即可
  constructor(name) {
    this.name = name;
  }
}
//然后将其导入而不需要花括号：
// 📁 main.js
import User from './user.js'; // 不需要花括号 {User}，只需要写成 User 即可
new User('John');
\`\`\`

- Promise

\`Promise\` 是异步编程的一种解决方案 - 它在未来的某时会生成一个值。  
\`Promise\` 是构造器函数，需要通过 new 关键字来创建。 构造器参数是一个函数，该函数有两个参数 - \`resolve\` 和 \`reject\`。 通过它们来判断 promise 的执行结果。

\`\`\` javascript
const myPromise = new Promise((resolve, reject) => {});
\`\`\`

Promise 有三个状态：\`pending\`、\`fulfilled\` 和 \`rejected\`。 Promise 成功时调用 resolve，promise 执行失败时调用 reject。

\`\`\` javascript
const myPromise = new Promise((resolve, reject) => {
  if(condition here) {
    resolve("Promise was fulfilled");
  } else {
    reject("Promise was rejected");
  }
});
\`\`\`

当 promise 完成 resolve 时会触发 then 方法。  

\`\`\` javascript
promise.then(
  function(result) { /* handle a successful result */ },
  function(error) { /* handle an error */ }
);
\`\`\`

\`.then\` 的第一个参数是一个函数，该函数将在 \`promise resolved\` 后运行并接收结果。  
\`.then\` 的第二个参数也是一个函数，该函数将在 \`promise rejected\` 后运行并接收 error。  
如果我们只对成功完成的情况感兴趣，那么我们可以只为 \`.then\` 提供一个函数参数。  
如果我们只对 error 感兴趣，那么我们可以使用 \`null\` 作为第一个参数：\`.then(null, errorHandlingFunction)\`。  
或者我们也可以使用 \`.catch(errorHandlingFunction)\`，其实是一样的：

\`\`\` javascript
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});
// .catch(f) 与 promise.then(null, f) 一样
promise.catch(alert); // 1 秒后显示 "Error: Whoops!"
\`\`\`

\`.catch(f)\` 调用是 \`.then(null, f)\` 的完全的模拟，它只是一个简写形式。
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
