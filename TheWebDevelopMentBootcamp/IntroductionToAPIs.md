# Introduction to APIs
## API 
- 应用编程接口
- ifttt(if that than that)  https://ifttt.com/
- api 网站 https://www.programmableweb.com/

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
url = "http://www.baidu.com";
requests = request(url,function(error,response,body) {
  if (!error && response.statusCode === 200) {
      console.log(body);
  }
});
// 后面的 function 被称为回调函数
// function b(function c):接收的参数是一个函数,c这个函数就叫回调函数

// 这儿的 error,response,body 是 requests 的子集
// body 相当于 requests.req.res.body
// response 相当于 requests.req.res
```
- url 改成 api 地址即可访问 api
