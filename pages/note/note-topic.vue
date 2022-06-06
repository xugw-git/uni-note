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
- 回文检查器

如果传入的字符串是回文字符串，则返回 true。 否则返回 false。  
回文 palindrome，指在忽略标点符号、大小写和空格的前提下，正着读和反着读一模一样。  
注意：检查回文时，你需要先去除所有非字母数字的字符（标点、空格和符号），并将所有字母都转换成大写或都转换成小写。  
例：\`palindrome("eye")\` 应返回 \`true\`  
\`palindrome("_eye")\` 应返回 \`true\`

\`\`\` javascript
//双指针(对撞指针)
function palindrome(str) {
  str=str.replace(/[\W_]/g,'').toLowerCase();
  for(let i=0;i<str.length/2;i++){
    if(str[i]!==str[str.length-i-1]){return false}
  }
  return true
}
//转换成数组，使用arr.reverse()
function palindrome (str) {
    if (str.length === 1) {
        return true;
    }
    str = str.replace(/[\W_]/g, '').toLowerCase();
    return str === str.split('').reverse().join('');
}
//递归比较首尾字符
function palindrome(str) {
    str = str.replace(/[\W_]/g, '').toLowerCase();
    if (str.length < 2) {
        return true;
    }
    if (str[0] !== str[str.length - 1]) {
        return false;
    }
    return palindrome(str.slice(1, -1));
}
\`\`\`

- 罗马数字转换器

把传入的数字转为罗马数字。  
转换后的罗马数字字母必须都是大写。  
例：\`convertToRoman(2)\` 应该返回字符串 \`II\`  
\`convertToRoman(3999)\` 应该返回字符串 \`MMMCMXCIX\`

\`\`\` javascript
//将罗马数字与十进制数字一一对应
function convertToRoman(num) {
  const arr=[["","I","II","III","IV","V","VI","VII","VIII","IX"],
  ["","X","XX","XXX","XL","L","LX","LXX","LXXX","XC"],
  ["","C","CC","CCC","CD","D","DC","DCC","DCCC","CM"],
  ["","M","MM","MMM"]];
  let str='';
  let thousandnum=Math.floor(num/1000);
  let hundrednum=Math.floor((num-thousandnum*1000)/100);
  let tennum=Math.floor((num-thousandnum*1000-hundrednum*100)/10);
  let digitnum=num%10;
  return str.concat(arr[3][thousandnum],arr[2][hundrednum],arr[1][tennum],arr[0][digitnum]);
}
//将罗马数字中的特殊数字列举出来，将参数从大到小匹配
function convert(num) {
    const arr = [1, 4, 5, 9, 10, 40, 50, 90, 100, 400, 500, 900, 1000];
    const strArr = ["I", "IV", "V", "IX", "X", "XL", "L", "XC", "C", "CD", "D", "CM", "M"];
    let result = '';
    while (num > 0) {
        var i = arr.length;
        while (i >= 0) {
            if (num >= arr[i]) {
                result += strArr[i];
                num -= arr[i];
            } else {
                i--;
            }
        }
    }
    return result;
}
\`\`\`

- 凯撒密码

凯撒密码（ Caesar cipher）是最简单且最广为人知的密码（ciphers），也被称为移位密码（shift cipher）。 在移位密码中，明文中的字母通过按照一个固定数目进行偏移后被替换成新的字母。  
ROT13 是一个被广泛使用的加密技术，明文中的所有字母都被移动 13 位。 也就是， A ↔ N，B ↔ O 等等。  
编写一个函数，它将 ROT13 编码的字符串作为输入并返回解码字符串。  
所有解码后的字母都必须为字母大写。 请不要解码非字母的字符（例如，空格、标点符号），但你需要在结果中保留它们。  
例：\`rot13("SERR PBQR PNZC")\` 应解码为 \`FREE CODE CAMP\`
\`rot13("SERR CVMMN!")\` 应解码为 \`FREE PIZZA!\`

\`\`\` javascript
//对照加密前后的字母列表替换相同索引处的字母
function rot13(str) {
  const arrbefore='ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('');
  const arrafter='NOPQRSTUVWXYZABCDEFGHIJKLM'.split('');
  return str.replace(/[A-Z]/g,function(letter){return arrafter[arrbefore.indexOf(letter)]})
}
//大写字母 A-Z 在 UTF-16 代码单元中的索引为 65-90
function rot13(str) {
    let result = '';
    for (let i = 0; i < str.length; i++) {
        let currentCode = str[i].charCodeAt();
        if (currentCode > 90 || currentCode < 65) {
            result += String.fromCharCode(currentCode);
        } else if (currentCode < 78) {
            result += String.fromCharCode(currentCode + 13);
        } else {
            result += String.fromCharCode(currentCode - 13);
        }
    }
    return result;
}
//对于正整数 n 与 m，n % m 的结果应该始终为 0 至 m - 1 中的一个数。
//n % 26 刚好偏移是 13，所以 String.fromCharCode(n) % 26 + 65 就是加密后的结果
function rot13(str) {
    return str.replace(/[A-Z]/g, char => String.fromCharCode(char.charCodeAt() % 26 + 65));
}
\`\`\`

- 电话号码检查器

如果传入的字符串是一个有效的美国电话号码格式，则返回 true。
以下是一些正确的例子：

\`\`\` javascript
555-555-5555
(555)555-5555
（555）555-5555
555 555 5555
5555555555
1 555 555 5555
\`\`\`

在这个挑战中，参数可能是 \`800-692-7753\` 或者 \`8oo-six427676;laskdjf\` 的号码。 你的任务是根据上面不同的格式组合，判断它是否为有效的电话号码。 其中，地区码（电话号码中的前三位）是必须的。 如果提供国家代码，则国家代码只能为 \`1\`。 如果传入的参数是有效的美国电话号码就返回 true，否则返回 false。  
例：\`telephoneCheck("1 555-555-5555")\` 应返回 \`true\`

\`\`\` javascript
//正则
function telephoneCheck(str) {
  return /^1?\s?(\(\d{3}\)|\d{3})(\s|-)?\d{3}(\s|-)?\d{4}$/.test(str);
}
//穷举排除
function telephoneCheck(str) {
  if(str.search(/[^\d\s-()]/)!=-1)return false;
  if(str.indexOf(")") >= 0 && str.indexOf("(") < 0)return false;
  if(str.indexOf("(") >= 0 && str.indexOf(")") < 0)return false;
  if(str.indexOf("(") >= 0 && str.indexOf(")") != str.indexOf("(") + 4 )return false;
  let strArr = str.split('');
  if(strArr[0] == "-"|strArr[str.length+1] == "-"|strArr[str.length] == "-"|strArr[str.length-1] == "-"|strArr[str.length-2] == "-")return false;
  let numStr = str.replace(/[^\d]/g,"");
  if(numStr.length !=10 && numStr.length != 11)return false;
  if(numStr.length == 11 && numStr[0] !== "1")return false;
  return true;
}
\`\`\`

- 计算找零

请编写一个用于收银机的函数 \`checkCashRegister()\`：它的第一个参数为售价 \`price\`、第二个参数为支付金额 \`cash\`、第三个参数为收银机內的金额 \`cid\`。
\`cid\` 是包含货币面值的二维数组。  
函数 \`checkCashRegister()\` 应返回含有 \`status\` 属性和 \`change\` 属性的对象。  
如果收银机內的金额少于应找回的零钱数，或者你无法返回确切的数目时，返回 \`{status: "INSUFFICIENT_FUNDS", change: []}\`。  
如果收银机內的金额恰好等于应找回的零钱数，返回 \`{status: "CLOSED", change: [...]}\`，其中 \`change\` 的属性值就是收银机內的金额。
否则，返回 \`{status: "OPEN", change: [...]}\`，其中 \`change\` 键值是应找回的零钱数，并将找零的面值由高到低排序。  
|货币单位 Unit|面值|
|:-|:-|
|Penny |0.01 美元（PENNY）|
|Nickel |0.05 美元（NICKEL）
|Dime |0.1 美元（DIME）
|Quarter |0.25 美元（QUARTER）
|Dollar |1 美元（ONE）
|Five Dollars |5 美元（五）
|Ten Dollars |10 美元（TEN）
|Twenty Dollars |20 美元（TWENTY）
|One-hundred Dollars |100美元（ONE HUNDRED）
下面是 \`cid\` 数组示例：

\`\`\` javascript
[
  ["PENNY", 1.01],
  ["NICKEL", 2.05],
  ["DIME", 3.1],
  ["QUARTER", 4.25],
  ["ONE", 90],
  ["FIVE", 55],
  ["TEN", 20],
  ["TWENTY", 60],
  ["ONE HUNDRED", 100]
]
\`\`\`

例：\`checkCashRegister(19.5, 20, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.1], ["QUARTER", 4.25], ["ONE", 90], ["FIVE", 55], ["TEN", 20], ["TWENTY", 60], ["ONE HUNDRED", 100]])\` 应返回 \`{status: "OPEN", change: [["QUARTER", 0.5]]}\`  
\`checkCashRegister(19.5, 20, [["PENNY", 0.01], ["NICKEL", 0], ["DIME", 0], ["QUARTER", 0], ["ONE", 0], ["FIVE", 0], ["TEN", 0], ["TWENTY", 0], ["ONE HUNDRED", 0]])\` 应返回 \`{status: "INSUFFICIENT_FUNDS", change: []}\`

\`\`\` javascript
function checkCashRegister(price, cash, cid) {
  let change=[];
  let changetotal=(cash-price)*100;
  let denomarr=[1, 5, 10, 25, 100, 500, 1000, 2000, 10000];
  let denomaname=[];
  let cidarr=[];
  for(let i=0;i<cid.length;i++){
    cidarr.push(Math.round(cid[i][1]*100));
    denomaname.push(cid[i][0])
  };
  let cidtotal=cidarr.reduce((sum,i)=>sum+i);
  if(changetotal>cidtotal){
    return {status: "INSUFFICIENT_FUNDS", change: []}
    };
  if(changetotal===cidtotal){
      return {status: "CLOSED", change: cid}
    };
  for(let i=denomarr.length-1;i>=0;i--){
    let currentneed=changetotal-changetotal%denomarr[i];
    if(currentneed>cidarr[i]&&cidarr[i]>0){
      change.push([denomaname[i],cidarr[i]/100])
      changetotal=changetotal-cidarr[i];
    }else if(currentneed<=cidarr[i]&&currentneed>0){
      change.push([denomaname[i],currentneed/100])
      changetotal=changetotal-currentneed;
    };
}
    if(changetotal===0){
      return {status: "OPEN", change:change}
      }else{
        return {status: "INSUFFICIENT_FUNDS", change: []}
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
