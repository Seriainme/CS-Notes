# Introduction to APIs
## API 
- 应用编程接口
- ifttt(if that than that)  https://ifttt.com/
- API 网站 https://www.programmableweb.com/

## json & xml
- xml(extended markup language)
```xml
<person>
  <age>21</age>
  <name>wyz</name>
  <city>Beijing</city>
</person>
```
- json(javascript object notation)
```json
{
  "person": {
    "age": 21,
    "name": "wyz",
    "city": "Beijing"
  }
}
```
- 注意 JavaScript 对象的 key 不是字符串
  - `var person = {firstName:"Bill", lastName:"Gates", age:62, eyeColor:"blue"};`
  - firstName,lastName,age,eyeColor 被称为属性
  
## 使用 js 访问 API
```javascript
// request google.com
const request = require("request");
url = "https://jsonplaceholder.typicode.com/users";
requests = request(url,function(error,response,body) {
  if (!error && response.statusCode === 200) {
      let parsedData = JSON.parse(body); //解析 API，之后可以用 parsedData["key1"] 调用
      console.log(parsedData[1]["name"]);
  }
});
// 后面的 function 被称为回调函数
// function b(function c):接收的参数是一个函数,c这个函数就叫回调函数

// 这儿的 error,response,body 是 requests 的子集
// body 相当于 requests.req.res.body
// response 相当于 requests.req.res
```
- url 改成 API 地址即可访问 API
- 获取的不是 JavaScript 对象，而是字符串
  - 对 json 进行解析 `let parsadData = JSON.parse(body);`
  - 之后可以访问 API 中的内容 
    - `parsedData["key1"]["key2"];`
    - `parsedData.key1.key2`
## 对 node 进行交互式 debug 的方法
- 使用 Pryjs 包
  - https://www.npmjs.com/package/pryjs
  - 安装 `npm install --save pryjs`
  - 使用 `pry = require('pryjs'); eval(pry.it);`
  - 可以在交互界面输出指定变量的值
  

## 使用 Promise 简化回调函数
- Promise是一个函数返回的对象，我们可以在它上面绑定回调函数，这样我们就不需要在一开始把回调函数作为参数传入这个函数了。

```javascript
// 成功的回调函数
function successCallback(result) {
  console.log("音频文件创建成功: " + result);
}

// 失败的回调函数
function failureCallback(error) {
  console.log("音频文件创建失败: " + error);
}

createAudioFileAsync(audioSettings, successCallback, failureCallback)
```

使用 promise 函数会返回一个 promise 对象，可以将你的回调函数绑定在该 promise 上。

```javascript
const promise = createAudioFileAsync(audioSettings); 
promise.then(successCallback, failureCallback);
// 这样可以避免回调函数地狱
```