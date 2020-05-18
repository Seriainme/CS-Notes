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