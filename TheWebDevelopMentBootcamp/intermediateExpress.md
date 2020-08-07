# intermediate express

## render html and templates
- EJS
  - “E” 表示 “可嵌入（Embedded）”，也可以是“高效（Effective）”、“优雅（Elegant）”或者是“简单（Easy）”
  - 利用普通的 JavaScript 代码生成 HTML 页面
- 使用方法
  - `npm install ejs`
  - `mkdir views`
  - `touch views/home.ejs`
  - `app.js` 中 `res.render("home.ejs")`
    - 也可以写作 `res.render("home")`，但需要在app.js加上 `app.set("view engine","ejs");`
    - `app.set("view engine","ejs");` 设置使用的模版引擎
    - 源码`app.set = function set(setting, val) {  this.settings[setting] = val;};`
      - 改变的是 settings 内的值
  - `app.js` 中 `res.render("love.ejs",{thingVar:req.params.thing})`
    - 在 `love.ejs` 中获取返回值 `<%= thingVar %>`
      - `<%= 5+5 %>` 会返回 `10`
    - 在 `love.ejs` 中获取代码 `<% if ... %>`
    ```ejs
    <% if (thingVar.toLowerCase() === "rusty") { %>
    <p>good choice, rusty is the best!</p>
    <% } else { %>
      <p>Bad choice!</p>
    <% } %>
    ```
  
### 设置静态文件目录
```javascript
// app.js
app.use(express.static (__dirname + "/public") ); //设置静态资源（文件）目录(项目根目录+ public)
// __dirname 是 node.js 中的一个全局变量，它指向当前脚本所在的目录。
```
- 为了提供对静态资源文件（图片，css，js文件）的服务，使用Express内置的中间函数express.static.
  - 传递一个包含静态资源的目录给express.static中间件用于立即开始提供文件
  - 现在可以加载该目录下的文件了
    - 不需要把静态目录作为URL的一部分
    - 可以使用 express.static 函数指定一个虚拟的静态目录
      - `app.use('/static', express.static (__dirname + "public"));`
      - 现在你可以使用 /static 作为前缀来加载 public 文件夹下的文件了
      - `http://localhost:3000/static/images/kitten.jpg`
      - 也可以在 ejs 文件中使用 link 标签找到对应的 css 文件
        - `<link rel="stylesheet" href="app.css">`

### html 模板
- <%- 输出非转义的数据到模板
- 模板标签插入正式的 ejs 文件
```ejs
<%- include('header'); -%>
<h1>
  Title
</h1>
<p>
  My page
</p>
<%- include('footer'); -%>
```
header.ejs 中为
```ejs
<!DOCTYPE html>
<html>
<head>
    <title>Demo App</title>
    <link rel="stylesheet" href="/app.css">
    <!--这儿必须是/app.css 对应的是静态目录下找到的 app.css文件，也就是public文件夹中的app.css文件-->
    <!--直接用 app.css 由于模板的位置不同而找不到 app.css       
</head>
<body>
```

## 中间件 body-parser
- [Express中间件body-parser](https://www.jianshu.com/p/cd3de110b4b6)
- 一个 HTTP 请求体解析的中间件，使用这个模块可以解析 JSON、Raw、文本、URL-encoded 格式的请求体
- Node 原生的 http 模块中，是将用户请求数据封装到了用于请求的对象 req 中
  - 这个对象是一个 IncomingMessage
  - 该对象同时也是一个可读流对象
- `app.use(bodyParser.urlencoded({extended: true}));`
  - 为项目帮助解析请求体 req
  - 经过这个中间件后，就可以在所有路由处理器的req.body中访问请求参数
- 也可以为单个路由帮助解析请求体
 ```javascript
//创建application/x-www-form-urlencoded
var jsonParser = bodyParser.json();
//POST /api/users 获取JSON编码的请求体
app.post('/api/users', jsonParser, function(req,res){
    if(!req.body) return res.sendStatus(400);
    //create user in req.body
});
```
- 还支持为某一种或者某一类内容类型的请求体指定解析方式
  - `app.use(bodyParser.json({type: 'text/plain'}))`

## 总结
- 创建 public 目录，把一些 css 和静态文件放里面
- `app.js` 中指定静态目录 `app.use('/static', express.static (__dirname + "public"));`
- `app.set("view engine","ejs");` 设置使用的模版引擎
  - 可以直接使用 `res.render("love")` 而不需要加 `.ejs` 后缀
- `views` 目录存放 `ejs` 文件，`views` 不需要在 `render` 指定文件夹名即可使用
- `views` 中新建 `partials` 文件夹储存模板 `ejs`
- `<%- include("partials/header") %>` 的语法导入模板 `ejs`
- 一些语法
  - `<%` '脚本' 标签，用于流程控制，无输出
  - `<%-` 输出非转义的 HTML 标签
  - `<%=` 输出数据到模板（输出是转义 HTML 标签）
- 要读取 post 的文本
  - 使用 body-parser 中间件
  - 给 `<input>` 标签中添加 `name`
  - 调用中间件帮助解析body `app.use(bodyParser.urlencoded({extended: true}));`
  - `req.body` 获取 post 的文本
