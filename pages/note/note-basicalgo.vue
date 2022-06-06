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
- 反转字符串

例：\`reverseString("hello")\` 应返回 \`olleh\`

\`\`\` javascript
//遍历
function reverseString(str) {
  let reversedStr = "";
  for (let i = str.length - 1; i >= 0; i--) {
    reversedStr += str[i];
  }
  return reversedStr;
}
//arr.reverse()
function reverseString(str) {
  return str.split('').reverse().join('');
}
\`\`\`

\`reverse()\` 方法将数组中元素的位置颠倒，并返回该数组。数组的第一个元素会变成最后一个，数组的最后一个元素变成第一个。该方法会改变原数组。

- 整数的阶乘

例：\`factorialize(5)\` 应该返回 \`120\`

\`\`\` javascript
//遍历
function factorialize(num) {
  let product = 1;
  for (let i = 2; i <= num; i++) {
    product *= i;
  }
  return product;
}
//递归
function factorialize(num) {
  if (num === 0) {return 1;}
  return num * factorialize(num - 1);
}
\`\`\`

- 找出字符串中的最长单词的长度

例：\`findLongestWordLength("The quick brown fox jumped over the lazy dog")\` 应返回 \`6\`

\`\`\` javascript
//for循环遍历
function findLongestWordLength(str) {
  let words = str.split(' ');
  let maxLength = 0;
  for (let i = 0; i < words.length; i++) {
    if (words[i].length > maxLength) {
      maxLength = words[i].length;
    }
  }
  return maxLength;
}
//arr.reduce
function findLongestWordLength(s) {
  return s.split(' ')
    .reduce(function(longest, word) {
      return Math.max(longest, word.length)
    }, 0);
}
//arr.map()
function findLongestWordLength(str) {
  return Math.max(...str.split(" ").map(word => word.length));
}
//递归
function findLongestWordLength(str) {
  const words = str.split(" ");
  if (words.length == 1) {
    return words[0].length;
  }
  return Math.max(
    words[0].length,
    findLongestWordLength(words.slice(1).join(" "))
  );
}
\`\`\`

- 找出多个数组中的最大数字

请返回一个数组，该数组由参数中每个子数组中的最大数字组成。 为简单起见，给出的数组总会包含 4 个子数组。
例：\`largestOfFour([[13, 27, 18, 26], [4, 5, 1, 3], [32, 35, 37, 39], [1000, 1001, 857, 1]])\` 应返回 \`[27, 5, 39, 1001]\`

\`\`\` javascript
//遍历
function largestOfFour(arr) {
  const results = [];
  for (let i = 0; i < arr.length; i++) {
    let largestNumber = arr[i][0];
    for (let j = 1; j < arr[i].length; j++) {
      if (arr[i][j] > largestNumber) {
        largestNumber = arr[i][j];
      }
    }
    results[i] = largestNumber;
  }
  return results;
}
//arr.map()
function largestOfFour(arr) {
  return arr.map(function(group) {
    return group.reduce(function(prev, current) {
      return current > prev ? current : prev;
    });
  });
}
//递归
function largestOfFour(arr, finalArr = []) {
  return !arr.length
    ? finalArr
    : largestOfFour(arr.slice(1), finalArr.concat(Math.max(...arr[0])))
}
\`\`\`

- 确认结尾

检查字符串（第一个参数 str）是否以给定的目标字符串（第二个参数 target）结束。  
这个挑战可以用 ES2015 引入的 \`str.endsWith(searchString[, length])\` 方法来解决。  
例：\`confirmEnding("Congratulation", "on")\` 应返回 \`true\`

\`\`\` javascript
//str.slice()
function confirmEnding(str, target) {
  return str.slice(str.length - target.length) === target;
}

function confirmEnding(str, target) {
  return str.slice(-target.length) === target
}
//正则
function confirmEnding(str, target) {
  let re = new RegExp(target + "$", "i");
  return re.test(str);
}
\`\`\`

- 重复输出字符串

将一个给定的字符串 str（第一个参数）重复输出 num（第二个参数）次。 如果 num 不是正数，返回空字符串。  
不要使用 JavaScript 内置的 \`str.repeat(count)\` 方法。  
例：\`repeatStringNumTimes("abc", 3)\` 应返回 \`abcabcabc\`

\`\`\` javascript
//循环
function repeatStringNumTimes(str, num) {
  let accumulatedStr = "";
  for (let i = 0; i < num; i++) {
    accumulatedStr += str;
  }
  return accumulatedStr;
}
//递归
function repeatStringNumTimes(str, num) {
  if (num < 1) {
    return "";
  } else {
    return str + repeatStringNumTimes(str, num - 1);
  }
}
\`\`\`

- 截断字符串

如果传入的字符串（第一个参数）的长度大于传入的值（第二个参数），请在这个位置截断它， 并在后面加上 \`...\`，然后返回结果。  
例：\`truncateString("A-tisket a-tasket A green and yellow basket", 8)\` 应返回 \`A-tisket...\`

\`\`\` javascript
function truncateString(str, num) {
  if (str.length > num) {
    return str.slice(0, num) + "...";
  } else {
    return str;
  }
}
\`\`\`

- 按参数过滤数组

请写一个函数来检查数组（第一个参数 \`arr\`）中的元素，并返回数组中第一个通过校验测试的元素。 其中，“通过校验测试”指的是对于数组中的一个元素 \`x\`，若 \`func(x)\` 返回的结果为 \`true\`，则校验测试通过。 如果没有元素通过测试，请返回 \`undefined\`。  
例：\`findElement([1, 3, 5, 8, 9, 10], function(num) { return num % 2 === 0; })\` 应返回 \`8\`

\`\`\` javascript
//遍历
function findElement(arr, func) {
  let num = 0;
  for (let i = 0; i < arr.length; i++) {
    num = arr[i];
    if (func(num)) {return num;}
  }
  return undefined;
}
//arr.find()
function findElement(arr, func) {
  return arr.find(func);
}
//arr.map()
function findElement(arr, func) {
  return arr[arr.map(func).indexOf(true)];
}
//递归
//(如果arr===[]或func(arr[0])===true)=>arr[0]
function findElement(arr, func) {
  return arr.length && !func(arr[0]) 
    ? findElement(arr.slice(1), func)
    : arr[0];
}
\`\`\`

- 句中单词首字母大写

请将传入的字符串中，每个单词的第一个字母变成大写并返回。 注意除首字母外，其余的字符都应是小写的。
例：\`titleCase("I'm a little tea pot")\` 应返回 \`I'm A Little Tea Pot\`

\`\`\` javascript
//拆成数组后遍历
function titleCase(str) {
  const newTitle = str.split(" ");
  const updatedTitle = [];
  for (let st in newTitle) {
    updatedTitle[st] = newTitle[st][0].toUpperCase() + newTitle[st].slice(1).toLowerCase();
  }
  return updatedTitle.join(" ");
}
//arr.map()
function titleCase(str) {
  return str
    .toLowerCase()
    .split(" ")
    .map(val => val.replace(val.charAt(0), val.charAt(0).toUpperCase()))
    .join(" ");
}
//正则
function titleCase(str) {
  return str
    .toLowerCase()
    .replace(/(^|\s)\S/g, L => L.toUpperCase());
}
\`\`\`

- Slice 与 Splice

本挑战的输入参数为两个数组和一个索引值。将第一个数组中的所有元素依次复制到第二个数组中。请注意，你需要从第二个数组索引值为 n 的地方开始插入。最后，请返回插入元素后的数组。 作为输入参数的两个数组在函数执行前后应保持不变。
例：\`frankenSplice([1, 2, 3], [4, 5], 1)\` 应返回 \`[4, 1, 2, 3, 5]\`

\`\`\` javascript
function frankenSplice(arr1, arr2, n) {
  let localArr = arr2.slice();
  localArr.splice(n, 0, ...arr1);
  return localArr;
}
\`\`\`

- 过滤数组中的假值

从数组中移除所有假值（falsy values）。
JavaScript 中的假值有 \`false\`、\`null\`、\`0\`、\`""\`、\`undefined\`、\`NaN\`。
提示：可以考虑将每个值都转换为布尔值（boolean）。
例：\`bouncer([7, "ate", "", false, 9])\` 应返回 \`[7, "ate", 9]\`

\`\`\` javascript
//遍历
function bouncer(arr) {
  let newArray = [];
  for (let i = 0; i < arr.length; i++) {
    if (arr[i]) newArray.push(arr[i]);
  }
  return newArray;
}
//
function bouncer(arr) {
  return arr.filter(Boolean);
}
\`\`\`

- 找出元素在排序后数组中的索引

数组（第一个参数）在排序后，将一个值（第二个参数）插入该数组，并使数组保持有序。返回这个新插入元素的最小索引值。 返回值应为一个数字。
例：\`getIndexToIns([10, 20, 30, 40, 50], 35)\` 应返回 \`3\`

\`\`\` javascript
//遍历
function getIndexToIns(arr, num) {
  arr.sort((a, b) => a - b);
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] >= num)
      return i;
  }
  return arr.length;
}
//arr.filter()
function getIndexToIns(arr, num) {
  return arr.filter(val => num > val).length;
}
//arr.findIndex()
function getIndexToIns(arr, num) {
  let index = arr
    .sort((curr, next) => curr - next)
    .findIndex(currNum => num <= currNum);
  return index === -1 ? arr.length : index;
}
//先合并排序再使用arr.indexOf()
function getIndexToIns(arr, num) {
  return arr
    .concat(num)
    .sort((a, b) => a - b)
    .indexOf(num);
}
\`\`\`

- 比较字符串

如果数组里的第一个字符串包含了第二个字符串中的所有字母（忽略大小写），则返回 true。
例：\`mutation(["hello", "Hello"])\` 应返回 \`true\`  
\`mutation(["hello", "hey"])\` 应返回 \`false\`

\`\`\` javascript
//遍历
function mutation(arr) {
  let test = arr[1].toLowerCase();
  let target = arr[0].toLowerCase();
  for (let i = 0; i < test.length; i++) {
    if (target.indexOf(test[i]) < 0) return false;
    //if (!target.includes(test[i])) return false;
  }
  return true;
}
//arr.every()
function mutation(arr) {
  return arr[1]
    .toLowerCase()
    .split("")
    .every(function(letter) {
      return arr[0].toLowerCase().indexOf(letter) !== -1;
    });
}
\`\`\`

- 分割数组

请编写一个函数，该函数将一个数组（第一个参数）拆分成若干长度为 \`size\`（第二个参数）的子数组，并将它们作为二维数组返回。  
例：\`chunkArrayInGroups(["a", "b", "c", "d"], 2)\` 应返回 \`[["a", "b"], ["c", "d"]]\`

\`\`\` javascript
//for遍历
function chunkArrayInGroups(arr, size) {
  let newArr = [];
  for (let i = 0; i < arr.length; i += size) {
    newArr.push(arr.slice(i, i + size));
  }
  return newArr;
}
//while遍历
function chunkArrayInGroups(arr, size) {
  let newArr = [];
  let i = 0;
  while (i < arr.length) {
    newArr.push(arr.slice(i, i + size));
    i += size;
  }
  return newArr;
}
//while遍历 使用arr.splice()
function chunkArrayInGroups(arr, size) {
  let newArr = [];
  while (arr.length > 0) {
    newArr.push(arr.splice(0, size));
  }
  return newArr;
}
//递归
function chunkArrayInGroups(arr, size) {
  if (arr.length <= size) {
    return [arr];
  } else {
    return [arr.slice(0, size)].concat(
      chunkArrayInGroups(arr.slice(size), size)
    );
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
