# Function

```javascript
function doSomething() {
    console.log("Hello world!");
}

doSomething();
doSomething();
doSomething();

// -----------------------------

// 函数声明
function foo(){
    function bar() {
        return 3;
    }
    return bar();
    function bar() {
        return 8;
    }
}
alert(foo());
// 函数声明和函数变量总是由JavaScript解释器移动（提升）到其JavaScript作用域的顶部”，即：
//**Simulated processing sequence for Question 1**
function foo(){
    //define bar once
    function bar() {
        return 3;
    }
    //redefine it
    function bar() {
        return 8;
    }
    //return its invocation
    return bar(); //8
}
alert(foo());

// -----------------------------------

// 函数表达式
//anonymous function expression
var a = function() {
    return 3;
}
//named function expression
var a = function bar() {
    return 3;
}

// 函数表达式是否被挂起？
function foo(){
    var bar = function() {
        return 3;
    };
    return bar();
    var bar = function() {
        return 8;
    };
}
alert(foo());

// 挂起为：
//**Simulated processing sequence for Question 2**
function foo(){
    //a declaration for each function expression
    var bar = undefined;
    var bar = undefined;
    //first Function Expression is executed
    bar = function() {
        return 3;
    };
    // Function created by first Function Expression is invoked
    return bar();
    // second Function Expression unreachable
}
alert(foo()); //3
```
- 在JavaScript中，函数是具有值的活动对象（Java方法只是元数据存储）
- 函数表达式更具通用性
- 使用匿名函数进行调试可能会令人沮丧，建议使用命名函数表达式（NFE）作为解决方法。
- [Function Declarations vs. Function Expressions](https://javascriptweblog.wordpress.com/2010/07/06/function-declarations-vs-function-expressions/)

### 替换字符串
```javascript 1.8
//replace all '-' with "_"'s
function kebabToSnake(str) {
    return str.replace(/-/g,"_");
}
```
- stringObject.replace(regexp/substr,replacement)
- regexp/substr	
  - 规定子字符串或要替换的模式的 RegExp 对象
    - g 全局模式
    - i 不区分大小写
  - 如果是一个字符串，则将它作为要检索的直接量文本模式
  
- replaceAll(search, replaceWith)
  - search
    - 是字符串则替换所有出现的 search
    - 是非全局正则表达式，则抛出TypeError异常

## JS Scope

- var 
  - 在自己的 frame 里 
  - 有声明提前
  
- let
  - 在语句所在的代码块内
  - 不允许重复声明

 
 ```javascript 1.8
for (let i = 0; i < 5; i++) {
  console.log(i); //0 1 2 3 4 
}
      
console.log(i); //ReferenceError: i is not defined
```

## Higher Order Functions
```javascript 1.8
let x = setInterval(anotherFunc, interval);
// 高阶函数，传入其他函数作为参数
// setInterval: 每 interval 时间(ms) 执行一次 anotherFunc
// 我们没有调用 anotherFunc, setInterval 在调用 anotherFunc
clearInterval(x); // 停止调用

setInterval(function() { // 匿名函数(主要用于 pass a function to another function)
  console.log("I am an anonymous function!");
},2000)
```