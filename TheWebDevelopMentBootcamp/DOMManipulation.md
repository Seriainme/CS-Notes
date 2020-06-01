# DOM (Document Object Model)
javascript 与 html + css 的接口
```javascript 1.8
console.dir(document);  // 把 html 转化为 object 格式
```

## select an element
```javascript 1.8
let h1 = document.querySelector("h1"); //select
h1.style.color="pink";  // 操作


// 页面每 1 秒闪一下
let body = document.querySelector("body");
let isBlue = false;
// setInterval(func,timeout)
setInterval(function() {    
    if (isBlue) {
    body.style.background = "white";
} else {
    body.style.background = "#3498db";
}
    isBlue = !isBlue;
},1000) 

// 选择元素
document.getElementById("Id")
document.getElementsByClassName("ClassName")  // return 一个轻量级list(包含很少的功能)
document.getElementsByTagName("li")

// css selector (return the first element)
document.querySelector("#ID")  
document.querySelector(".ClassName")
document.querySelector("h1")  
document.querySelector("li a.special")

// css selector (return all elements with a simple list)
document.querySelectorAll("h1")

```
## 操作元素
- 改变元素的 style
- 增加删除 class
- 改变 tag 的内容
- 改变属性 (src,href)
```javascript 1.8
// 改变 style， 这种方式不够好, 破坏了独立性
let tag = document.getElementById("hightlight");
tag.style.color = "blue";
tag.style.fontSize = "70px";
```
更好的方法
- 在 css 中定义一个 class
- 把这个 class 绑定到 tag 上
```css
/* Define a class in css*/
.some-class {
    color: blue;
    border: 10px solid red;
}
```
```javascript 1.8
// 定义一个 css class
let tag = document.getElementById("hightlight");
tag.classList.add("some-class"); // 增加一个 class

// 其他的用法
tag.classList.remove("another-class");
tag.classList.toggle("another-class");  //切换(有->无->有->无)
```

## textContent
```javascript 1.8
let tag = document.querySelector("p");
tag.textContent = "blah blah blah blah blah"; // 改变内容(但不包含html)，不能给内容加 html 格式

tag.innerHTML // 包含 html 标签

// 读取 input 框的元素
input.value
```

## attributes
```javascript 1.8
let link = document.querySelector("a");
link.getAttribute("href"); // "www.google.com"
link.setAttribute("href","www.dogs.com"); // 改变了地址
```

## DOM Events
- 点按钮
- hovering over a link
- dragging and dropping
- 按回车

```javascript 1.8
// element.addEventListener(type,functionToCall);

let button = document.querySelector("button");
button.addEventListener("click",function() {
    console.log("Someone clicked the button!");
});

// 鼠标移至: mouseover
// 鼠标移出：mouseout
```