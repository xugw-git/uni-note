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
- regexp.test(str)

方法 \`regexp.test(str)\` 查找匹配项，然后返回 true/false 表示是否存在。

- 选择（OR）\`|\`

- 修饰符

\`i\` - 使用此修饰符后，搜索时不区分大小写: A 和 a 没有区别。
\`g\` - 使用此修饰符后，搜索时会查找所有的匹配项，而不只是第一个。
\`m\` - 多行模式（详见章节 Flag "m" — 多行模式）。
\`s\` - 启用 “dotall” 模式，允许点 \`.\` 匹配换行符 \`\n\`。

- str.match(regexp)

\`match()\` 方法检索返回一个字符串匹配正则表达式的结果。
如果未使用g标志，则仅返回第一个完整匹配及其相关的捕获组（Array）。 在这种情况下，返回的项目将具有如下所述的其他属性。
\`groups\`: 一个捕获组数组 或 undefined（如果没有定义命名捕获组）。
\`index\`: 匹配的结果的开始位置
\`input\`: 搜索的字符串str

\`\`\` javascript
const paragraph = 'The quick brown fox jumps over the lazy dog. It barked.';
const regex = /[A-Z]/;
const found = paragraph.match(regex);
console.log(found);        //Array ["T"]
console.log(found.groups)  //undefined
console.log(found.index);  //0
console.log(found.input);  //"The quick brown fox jumps over the lazy dog. It barked."
\`\`\`

如果使用g标志，则将返回与完整正则表达式匹配的所有结果，但不会返回捕获组。

\`\`\` javascript
const paragraph = 'The quick brown fox jumps over the lazy dog. It barked.';
const regex = /[A-Z]/g;
const found = paragraph.match(regex);
console.log(found);    //Array ["T", "I"]
\`\`\`

如果没有匹配项，则无论是否带有标记 g ，都将返回 \`null\`。  
如果没有匹配项，我们得到的不是一个空数组，而是 \`null\`。

- 字符类

\`\d\`（“d” 来自 “digit”） - 数字：从 0 到 9 的字符。  
\`\s\`（“s” 来自 “space”） - 空格符号：包括空格，制表符 \`\t\`，换行符 \`\n\` 和其他少数稀有字符，例如 \`\v\`，\`\f\` 和 \`\r\`。  
\`\w\`（“w” 来自 “word”） - “单字”字符：拉丁字母或数字或下划线 _。非拉丁字母（如西里尔字母或印地文）不属于 \w。  
\`\D\` - 非数字：除 \`\d\` 以外的任何字符，例如字母。  
\`\S\` - 非空格符号：除 \`\s\` 以外的任何字符，例如字母。  
\`\W\` - 非单字字符：除 \`\w\` 以外的任何字符，例如非拉丁字母或空格。  
\`.\` - 是一种特殊字符类，它与 “除换行符之外的任何字符” 匹配。在“dotall” 模式下匹配任何字符。

- 集合和范围 [...]

在方括号 \`[…]\` 中的几个字符或者字符类意味着“搜索给定的字符中的任意一个”。

比如说，\`[eao]\` 意味着查找在 3 个字符 'a'、'e' 或者 'o' 中的任意一个。这被叫做一个集合。  
方括号也可以包含字符范围。比如说，\`[a-z]\` 会匹配从 a 到 z 范围内的字母，\`[0-5]\` 表示从 0 到 5 的数字。  
除了普通的范围匹配，还有类似 \`[^…]\` 的“排除”范围匹配。比如 \`[^aeyo]\` 匹配任何除了 'a'、'e'、'y' 或者 'o' 之外的字符。  
在方括号 \`[…]\` 表示中，绝大多数特殊字符可以在不转义的情况下使用。

- 量词 \`+,*,?\` 和 \`{n}\`

数量 \`{n}\`  
  确切的位数：\`{5}\`
  某个范围的位数：\`{3,5}\`
缩写  
  \`+\` - 代表“一个或多个”，相当于 \`{1,}\`
  \`*\` - 代表着“零个或多个”，相当于 \`{0,}\`。
  \`?\` - 代表“零个或一个”，相当于 \`{0,1}\`。

- 转义

\`\[\` \`\\\` \`\^\` \`\$\` \`\.\` \`\|\` \`\?\` \`\*\` \`\+\` \`\(\` \`\)\` \`\/\` 等需要在它们前面加上反斜杠 \`\\\`（“转义它们”）

- 懒惰模式

默认情况下，正则表达式引擎会尝试尽可能多地重复量词。例如，\`\d+\` 检测所有可能的字符。  
通过在量词后添加问号 \`?\` 来启用。如\`\d+?\`  

- 锚点：字符串开始 \`^\` 和末尾 \`$\`

- 捕获组

模式的一部分可以用括号括起来 \`(...)\`。这称为“捕获组（capturing group）”。  
这有两个影响：  
它允许将匹配的一部分作为结果数组中的单独项。  
如果我们将量词放在括号后，则它将括号视为一个整体。
>不带括号，模式 \`go+\` 表示 g 字符，其后 o 重复一次或多次。例如 \`goooo\` 或 \`gooooooooo\`。
>括号将字符组合，所以 \`(go)+\` 匹配 \`go\`，\`gogo\`，\`gogogo\`等。

\`\`\` javascript
//域名
let regexp = /(\w+\.)+\w+/g;
alert( "site.com my.site.com".match(regexp) ); // site.com,my.site.com

//邮箱
let regexp = /[-.\w]+@([\w-]+\.)+[\w-]+/g;
alert("my@mail.com @ his@site.com.uk".match(regexp)); // my@mail.com, his@site.com.uk
\`\`\`

更多内容：<https://zh.javascript.info/regexp-groups>

- 前瞻断言与后瞻断言

前瞻肯定断言 \`x(?=y)\`，它表示“匹配 x, 仅在后面是 y 的情况”。

\`\`\` javascript
let str = "1 turkey costs 30€";
alert( str.match(/\d+(?=€)/) ); // 30 （正确地跳过了单个的数字 1）
\`\`\`

前瞻否定断言 \`x(?!y)\`，意思是 “匹配 x, 但是仅在不被 y 跟随的情况下匹配成功”。

\`\`\` javascript
let str = "2 turkeys cost 60€";
alert( str.match(/\d+(?!€)/) ); // 2（正确地跳过了价格）
\`\`\`

后瞻肯定断言：\`(?<=y)x\`, 匹配 x, 仅在前面是 y 的情况。  
后瞻否定断言：\`(?<!y)x\`, 匹配 x, 仅在前面不是 y 的情况。  
捕获组:  
  环视断言括号中（前瞻和后瞻的通用名称）的内容不会成为匹配到的一部分结果。例如：在模式 \`\d+(?!€)\` 中，€ 符号就不会出现在匹配结果中。
  但是如果我们想要捕捉整个环视表达式或其中的一部分，那也是有可能的。只需要将其包裹在另加的括号中。

  \`\`\` javascript
  let str = "1 turkey costs 30€";
  let reg = /\d+(?=(€|kr))/; // €|kr 两边有额外的括号
  alert( str.match(reg) ); // 30, €
  \`\`\`

- str.replace(str|regexp, str|func)

\`replace()\` 方法返回一个由替换值（replacement）替换部分或所有的模式（pattern）匹配项后的新字符串。模式可以是一个字符串或者一个正则表达式，替换值可以是一个字符串或者一个每次匹配都要调用的回调函数。如果pattern是字符串，则仅替换第一个匹配项。  
该方法并不改变调用它的字符串本身，而只是返回一个新的替换后的字符串。  
在进行全局的搜索替换时，正则表达式需包含 g 标志。

- str.trim()

\`trim()\` 方法会从一个字符串的两端删除空白字符。  
\`trim()\` 方法返回一个从两头去掉空白字符的字符串，并不影响原字符串本身。
>使用正则表达式和 \`str.replace()\` 实现：

\`\`\` javascript
let result = str.replace(/^\s+|\s+$/g, "");
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
