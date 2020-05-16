# javascript
[X分钟速成Javascript](https://learnxinyminutes.com/docs/zh-cn/javascript-cn/)

```javascript
// 所有基本的算数运算都如你预期。
1 + 1; // = 2
0.1 + 0.2; // = 0.30000000000000004
8 - 1; // = 7
10 * 2; // = 20
35 / 5; // = 7

// 基本变量
var

// 也有布尔值。
true;
false;

// 可以通过单引号或双引号来构造字符串。
'abc';
"Hello, world";

null;      // 用来表示刻意设置的空值
undefined; // 用来表示还没有设置的值(尽管`undefined`自身实际是一个值)

// es6 新增
let name = 'dog';
var name2 = 'cat';
const age = '10'; //const声明额变量不能改变，所以，const一旦声明一个变量，就必须马上初始化，不能留到以后赋值
console.log(window.name);  //undefined
console.log(window.name2); // 'cat'

var x = 10;
var x = "abc"; // OK

let y = 10;
let y = "abc"; // ERROR
```

## alert prompt console.log
```javascript
alert("abc"); // 只是显示警示
console.log("abc"); // 只在 console 打印
let texts = prompt("some text"); // 给用户输入内容来交互

