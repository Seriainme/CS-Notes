# Node JS

## npm
- js 的包管理
  - ```npm install ...```
- 调用包
  - ```let something = require("packageName")```

## module vs module.exports
- Node先准备一个对象```module```,并将其传入加载函数,因此我们在js文件中可以直接调用
- 真正的函数运行如下：
```javascript
// 准备module对象:
var module = {
    id: 'hello',
    exports: {}  // 当我们用require()获取module时，
                 // Node找到对应的module，把这个module的exports变量返回
};
var load = function (exports,module) {
    // 读取的hello.js代码:
    function greet(name) {
        console.log('Hello, ' + name + '!');
    }
    
    module.exports = greet;
    // hello.js代码结束
    return module.exports;  // 可以看到返回的就是module.exports
};
var exported = load(module.exports,module);
// 保存module:
save(module, exported);
```
由上可以看出：
- 默认情况下，Node准备的```exports```变量和```module.exports```变量实际上是同一个变量
  - 并且初始化为空对象{}
- 因此两种是等价的
  - ```exports.foo = function () { return 'foo'; };```
  - ```module.exports.foo = function () { return 'foo'; };```
- 但要输出是一个函数或数组，那么，只能给 ```module.exports```赋值：
  - 因为你要输出不同的东西的时候，```exports``` 变了但 ```module.exports``` 没变
  - ```module.exports = function () { return 'foo'; };```

结论：
- 只需要用 ```module.exports``` , ```exports``` 是一种函数内部的其他变量名的引用
  
## express

### 最简单的 express 路由

app.js
```javascript
//require 返回的是我们用module.exports = createApplication;输出的createApplication函数
const express = require("./express");

const app = express(); // app 是一个 createApplication(),里面有get、listen方法等

app.get("/",function (req,res) {
    // https://stackoverflow.com/questions/53556939/nodejs-response-send-is-not-a-function
    // 我写的 express 中，response是一个http.ServerResponse实例，没有.send方法。
    // Express是构建HTTP服务器的框架，它在常规http模块之上提供了其他功能
    // 这些附加功能之一是用于HTTP响应实例的.send方法：
    res.end("hello world!"); });

app.listen(port = 3000,function () {
    console.log('Example app listening on port ' + port);
})
```
express.js
```javascript
const http = require('http');  // 创建 http 服务
const url = require('url');  // 拿到当前的请求路径

// 存路由，每个都是一个 Object,包含路径、方法、回调函数
// 存一个所有都没匹配后的失败请求
let router = [
    {path:"*",method:"*",handler(req,res){
        let message = "Cannot "+ req.method+" " + req.url;
        res.end(message); //end 停止连接
    }}
];

function createApplication() {
    // return 一个 Object，里面有 get, listen 两个函数
    return {
        // 调用 get 函数就把 Object 存进路由
        get(path,handler){
            router.push({
                path:path,
                method:"get",
                handler
            })
        },
        // listen 用来匹配路由，并 listen 对应的端口
        listen(){
            // 创建一个服务,服务中传入的是一个function
            let server = http.createServer(function (req,res) {
                // url.parse(urlStr); 一个URL字符串转换成对象并返回
                let pathname = url.parse(req.url).pathname;
                // 寻找路由
                for (let i = 1; i < router.length ; i++) {
                    let currRouter = router[i];
                    if (pathname === currRouter.path && req.method.toLowerCase() === currRouter.method) {
                        return currRouter.handler(req,res);
                    }
                }
                return router[0].handler(req,res);
            });
            // listen 对应的端口
            server.listen(...arguments);
        }
    }
}

// 要在模块中对外输出变量，用：module.exports
// 这里把函数createApplication作为模块的输出暴露出去
module.exports = createApplication;
```