> 提示：这是我对这个 https://github.com/loverajoel/jstips 项目的翻译版

![header](https://raw.githubusercontent.com/loverajoel/jstips/master/resources/jstips-header-blog.gif)

# JavaScript 技巧介绍

> 新年新项目，每天介绍一个 JS 小技巧

怀着激动的心情, 我来介绍这些可以提高你的代码质量的短小但有用的Javascript小技巧. 每天花费不到两分钟, 你就可以了解到有关性能,习惯,hacks,面试问题以及这个超棒语言提给我们的所有东西.

无论工作日或节假日，都将有一条小知识发布。

### 你能帮我们丰富它吗?

请尽情将你们自己的JavaScript小知识向我们提交PR吧。我们欢迎任何意见和建议！
[点这里查看操作说明](https://github.com/loverajoel/jstips/blob/master/CONTRIBUTING.md)

### 让我们保持联系
想要获取更新, watch 本仓库并关注[Twitter 账户](https://twitter.com/tips_js),每天只有一条推文发布，这永远不会变!
> 不要忘了Star我们的仓库,这将帮助我们提高此项目!

# 技巧列表

## #27 JS 中的短路求值

短路求值是说, 只有当第一个运算数的值无法确定逻辑运算的结果时，才对第二个运算数进行求值：当AND(&&)的第一个运算数的值为false时，其结果必定为false；当OR(||)的第一个运算数为true时，最后结果必定为true。
对于下面的test条件和isTrue与isFalse方法

```
var test = true;
var isTrue = function(){
  console.log('Test is true.');
};
var isFalse = function(){
  console.log('Test is false.');
};
```

使用逻辑与 - &&.

```
// 普通的if语句
if(test){
  isTrue();    // Test is true
}
```

// 上面的语句可以使用 '&&' 写为：

```
( test && isTrue() );  // Test is true
使用逻辑或 - ||.
test = false;
if(!test){
  isFalse();    // Test is false.
}

( test || isFalse());  // Test is false.
```

逻辑或可以用来给参数设置默认值。

```
function theSameOldFoo(name){
    name = name || 'Bar' ;
    console.log("My best friend's name is " + name);
}
theSameOldFoo();  // My best friend's name is Bar
theSameOldFoo('Bhaskar');  // My best friend's name is Bhaskar
```

逻辑与可以用来避免调用undefined参数的属性时报错 例如:-
```
var dog = {
  bark: function(){
     console.log('Woof Woof');
   }
};
```

```
// 调用 dog.bark();
dog.bark(); // Woof Woof.
```
// 但是当dog未定义时，dog.bark() 将会抛出"Cannot read property 'bark' of undefined." 错误
// 防止这种情况，我们可以使用 &&.

```
dog&&dog.bark();   // This will only call dog.bark(), if dog is defined.
```

## #26 过滤并排序字符串列表
你可能有一个很多名字组成的列表，需要过滤掉重复的名字并按字母表将其排序。
在我们的例子里准备用不同版本语言的JavaScript 保留字的列表，但是你能发现，有很多重复的关键字而且它们并没有按字母表顺序排列。所以这是一个完美的字符串列表(数组)来测试我们的JavaScript小知识。

```
var keywords = ['do', 'if', 'in', 'for', 'new', 'try', 'var', 'case', 'else', 'enum', 'null', 'this', 'true', 'void', 'with', 'break', 'catch', 'class', 'const', 'false', 'super', 'throw', 'while', 'delete', 'export', 'import', 'return', 'switch', 'typeof', 'default', 'extends', 'finally', 'continue', 'debugger', 'function', 'do', 'if', 'in', 'for', 'int', 'new', 'try', 'var', 'byte', 'case', 'char', 'else', 'enum', 'goto', 'long', 'null', 'this', 'true', 'void', 'with', 'break', 'catch', 'class', 'const', 'false', 'final', 'float', 'short', 'super', 'throw', 'while', 'delete', 'double', 'export', 'import', 'native', 'public', 'return', 'static', 'switch', 'throws', 'typeof', 'boolean', 'default', 'extends', 'finally', 'package', 'private', 'abstract', 'continue', 'debugger', 'function', 'volatile', 'interface', 'protected', 'transient', 'implements', 'instanceof', 'synchronized', 'do', 'if', 'in', 'for', 'let', 'new', 'try', 'var', 'case', 'else', 'enum', 'eval', 'null', 'this', 'true', 'void', 'with', 'break', 'catch', 'class', 'const', 'false', 'super', 'throw', 'while', 'yield', 'delete', 'export', 'import', 'public', 'return', 'static', 'switch', 'typeof', 'default', 'extends', 'finally', 'package', 'private', 'continue', 'debugger', 'function', 'arguments', 'interface', 'protected', 'implements', 'instanceof', 'do', 'if', 'in', 'for', 'let', 'new', 'try', 'var', 'case', 'else', 'enum', 'eval', 'null', 'this', 'true', 'void', 'with', 'await', 'break', 'catch', 'class', 'const', 'false', 'super', 'throw', 'while', 'yield', 'delete', 'export', 'import', 'public', 'return', 'static', 'switch', 'typeof', 'default', 'extends', 'finally', 'package', 'private', 'continue', 'debugger', 'function', 'arguments', 'interface', 'protected', 'implements', 'instanceof'];
```

因为我们不想改变我们的原始列表，所以我们准备用高阶函数叫做filter，它将基于我们传递的回调方法返回一个新的过滤后的数组。回调方法将比较当前关键字在原始列表里的索引和新列表中的索引，仅当索引匹配时将当前关键字push到新数组。
最后我们准备使用sort方法排序过滤后的列表，sort只接受一个比较方法作为参数，并返回按字母表排序后的列表。

```
var filteredAndSortedKeywords = keywords
  .filter(function (keyword, index) {
      return keywords.lastIndexOf(keyword) === index;
    })
  .sort(function (a, b) {
      return a < b ? -1 : 1;
    });
    
```

在ES6 (ECMAScript 2015)版本下使用箭头函数看起来更简单:

```
const filteredAndSortedKeywords = keywords
  .filter((keyword, index) => keywords.lastIndexOf(keyword) === index)
  .sort((a, b) => a < b ? -1 : 1);
 ```
 
这是最后过滤和排序后的JavaScript保留字列表：

```
console.log(filteredAndSortedKeywords);

// ['abstract', 'arguments', 'await', 'boolean', 'break', 'byte', 'case', 'catch', 'char', 'class', 'const', 'continue', 'debugger', 'default', 'delete', 'do', 'double', 'else', 'enum', 'eval', 'export', 'extends', 'false', 'final', 'finally', 'float', 'for', 'function', 'goto', 'if', 'implements', 'import', 'in', 'instanceof', 'int', 'interface', 'let', 'long', 'native', 'new', 'null', 'package', 'private', 'protected', 'public', 'return', 'short', 'static', 'super', 'switch', 'synchronized', 'this', 'throw', 'throws', 'transient', 'true', 'try', 'typeof', 'var', 'void', 'volatile', 'while', 'with', 'yield']
```

## #25 使用立即执行函数表达式

立即执行函数表达式( IIFE - immediately invoked function expression)是一个立即执行的匿名函数表达式，它在Javascript中有一些很重要的用途。

```
(function() {
 // Do something​
 }
)()
```


这是一个立即执行的匿名函数表达式，它在有JavaScript一些特别重要的用途。
两对括号包裹着一个匿名函数，使匿名函数变成了一个函数表达式。于是，我们现在拥有了一个未命名的函数表达式，而不是一个全局作用域下或在任何地方定义的的简单函数。
类似地，我们也可以创建一个命名过的立即执行函数表达式：

```
(someNamedFunction = function(msg) {
	console.log(msg || "Nothing for today !!")
	}) (); // 输出 --> Nothing for today !!​
​
someNamedFunction("Javascript rocks !!"); // 输出 --> Javascript rocks !!
someNamedFunction(); // 输出 --> Nothing for today !!​
```

## #24 - 使用 === 而不是 ==
== (或者 !=) 操作在需要的情况下自动进行了类型转换。=== (或 !==)操作不会执行任何转换。===在比较值和类型时，可以说比==更快

```
[10] ==  10      // 为 true
[10] === 10      // 为 false

'10' ==  10      // 为 true
'10' === 10      // 为 false

 []  ==  0       // 为 true
 []  === 0       // 为 false

 ''  ==  false   // 为 true 但 true == "a" 为false
 ''  === false   // 为 false 
 ```

## #23 - 转换为数字的更快方法([原文](https://github.com/loverajoel/jstips#23---converting-to-number-fast-way))

> 2016-01-23 by [@sonnyt](http://twitter.com/sonnyt)

将字符串转换为数字是极为常见的。最简单和快速的方法([jsPref](https://jsperf.com/number-vs-parseint-vs-plus/29))`+`(加号) 来实现。

```javascript
var one = '1';

var numberOne = +one; // Number 1
```

你也可以用`-`(减号)将其转化为负数值。

```javascript
var one = '1';

var negativeNumberOne = -one; // Number -1
```

## #22 - 清空数组([原文](https://github.com/loverajoel/jstips#22---empty-an-array))

> 2016-01-22 by [microlv](https://github.com/microlv)

如果你定义了一个数组，然后你想清空它。
通常，你会这样做：

```javascript
// 定义一个数组
var list = [1, 2, 3, 4];
function empty() {
    //清空数组
    list = [];
}
empty();
```
但是，这有一个效率更高的方法来清空数组。
你可以这样写:
```javascript
var list = [1, 2, 3, 4];
function empty() {
    //empty your array
    list.length = 0;
}
empty();
```
* `list = []` 将一个新的数组的引用赋值给变量，其他引用并不受影响。
这意味着以前数组的内容被引用的话将依旧存在于内存中，这将导致内存泄漏。

* `list.length = 0` 删除数组里的所有内容，也将影响到其他引用。

然而，如果你复制了一个数组（A 和 Copy-A），如果你用`list.length = 0`清空了它的内容，复制的数组也会清空它的内容。

考虑一下将会输出什么：
```js
var foo = [1,2,3];
var bar = [1,2,3];
var foo2 = foo;
var bar2 = bar;
foo = [];
bar.length = 0;
console.log(foo, bar, foo2, bar2);

//[] [] [1, 2, 3] []
```
更多内容请看Stackoverflow：
[difference-between-array-length-0-and-array](http://stackoverflow.com/questions/4804235/difference-between-array-length-0-and-array)


## #21 - 对数组洗牌([原文](https://github.com/loverajoel/jstips#21---shuffle-an-array))

> 2016-01-21 by [@0xmtn](https://github.com/0xmtn/)

 这段代码运用了[Fisher-Yates Shuffling](https://www.wikiwand.com/en/Fisher%E2%80%93Yates_shuffle)算法对数组进行洗牌。
  
```javascript
function shuffle(arr) {
    var i, 
        j,
        temp;
    for (i = arr.length - 1; i > 0; i--) {
        j = Math.floor(Math.random() * (i + 1));
        temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    return arr;    
};
```
调用示例:

```javascript
var a = [1, 2, 3, 4, 5, 6, 7, 8];
var b = shuffle(a);
console.log(b);
// [2, 7, 8, 6, 5, 3, 1, 4]
```


## #20 - 返回对象，使方法可以链式调用([原文](https://github.com/loverajoel/jstips#20---return-objects-to-enable-chaining-of-functions))

> 2016-01-20 by [@WakeskaterX](https://twitter.com/WakeStudio)

在面向对象的Javascript中为对象建立一个方法时，返回当前对象可以让你在一条链上调用方法。

```js
function Person(name) {
  this.name = name;

  this.sayName = function() {
    console.log("Hello my name is: ", this.name);
    return this;
  };

  this.changeName = function(name) {
    this.name = name;
    return this;
  };
}

var person = new Person("John");
person.sayName().changeName("Timmy").sayName();
```


## #19 - 安全的字符串拼接([原文](https://github.com/loverajoel/jstips#19---safe-string-concatenation))

> 2016-01-19 by [@gogainda](https://twitter.com/gogainda)

假如你需要拼接一些不确定类型的变量为字符串，你需要确保算术运算符在你拼接时不会起作用。

```javascript
var one = 1;
var two = 2;
var three = '3';

var result = ''.concat(one, two, three); //"123"
```

这应该就是你所期望的拼接结果。如果不这样，拼接时加号可能会导致你意想不到的结果：

```javascript
var one = 1;
var two = 2;
var three = '3';

var result = one + two + three; //"33" instead of "123"
```

关于性能,与用[```join```](http://www.sitepoint.com/javascript-fast-string-concatenation/)来拼接字符串相比 ```concat```的效率是几乎一样的。

你可以在[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)了解更多关于```concat```方法的内容。

## #18 - 更快的取整([原文](https://github.com/loverajoel/jstips#18---rounding-the-fast-way))

> 2016-01-18 by [pklinger](https://github.com/pklinger)

今天的小知识有关性能表现。[曾经用过双波浪](http://stackoverflow.com/questions/5971645/what-is-the-double-tilde-operator-in-javascript) `~~` 操作符吗? 有时候也称为双非(NOT)位操作. 你可以用它作为`Math.floor()`的替代方法。为什么会这样呢?

一个位操作符 `~` 将输入的32位的数字(input)转换为 `-(input+1)`. 两个位操作符将输入(input)转变为 `-(-(input + 1)+1)` 是一个使结果趋向于0的取整好工具. 对于数字, 负数就像使用`Math.ceil()`方法而正数就像使用`Math.floor()`方法. 转换失败时,返回`0`,这在`Math.floor()`方法转换失败返回`NaN`时或许会派上用场。

```javascript
// 单个 ~
console.log(~1337)    // -1338

// 数字输入
console.log(~~47.11)  // -> 47
console.log(~~-12.88) // -> -12
console.log(~~1.9999) // -> 1
console.log(~~3)      // -> 3

// 转换失败
console.log(~~[]) // -> 0
console.log(~~NaN)  // -> 0
console.log(~~null) // -> 0

// 大于32位整数时转换失败
console.log(~~(2147483647 + 1) === (2147483647 + 1)) // -> 0
```

虽然`~~`的性能更好,为了代码的可读性请用`Math.floor()`.

## #17 - Node.js: 运行未被引用的模块([原文](https://github.com/loverajoel/jstips#17---nodejs-run-a-module-if-it-is-not-required))

> 2016-01-17 by [@odsdq](https://twitter.com/odsdq)

在Node里,你可以让你的程序根据其运行自`require('./something.js')`或者`node something.js`而做不同的处理。如果你想与你的一个独立的模块进行交互，这是非常有用的。

```js
if (!module.parent) {
    // 通过 `node something.js` 启动
    app.listen(8088, function() {
        console.log('app listening on port 8088');
    })
} else {
    // 通过 `require('/.something.js')` 被引用
    module.exports = app;
}
```

更多内容请看 [modules的文档](https://nodejs.org/api/modules.html#modules_module_parent)

## #16 - 向回调方法传递参数([原文](https://github.com/loverajoel/jstips#16---passing-arguments-to-callback-functions))
> 2016-01-16 by [@minhazav](https://twitter.com/minhazav)

通常下，你并不能给回调函数传递参数。 比如:
```js
function callback() {
  console.log('Hi human');
}

document.getElementById('someelem').addEventListener('click', callback);
```
你可以借助Javascript闭包的优势来传递参数给回调函数。看这个例子:
```js
function callback(a, b) {
  return function() {
    console.log('sum = ', (a+b));
  }
}

var x = 1, y = 2;
document.getElementById('someelem').addEventListener('click', callback(x, y));
```

**什么是闭包?**
闭包是指函数有自由独立的变量。换句话说，定义在闭包中的函数可以“记忆”它创建时候的环境。想了解更多请[参考MDN的文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)。

这种方法使参数`x`和`y`在回调方法被调用时处于其作用域内。

另一个办法是使用`bind`方法。比如:
```js
var alertText = function(text) {
  alert(text);
};

document.getElementById('someelem').addEventListener('click', alertText.bind(this, 'hello'));
```
两种方法之间有着微小的性能差异,请看[jsperf](http://jsperf.com/bind-vs-closure-23).

## #15 - 更简单的使用indexOf实现contains功能([原文](https://github.com/loverajoel/jstips#15---even-simpler-way-of-using-indexof-as-a-contains-clause))
> 2016-01-15 by [@jhogoforbroke](https://twitter.com/jhogoforbroke)

JavaScript并未提供contains方法。检测子字符串是否存在于字符串或者变量是否存在于数组你可能会这样做：

```javascript
var someText = 'javascript rules';
if (someText.indexOf('javascript') !== -1) {
}

// or
if (someText.indexOf('javascript') >= 0) {
}
```

但是让我们看一下这些 [Expressjs](https://github.com/strongloop/express)代码段。

[examples/mvc/lib/boot.js](https://github.com/strongloop/express/blob/2f8ac6726fa20ab5b4a05c112c886752868ac8ce/examples/mvc/lib/boot.js#L26)
```javascript
for (var key in obj) {
  // "reserved" exports
  if (~['name', 'prefix', 'engine', 'before'].indexOf(key)) continue;
```

[lib/utils.js](https://github.com/strongloop/express/blob/2f8ac6726fa20ab5b4a05c112c886752868ac8ce/lib/utils.js#L93)
```javascript
exports.normalizeType = function(type){
  return ~type.indexOf('/')
    ? acceptParams(type)
    : { value: mime.lookup(type), params: {} };
};
```

[examples/web-service/index.js](https://github.com/strongloop/express/blob/2f8ac6726fa20ab5b4a05c112c886752868ac8ce/examples/web-service/index.js#L35)
```javascript
// key is invalid
if (!~apiKeys.indexOf(key)) return next(error(401, 'invalid api key'));
```

难点是 [位操作符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators) **~**, “按位操作符操作数字的二进制形式，但是返回值依然是标准的JavaScript数值。”

它将`-1`转换为`0`,而`0`在javascript为`false`,所以:

```javascript
var someText = 'text';
!!~someText.indexOf('tex'); // someText contains "tex" - true
!~someText.indexOf('tex'); // someText NOT contains "tex" - false
~someText.indexOf('asd'); // someText doesn't contain "asd" - false
~someText.indexOf('ext'); // someText contains "ext" - true
```

### String.prototype.includes()

在ES6中提供了[includes() 方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/includes)供我们判断一个字符串是否包含了另一个字符串:

```javascript
'something'.includes('thing'); // true
```

在ECMAScript 2016 (ES7)甚至可能将其应用于数组，像indexOf一样:

```javascript
!!~[1, 2, 3].indexOf(1); // true
[1, 2, 3].includes(1); // true
```

**不幸的是, 只有Chrome、Firefox、Safari 9及其更高版本和Edge支持了这功能。IE11及其更低版本并不支持**
**最好在受控的环境中使用此功能**

## #14 - 箭头函数 #ES6([原文](https://github.com/loverajoel/jstips#14---fat-arrow-functions-es6))
> 2016-01-13 by [@pklinger](https://github.com/pklinger/)

介绍一个ES6的新特性，箭头函数或许一个让你用更少行写更多代码的方便工具。它的名字(fat arrow functions)来自于它的语法`=>`是一个比瘦箭头`->`要'胖的箭头'（译者注：但是国内貌似不分胖瘦就叫箭头函数）。Some programmers might already know this type of functions from different languages such as Haskell as 'lambda expressions' respectively 'anonymous functions'. It is called anonymous, as these arrow functions do not have a descriptive function name.（译者注：一些其他语言中的箭头函数，避免不准确就不翻译了 欢迎PR）

### 有什么益处呢?
* 语法: 更少的代码行; 不再需要一遍一遍的打`function`了
* 语义: 从上下文中捕获`this`关键字

### 简单的语法示例
观察一下这两个功能完全相同的代码片段。你将很快明白箭头函数做了什么。

```javascript
// 箭头函数的一般语法
param => expression

// 也可以用用小括号
// 多参数时小括号是必须的
(param1 [, param2]) => expression


// 使用functions
var arr = [5,3,2,9,1];
var arrFunc = arr.map(function(x) {
  return x * x;
});
console.log(arr)

// 使用箭头函数
var arr = [5,3,2,9,1];
var arrFunc = arr.map((x) => x*x);
console.log(arr)
```

正如你所看到的，箭头函数在这种情况下省去了写小括号，function以及return的时间。我建议你总是使用小括号，因为对于像`(x,y) => x+y`这样多参数函数，小括号总是需要的。这仅是以防在不同使用场景下忘记小括号的一种方法。但是上面的代码和`x => x*x`是一样的。至此仅是语法上的提升，减少了代码行数并提高了可读性。

### Lexically binding `this`

这是另一个使用箭头函数的好原因。这是一个关于`this`上下文的问题。使用箭头函数，你不需要再担心`.bind(this)`也不用再设置`that = this`了，因为箭头函数继承了外围作用域的`this`值。看一下下面的[示例](https://jsfiddle.net/pklinger/rw94oc11/):

```javascript

// 全局定义 this.i
this.i = 100;

var counterA = new CounterA();
var counterB = new CounterB();
var counterC = new CounterC();
var counterD = new CounterD();

// 不好的例子
function CounterA() {
  // CounterA的`this`实例 (!!调用时忽略了此实例)
  this.i = 0;
  setInterval(function () {
    // `this` 指向全局(global)对象,而不是CounterA的`this`
    // 所以从100开始计数,而不是0 (本地的this.i)
    this.i++;
    document.getElementById("counterA").innerHTML = this.i;
  }, 500);
}

// 手动绑定 that = this
function CounterB() {
  this.i = 0;
  var that = this;
  setInterval(function() {
    that.i++;
    document.getElementById("counterB").innerHTML = that.i;
  }, 500);
}

// 使用 .bind(this)
function CounterC() {
  this.i = 0;
  setInterval(function() {
    this.i++;
    document.getElementById("counterC").innerHTML = this.i;
  }.bind(this), 500);
}

// 箭头函数
function CounterD() {
  this.i = 0;
  setInterval(() => {
    this.i++;
    document.getElementById("counterD").innerHTML = this.i;
  }, 500);
}
```

更多有关箭头函数的内容可以查看[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)。更多语法选项请看[这里](http://jsrocks.org/2014/10/arrow-functions-and-their-scope/).


## #13 - 测量javascript代码块性能的小知识([原文](https://github.com/loverajoel/jstips#13---tip-to-measure-performance-of-a-javascript-block))

> 2016-01-13 by [@manmadareddy](https://twitter.com/manmadareddy)

快速的测量javascript的性能，我们可以使用console的方法，例如
[```console.time(label)```](https://developer.chrome.com/devtools/docs/console-api#consoletimelabel) 和 [```console.timeEnd(label)```](https://developer.chrome.com/devtools/docs/console-api#consoletimeendlabel)


```javascript
console.time("Array initialize");
var arr = new Array(100),
    len = arr.length,
    i;

for (i = 0; i < len; i++) {
    arr[i] = new Object();
};
console.timeEnd("Array initialize"); // Outputs: Array initialize: 0.711ms
```


更多内容:
[Console object](https://github.com/DeveloperToolsWG/console-object),
[Javascript benchmarking](https://mathiasbynens.be/notes/javascript-benchmarking)

Demo: [jsfiddle](https://jsfiddle.net/meottb62/) - [codepen](http://codepen.io/anon/pen/JGJPoa) (在浏览器控制台输出)


## #12 - ES6中的伪强制参数 #ES6([原文](https://github.com/loverajoel/jstips#12---pseudomandatory-parameters-in-es6-functions-es6))

> 2016-01-12 by [Avraam Mavridis](https://github.com/AvraamMavridis)



在许多编程语言中，方法的参数时默认强制需要的，开发人员需要明确定义一个可选的参数。在Javascript中任何参数都是可选的，但是我们可以利用[**es6参数默认值**](http://exploringjs.com/es6/ch_parameter-handling.html#sec_parameter-default-values)特性的优点来实现强制要求这种表现而不污染本身的函数体。

```javascript
const _err = function( message ){
  throw new Error( message );
}

const getSum = (a = _err('a is not defined'), b = _err('b is not defined')) => a + b

getSum( 10 ) // throws Error, b is not defined
getSum( undefined, 10 ) // throws Error, a is not defined
 ```



 `_err`方法会立即抛出一个错误。如果没有传递值给参数，默认值将会被使用, `_err`方法将被调用而错误也将被抛出。你可以从[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Default_parameters)看到更多关于**默认参数特性**的例子。 


## #11 - 变量提升([原文](https://github.com/loverajoel/jstips#11---hoisting))
> 2016-01-11 by [@squizzleflip](https://twitter.com/squizzleflip)

了解[变量提升](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/var#var_hoisting)可以帮助你组织方法作用域。只要记住变量生命和方法生命都会被提升到顶部。变量的定义不会提升，即使你在同一行声明和定义一个变量。变量**声明**是让系统知道有这个变量存在而**定义**是给其赋值。

```javascript
function doTheThing() {
  // ReferenceError: notDeclared is not defined
  console.log(notDeclared);

  // Outputs: undefined
  console.log(definedLater);
  var definedLater;

  definedLater = 'I am defined!'
  // Outputs: 'I am defined!'
  console.log(definedLater)

  // Outputs: undefined
  console.log(definedSimulateneously);
  var definedSimulateneously = 'I am defined!'
  // Outputs: 'I am defined!'
  console.log(definedSimulateneously)

  // Outputs: 'I did it!'
  doSomethingElse();

  function doSomethingElse(){
    console.log('I did it!');
  }

  // TypeError: undefined is not a function
  functionVar();

  var functionVar = function(){
    console.log('I did it!');
  }
}
```


为了让你的代码更易读，将所有的变量声明在函数的顶端，这样可以更清楚的知道变量来自哪个作用域。在使用变量之前声明变量。将方法定义在函数的底部。

## #10 - 检查某对象是否有某属性([原文](https://github.com/loverajoel/jstips#10---check-if-a-property-is-in-a-object))

> 2016-01-10 by [@loverajoel](https://www.twitter.com/loverajoel)

当你需要检查某属性是否存在于一个[对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Working_with_Objects)，你可能会这样做：

```javascript
var myObject = {
  name: '@tips_js'
};


if (myObject.name) { ... }

```

这是可以的，但是你需要知道有两种原生方法可以解决此类问题。[`in` 操作符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/in) 和 [`Object.hasOwnProperty`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)，任何继承自`Object`的对象都可以使用这两种方法。

### 看一下较大的区别

```javascript
var myObject = {
  name: '@tips_js'
};

myObject.hasOwnProperty('name'); // true
'name' in myObject; // true


myObject.hasOwnProperty('valueOf'); // false, valueOf 继承自原型链
'valueOf' in myObject; // true

```


两者检查属性的深度不同，换言之`hasOwnProperty`只在本身有此属性时返回true,而`in`操作符不区分属性来自于本身或继承自原型链。

这是另一个例子

```javascript
var myFunc = function() {
  this.name = '@tips_js';
};
myFunc.prototype.age = '10 days';

var user = new myFunc();

user.hasOwnProperty('name'); // true

user.hasOwnProperty('age'); // false, 因为age来自于原型链
```

[在线示例](https://jsbin.com/tecoqa/edit?js,console)!

同样建议阅读关于检查对象是否包含属性时常见错误的[讨论](https://github.com/loverajoel/jstips/issues/62)。

## #09 - 模板字符串

> 2016-01-09 by [@JakeRawr](https://github.com/JakeRawr)([原文](https://github.com/loverajoel/jstips#09---template-strings))

ES6中，JS现在有了引号拼接字符串的替代品，模板字符串。

示例:
普通字符串
```javascript
var firstName = 'Jake';
var lastName = 'Rawr';
console.log('My name is ' + firstName + ' ' + lastName);
// My name is Jake Rawr
```

模板字符串
```javascript
var firstName = 'Jake';
var lastName = 'Rawr';
console.log(`My name is ${firstName} ${lastName}`);
// My name is Jake Rawr
```


在模板字符串中，你可以不用`\n`来生成多行字符串还可以在`${}`里做简单的逻辑运算（例如 2+3）。

你也可以使用方法修改模板字符串的输出内容；他们被称为[带标签的模板字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/template_strings#Tagged_template_strings)（其中有带标签的模板字符串的示例）

或许你还想[阅读更多内容](https://hacks.mozilla.org/2015/05/es6-in-depth-template-strings-2)来了解模板字符串。

## #08 - 将Node List转换为数组(Array)([原文](https://github.com/loverajoel/jstips#08---converting-a-node-list-to-an-array))

> 2016-01-08 by [@Tevko](https://twitter.com/tevko)

`querySelectorAll`方法返回一个类数组对象称为node list。这些数据结构被称为“类数组”，因为他们看似数组却没有类似`map`、`foreach`这样的数组方法。这是一个快速、安全、可重用的方法将node list转换为DOM元素的数组：

```javascript
const nodelist = document.querySelectorAll('div');
const nodelistToArray = Array.apply(null, nodelist);


//之后 ..

nodelistToArray.forEach(...);
nodelistToArray.map(...);
nodelistToArray.slice(...);


//等...
```

`apply`方法可以在指定`this`时以数组形式向方法传递参数。[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)规定`apply`可以接受类数组对象,恰巧就是`querySelectorAll`方法所返回的内容。如果我们不需要指定方法内`this`的值传`null`或`0`即可。返回的结果即包含所有数组方法的DOM元素数组。

如果你正在用ES2015你可以使用[展开运算符 `...`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Spread_operator)

```js
const nodelist = [...document.querySelectorAll('div')]; // 返回一个真正的数组

//之后 ..

nodelist.forEach(...);
nodelist.map(...);
nodelist.slice(...);


//等...
```

## #07 - "use strict" 变得懒惰([原文](https://github.com/loverajoel/jstips#07---use-strict-and-get-lazy))

> 2016-01-07 by [@nainslie](https://twitter.com/nat5an)

（译者注：此片翻译较渣，欢迎指正，建议大家[阅读原文](https://github.com/loverajoel/jstips#07---use-strict-and-get-lazy)或直接阅读[MDN对严格模式的中文介绍](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode) 并欢迎PR）

JavaScript的严格模式使开发者更容易写出“安全”的代码。

通常情况下，JavaScript允许程序员相当粗心，比如你可以引用一个从未用"var"声明的变量。或许对于一个刚入门的开发者来说这看起来很方便，但这也是变量拼写错误或者从作用域外引用变量时引发的一系列错误的原因。

程序员喜欢电脑帮我们做一些无聊的工作，喜欢它自动的检查我们工作上的错误。这就是"use strict"帮我们做的，它把我们的错误转变为了JavaScript错误。

我们把这个指令放在js文件顶端来使用它:

```javascript
// 全脚本严格模式
"use strict";
var v = "Hi!  I'm a strict mode script!";
```


或者放在一个方法内：

```javascript
function f()
{

  // 方法级严格模式
  'use strict';
  function nested() { return "And so am I!"; }
  return "Hi!  I'm a strict mode function!  " + nested();
}
function f2() { return "I'm not strict."; }
```


通过在JavaScript文件或方法内引入此指令，使JavaScript引擎运行在严格模式下，这直接禁止了许多大项目中不受欢迎的操作。另外，严格模式也改变了以下行为：
* 只有被"var"声明过的变量才可以引用。
* 试图写只读变量时将会报错
* 只能通过"new"关键字调用构造方法
* "this"不再隐式的指向全局变量
* 对eval()有更严格的限制
* 防止你使用预保留关键字命名变量

严格模式对于新项目来说是很棒的，但对于一些并没有使用它的老项目来说，引入它也是很有挑战性的。如果你把所有js文件都连接到一个大文件中的话，可能导致所有文件都运行在严格模式下，这可能也会有一些问题。

It is not a statement, but a literal expression, ignored by earlier versions of JavaScript.
严格模式的支持情况:
* Internet Explorer from version 10.
* Firefox from version 4.
* Chrome from version 13.
* Safari from version 5.1.
* Opera from version 12.


[MDN对严格模式的全面介绍](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)

## #06 - 可以接受单各参数和数组的方法

> 2016-01-06 by [@mattfxyz](https://twitter.com/mattfxyz)([原文](https://github.com/loverajoel/jstips#06---writing-a-single-method-for-arrays-or-single-elements))

写一个方法可以接受单个参数也可以接受一个数组，而不是分开写两个方法。这和jQuery的一些方法的工作原理很像(`css` 可以修改任何匹配到的选择器).

你只要把任何东西连接到一个数组. `Array.concat`可以接受一个数组也可以接受单个参数。

```javascript
function printUpperCase(words) {
  var elements = [].concat(words);
  for (var i = 0; i < elements.length; i++) {
    console.log(elements[i].toUpperCase());
  }
}
```


`printUpperCase`现在可以接受单个单词或多个单词的数组作为它的参数。

```javascript
printUpperCase("cactus");
// => CACTUS
printUpperCase(["cactus", "bear", "potato"]);
// => CACTUS
//  BEAR
//  POTATO
```


## #05 - `undefined`与`null`的区别([原文](https://github.com/loverajoel/jstips#05---differences-between-undefined-and-null))

> 2016-01-05 by [@loverajoel](https://twitter.com/loverajoel)

- `undefined`表示一个变量没有被声明，或者被声明了但没有被赋值
- `null`是一个表示“没有值”的值
- Javascript将未赋值的变量默认值设为`undefined`
- Javascript从来不会将变量设为`null`。它是用来让程序员表明某个用`var`声明的变量时没有值的。
- `undefined`不是一个有效的JSON而`null`是
- `undefined`的类型(typeof)是`undefined`
- `null`的类型(typeof)是`object`. [为什么?](http://www.2ality.com/2013/10/typeof-null.html)
- 它们都是基本类型
- 他们都是[falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)
  (`Boolean(undefined) // false`, `Boolean(null) // false`)
- 你可以这样判断一个变量是否是[undefined](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)

  ```javascript
  typeof variable === "undefined"
```

- 你可以这样判断一个变量是否是[null](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null)

  ```javascript
  variable === null
```

- **双等号**比较时它们相等，但**三等号**比较时不相等

  ```javascript
  null == undefined // true

  null === undefined // false
```


## #04 - 排列带音节字母的字符串([原文](https://github.com/loverajoel/jstips#04---sorting-strings-with-accented-characters))

> 2016-01-04 by [@loverajoel](https://twitter.com/loverajoel)

Javascript有一个原生方法**[sort](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)**可以排列数组。一次简单的`array.sort()`将每一个数组元素视为字符串并按照字母表排列。你也可以提供[自定义排列方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort#Parameters) function.

```javascript
['Shanghai', 'New York', 'Mumbai', 'Buenos Aires'].sort();
// ["Buenos Aires", "Mumbai", "New York", "Shanghai"]
```


但是当你试图整理一个如`['é', 'a', 'ú', 'c']`这样的非ASCII元素的数组时，你可能会得到一个奇怪的结果`['c', 'e', 'á', 'ú']`。这是因为排序方法只在英文下有用。

看一下下一个例子:

```javascript
// 西班牙语
['único','árbol', 'cosas', 'fútbol'].sort();
// ["cosas", "fútbol", "árbol", "único"] // bad order

// 德语
['Woche', 'wöchentlich', 'wäre', 'Wann'].sort();
// ["Wann", "Woche", "wäre", "wöchentlich"] // bad order
```


幸运的是，有两种方法可以解决这个问题，由ECMAScript国际化API提供的[localeCompare](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)和[Intl.Collator](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Collator)。

> 两个方法都有自定义配置参数可以使其更好用。

### 使用`localeCompare()`

```javascript
['único','árbol', 'cosas', 'fútbol'].sort(function (a, b) {
  return a.localeCompare(b);
});
// ["árbol", "cosas", "fútbol", "único"]

['Woche', 'wöchentlich', 'wäre', 'Wann'].sort(function (a, b) {
  return a.localeCompare(b);
});
// ["Wann", "wäre", "Woche", "wöchentlich"]
```


### 使用`Intl.Collator()`

```javascript
['único','árbol', 'cosas', 'fútbol'].sort(Intl.Collator().compare);
// ["árbol", "cosas", "fútbol", "único"]

['Woche', 'wöchentlich', 'wäre', 'Wann'].sort(Intl.Collator().compare);
// ["Wann", "wäre", "Woche", "wöchentlich"]
```


- 两个方法都可以自定义区域位置。For each method you can customize the location.
- 根据[Firefox](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare#Performance)，当比较大数量的字符串是Intl.Collator更快。

所以当你处理一个由非英语组成的字符串数组时，记得使用此方法来避免排序出现意外。

## #03 - 优化嵌套的条件语句([原文](https://github.com/loverajoel/jstips#03---improve-nested-conditionals))
> 2016-01-03 by [AlbertoFuente](https://github.com/AlbertoFuente)

我们怎样来提高和优化javascript里嵌套的`if`语句呢？

```javascript
if (color) {
  if (color === 'black') {
    printBlackBackground();
  } else if (color === 'red') {
    printRedBackground();
  } else if (color === 'blue') {
    printBlueBackground();
  } else if (color === 'green') {
    printGreenBackground();
  } else {
    printYellowBackground();
  }
}
```


一种方法来提高嵌套的`if`语句是用`switch`语句。虽然它不那么啰嗦而且排列整齐，但是并不建议使用它，因为这对于调试错误很困难。这告诉你[为什么](https://toddmotto.com/deprecating-the-switch-statement-for-object-literals/).

```javascript
switch(color) {
  case 'black':
    printBlackBackground();
    break;
  case 'red':
    printRedBackground();
    break;
  case 'blue':
    printBlueBackground();
    break;
  case 'green':
    printGreenBackground();
    break;
  default:
    printYellowBackground();
}
```


但是如果在每个语句中都有很多条件检查时该怎么办呢？这种情况下，如果我们想要不罗嗦又整洁的话，我们可以用有条件的`switch`。如果我们传递`true`给`switch`语句，我没变可以在每个case中使用条件语句了。

```javascript
switch(true) {
  case (typeof color === 'string' && color === 'black'):
    printBlackBackground();
    break;
  case (typeof color === 'string' && color === 'red'):
    printRedBackground();
    break;
  case (typeof color === 'string' && color === 'blue'):
    printBlueBackground();
    break;
  case (typeof color === 'string' && color === 'green'):
    printGreenBackground();
    break;
  case (typeof color === 'string' && color === 'yellow'):
    printYellowBackground();
    break;
}
```

但是我们应该时刻注意避免太多判断在一个条件里，尽量少的使用`switch`，考虑最有效率的方法：借助`object`。

```javascript
var colorObj = {
  'black': printBlackBackground,
  'red': printRedBackground,
  'blue': printBlueBackground,
  'green': printGreenBackground,
  'yellow': printYellowBackground
};


if (color in colorObj) {
  colorObj[color]();
}
```


这里有更多相关的[内容](http://www.nicoespeon.com/en/2015/01/oop-revisited-switch-in-js/).

## #02 - ReactJs - Keys in children components are important([原文](https://github.com/loverajoel/jstips#02---reactjs---keys-in-children-components-are-important))

> 2016-01-02  by [@loverajoel](https://twitter.com/loverajoel)

key必须传递给从数组中动态创建的所有组件的一个值。它是一个唯一且固定的id，用来识别DOM中的每个组件，也可以让我们区别它是否是同一个组件。使用key可以确保子容器是可保存而且不需要重复创建的，还可以防止奇怪的事情发生。

> key跟效率不是很相关，它更与身份有关系（这间接的使效率更好）。随机的赋值或改变值将不能识别身份Paul O’Shannessy
使用对象存在的的唯一值。
在父组件定义key,而不是子组件。

```
//bad
...
render() {
    <div key=></div>
}
...

//good
<MyComponent key=/>
```

- 使用数组索引是一个坏习惯
- random() 不会起作用

```
//bad
<MyComponent key=/>
```
你可以创建以自己的唯一id。确定这个方法运行速度够快，把它附着到你的对象上。
当子组件的数量很大或者包含重量级的组件时，使用key来提高性能。
你必须提供key值给ReactCSSTransitionGroup的每个子组件

## #1 - AngularJs: `$digest` vs `$apply`([原文](https://github.com/loverajoel/jstips#1---angularjs-digest-vs-apply))

> 2016-01-01  by [@loverajoel](https://twitter.com/loverajoel)

AngularJs最令人欣赏的特性之一就是双向数据绑定。AngularJs通过循环($digest)检查model和view的变化实现此功能。想要理解框架底层的运行机制你需要理解这个概念。
当一个事件被触发时，Angular触发每个watcher. 这是我们已知的$digest循环。有时你需要强制手动运行一个新的循环，而且因为这是最影响性能的一方面，你必须选择一个正确的选项。

#### $apply
这个核心方法可以让你显式启动digest循环。这意味着所有的watcher将会被检测；整个应用启动$digest loop。在内部在会执行一个可选的方法之后，会调用$rootScope.$digest();.

#### $digest
这种情况下$digest方法在当前作用域和它的子项启动$digest循环。你需要注意他的父作用域将不会被检测也不会被影响。
推荐

- 仅当浏览器DOM事件在AngularJS之外被出发时使用$apply或$digest。
- 给$apply传递方法，它将包含错误处理机制而且允许整合在digest循环里的变化。
```
$scope.$apply(() => {
    $scope.tip = 'Javascript Tip';
});
```

- 如果你只需要更新当前的作用域或者它的子项的话，使用$digest，而且要防止在整个应用里运行新的digest循环。这在性能上的好处是显而易见的。
- $apply()对机器来说是一个困难的处理过程，在绑定过多的时候可能会引发性能问题。
如果你正使用>AngularJS 1.2.X版本，使用$evalAsync, 这个方法将在当前循环或下一个循环执行表达式，这能提高你的应用的性能。



## #0 - 向数组中插入元素([原文](https://github.com/loverajoel/jstips#0---insert-item-inside-an-array))
> 2015-12-29

向一个数组中插入元素是平时很常见的一件事情。你可以使用push在数组尾部插入元素,可以用unshift在数组头部插入元素,也可以用splice在数组中间插入元素。

但是这些已知的方法，并不意味着没有更加高效的方法。让我们接着往下看……

向数组结尾添加元素用push()很简单，但下面有一个更高效的方法

```javascript
var arr = [1,2,3,4,5];

arr.push(6);

arr[arr.length] = 6; // 在Mac OS X 10.11.1下的Chrome 47.0.2526.106中快43%
```
两种方法都是修改原始数组。不信？看看[jsperf](http://jsperf.com/push-item-inside-an-array)

现在我们试着向数组的头部添加元素：

```javascript
var arr = [1,2,3,4,5];

arr.unshift(0);

[0].concat(arr); // 在Mac OS X 10.11.1下的Chrome 47.0.2526.106中快98%
```
这里有一些小区别，unshift操作的是原始数组，concat返回一个新数组，参考[jsperf](http://jsperf.com/unshift-item-inside-an-array)

使用splice可以简单的向数组总监添加元素，这也是最高效的方法。

```javascript
var items = ['one', 'two', 'three', 'four'];
items.splice(items.length / 2, 0, 'hello');
```


我在许多浏览器和系统中进行了测试，结果都是相似的。希望这条小知识可以帮到你，也欢迎大家自行测试。

### License
[![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)

