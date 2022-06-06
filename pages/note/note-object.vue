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
- 对象

我们可以通过使用带有可选 属性列表 的花括号 \`{…}\` 来创建对象。一个属性就是一个键值对（\`“key: value”\`），其中键（\`key\`）是一个字符串（也叫做属性名），值（\`value\`）可以是任何值。  
我们可以用下面两种语法中的任一种来创建一个空的对象:

\`\`\` javascript
let user = new Object(); // “构造函数” 的语法
let user = {};  // “字面量” 的语法
\`\`\`

通常，我们用花括号。这种方式我们叫做字面量。

- 方法

\`\`\` javascript
let user = {
  name: "John",
  age: 30
};
user.sayHi = function() {
  alert("Hello!");
};
user.sayHi(); // Hello!
\`\`\`

这里我们使用函数表达式创建了一个函数，并将其指定给对象的 \`user.sayHi\` 属性。随后我们像这样 \`user.sayHi()\` 调用它。  
作为对象属性的函数被称为 方法。

- 方法中的this

通常，对象方法需要访问对象中存储的信息才能完成其工作。
例如，\`user.sayHi()\` 中的代码可能需要用到 \`user\` 的 \`name\` 属性。  
为了访问该对象，方法中可以使用 \`this\` 关键字。  
\`this\` 的值就是在点之前的这个对象，即调用该方法的对象。

\`\`\` javascript
let user = {
  name: "John",
  age: 30,
  sayHi() {
    // "this" 指的是“当前的对象”
    alert(this.name);
  }
};
user.sayHi(); // John
\`\`\`

- 构造器

构造函数在技术上是常规函数。不过有两个约定：  
它们的命名以大写字母开头。  
它们只能由 "\`new\`" 操作符来执行。  

\`\`\` javascript
function User(name) {
  this.name = name;
  this.isAdmin = false;
}
let user = new User("Jack");
\`\`\`

这和使用字面量创建是一样的：

\`\`\` javascript
let user = {
  name: "Jack",
  isAdmin: false
};
\`\`\`

构造器的主要目的 —— 实现可重用的对象创建代码。
让我们再强调一遍 —— 从技术上讲，任何函数（除了箭头函数，它没有自己的 this）都可以用作构造器。即可以通过 new 来运行，它会执行上面的算法。“首字母大写”是一个共同的约定，以明确表示一个函数将被使用 new 来运行。

- 使用 instanceof 验证对象的构造函数

\`object instanceof constructor\` 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。返回值是返回 true 或者 false。

- 自有属性和原型属性

直接在实例对象上定义的属性叫自有属性（own properties）；  
\`prototype\` 是一个可以在所有\`构造函数\`的实例之间共享的对象。  
JavaScript 中几乎所有的对象都有一个 prototype 属性，这个属性是属于它所在的构造函数。

- 迭代所有属性

\`\`\` javascript
function Bird(name) {
  this.name = name;  //own property
}
Bird.prototype.numLegs = 2; // prototype property
let duck = new Bird("Donald");

let ownProps = [];
let prototypeProps = [];
for (let property in duck) {
  if(duck.hasOwnProperty(property)) {
    ownProps.push(property);
  } else {
    prototypeProps.push(property);
  }
}

console.log(ownProps);   //["name"]
console.log(prototypeProps);  //["numLegs"]
\`\`\`

- 构造函数属性

\`Object.prototype.constructor\` 返回创建实例对象的 Object 构造函数的引用。注意，此属性的值是对函数本身的引用，而不是一个包含函数名称的字符串。

- 将原型更改为新对象

到目前为止，你已经可以单独给 prototype 添加属性了：

\`\`\` javascript
Bird.prototype.numLegs = 2;
\`\`\`

需要添加多个属性的，这未免会显得拖沓。

\`\`\` javascript
Bird.prototype.eat = function() {
  console.log("nom nom nom");
}
Bird.prototype.describe = function() {
  console.log("My name is " + this.name);
}
\`\`\`

一种更有效的方法就是给对象的 prototype 设置为一个已经包含了属性的新对象。 这样一来，所有属性都可以一次性添加进来：

\`\`\` javascript
Bird.prototype = {
  numLegs: 2, 
  eat: function() {
    console.log("nom nom nom");
  },
  describe: function() {
    console.log("My name is " + this.name);
  }
};
\`\`\`

- 更改原型时，记得设置构造函数属性

手动设置一个新对象的原型有一个重要的副作用。 它清除了 constructor 属性！  
此属性可以用来检查是哪个构造函数创建了实例，但由于该属性已被覆盖，它现在给出了错误的结果：

\`\`\` javascript
duck.constructor === Bird;   //false
duck.constructor === Object;   //true
duck instanceof Bird;   //true
\`\`\`

为了解决这个问题，凡是手动给新对象重新设置过原型对象的，都别忘记在原型对象中定义一个 constructor 属性:

\`\`\` javascript
Bird.prototype = {
  constructor: Bird,
  numLegs: 2,
  eat: function() {
    console.log("nom nom nom");
  },
  describe: function() {
    console.log("My name is " + this.name); 
  }
};
\`\`\`

- prototypeObj.isPrototypeOf(object)

\`isPrototypeOf()\` 方法用于测试一个对象是否存在于另一个对象的原型链上。
>isPrototypeOf() 与 instanceof 运算符不同。在表达式 "object instanceof AFunction"中，object 的原型链是针对 AFunction.prototype 进行检查的，而不是针对 AFunction 本身。

\`\`\` javascript
function Bird(name) {
  this.name = name;
}
let duck = new Bird("Donald");
Bird.prototype.isPrototypeOf(duck);  //true
\`\`\`

duck 从 Bird 构造函数那里继承了它的 prototype。

- 原型链

JavaScript 中所有的对象（除了少数例外）都有自己的 prototype。 而且，对象的 prototype 本身也是一个对象。所以 prototype 对象也有它自己的 prototype。  
\`supertype\` 超类或父类，\`subtype\`亚类或子类。  
Object 是 JavaScript 中所有对象的 supertype，也就是原型链的最顶层。 因此，所有对象都可以访问 \`Object.prototype\` 上的方法，如\`hasOwnProperty\`。

- 从超类继承

我们创建了一个Animal 超类（supertype），用来定义所有动物共有的行为：

\`\`\` javascript
function Animal() { }
Animal.prototype.eat = function() {
  console.log("nom nom nom");
};
\`\`\`

第一步：创建一个超类 supertype（或者叫父类）的实例。

\`\`\` javascript
let animal = Object.create(Animal.prototype);
\`\`\`

\`Object.create(obj)\` 创建了一个新对象，并指定了 obj 作为新对象的 prototype。  
第二步：给子类设置 prototype。

\`\`\` javascript
Bird.prototype = Object.create(Animal.prototype);
\`\`\`

这样一来，Bird 就是 Animal 的一个实例了。

- 重置一个继承的构造函数属性

当一个对象从另一个对象那里继承了其 prototype 时，那它也继承了父类的 constructor 属性。

\`\`\` javascript
function Bird() { }
Bird.prototype = Object.create(Animal.prototype);
let duck = new Bird();
duck.constructor
\`\`\`

但是 duck 和其他所有 Bird 的实例都应该表明它们是由 Bird 创建的，而不是由 Animal 创建的。 为此，你可以手动将 Bird 的构造函数属性设置为 Bird 对象：

\`\`\` javascript
Bird.prototype.constructor = Bird;
duck.constructor
\`\`\`

- 继承后添加方法

\`\`\` javascript
function Animal() { }
Animal.prototype.eat = function() {
  console.log("nom nom nom");
};
function Bird() { }
Bird.prototype = Object.create(Animal.prototype);
Bird.prototype.constructor = Bird;
\`\`\`

除了从 Animal 构造函数继承的行为之外，还需要给 Bird 对象添加它独有的行为。 这里，我们给 Bird 对象添加一个 fly() 函数。 函数会以一种与其他构造函数相同的方式添加到 Bird's 的 prototype 中：

\`\`\` javascript
Bird.prototype.fly = function() {
  console.log("I'm flying!");
};
\`\`\`

- 重写继承的方法

我们学习了一个对象可以通过引用另一个对象的 prototype 来继承其属性和行为（或方法）：

\`\`\` javascript
ChildObject.prototype = Object.create(ParentObject.prototype);
\`\`\`

然后，ChildObject 将自己的方法链接到它的 prototype中：

\`\`\` javascript
ChildObject.prototype.methodName = function() {...};
\`\`\`

我们还可以重写继承的方法。 以同样的方式 - 通过使用一个与需要重写的方法相同的方法名，向\`ChildObject.prototype\` 中添加方法。

\`\`\` javascript
function Animal() { }
Animal.prototype.eat = function() {
  return "nom nom nom";
};
function Bird() { }
Bird.prototype = Object.create(Animal.prototype);
Bird.prototype.eat = function() {
  return "peck peck peck";
};
\`\`\`

如果你有一个实例：\`let duck = new Bird()\`;，然后你调用了 \`duck.eat()\`，以下就是 JavaScript 在 duck 的 prototype 链上寻找方法的过程：
duck => eat() 是定义在这里吗？ 不是。
Bird => eat() 是定义在这里吗？ => 是的。 执行它并停止往上搜索。
Animal => 这里也定义了 eat() 方法，但是 JavaScript 在到达这层原型链之前已停止了搜索。
Object => JavaScript 在到达这层原型链之前也已经停止了搜索。

- 使用 Mixin 在不相关对象之间添加共同行为

正如你所见，行为是可以通过继承来共享的。 然而，在有些情况下，继承不是最好的解决方案。 继承不适用于不相关的对象，比如 Bird 和 Airplane。 虽然它们都可以飞行，但是 Bird 并不是一种 Airplane，反之亦然。  
对于不相关的对象，更好的方法是使用 mixins。 mixin 允许其他对象使用函数集合。

\`\`\` javascript
let flyMixin = function(obj) {
  obj.fly = function() {
    console.log("Flying, wooosh!");
  }
};
\`\`\`

flyMixin 能接受任何对象，并为其提供 fly 方法。

\`\`\` javascript
let bird = {
  name: "Donald",
  numLegs: 2
};
let plane = {
  model: "777",
  numPassengers: 524
};
flyMixin(bird);
flyMixin(plane);
\`\`\`

里的 flyMixin 接收了bird 和 plane 对象，然后将 fly 方法分配给了每一个对象。 现在 bird 和 plane 都可以飞行了：

\`\`\` javascript
bird.fly();
plane.fly();
\`\`\`

- 使用闭包保护对象内的属性不被外部修改

在上一次挑战中，bird 有一个公共属性 name。 公共属性的定义就是：它可以在 bird 的定义范围之外被访问和更改。  
因此，代码的任何地方都可以轻松地将 bird 的 name 属性更改为任意值。使属性私有化最简单的方法就是在构造函数中创建变量。 可以将该变量范围限定在构造函数中，而不是全局可用。 这样，属性只能由构造函数中的方法访问和更改。

\`\`\` javascript
function Bird() {
  let hatchedEgg = 10;
  this.getHatchedEggCount = function() { 
    return hatchedEgg;
  };
}
let ducky = new Bird();
ducky.getHatchedEggCount();
\`\`\`

这里的 \`getHatchedEggCount\` 是一种特权方法，因为它可以访问私有属性 \`hatchedEgg\`。 这是因为 \`hatchedEgg\` 是在与 \`getHatchedEggCount\` 相同的上下文中声明的。 在 JavaScript 中，函数总是可以访问创建它的上下文。 这就叫做 闭包(closure)。

- 立即调用函数表达（IIFE）

函数没有名称，也不存储在变量中。 函数表达式末尾的两个括号（）会让它被立即执行或调用。 这种模式被叫做立即调用函数表达式（immediately invoked function expression) 。

\`\`\` javascript
(function () {
  console.log("Chirp, chirp!");
})();
\`\`\`

一个 IIFE 通常用于将相关功能分组到单个对象或者是 module 中。例如，先前的挑战中定义了两个 mixins：

\`\`\` javascript
function glideMixin(obj) {
  obj.glide = function() {
    console.log("Gliding on the water");
  };
}
function flyMixin(obj) {
  obj.fly = function() {
    console.log("Flying, wooosh!");
  };
}
\`\`\`

我们可以将这些 mixins 分成以下模块：

\`\`\` javascript
let motionModule = (function () {
  return {
    glideMixin: function(obj) {
      obj.glide = function() {
        console.log("Gliding on the water");
      };
    },
    flyMixin: function(obj) {
      obj.fly = function() {
        console.log("Flying, wooosh!");
      };
    }
  }
})();
\`\`\`

一个 IIFE 返回了一个 motionModule 对象。 返回的这个对象包含了作为对象属性的所有 mixin 行为。 module 模式的优点是，所有的运动相关的行为都可以打包成一个对象，然后由代码的其他部分使用。 下面是一个使用它的例子：

\`\`\` javascript
motionModule.glideMixin(duck);
duck.glide();
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
