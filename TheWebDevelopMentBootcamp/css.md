# CSS

## font color
```css
h1 {
    /* font color */
    color: black;
    color: rgb(143, 88, 88);
    color: rgba(117, 39, 39,0.8);
}
```
## background color
```css
/* background color */
body {
    background:blue;
    background: url(http://);
    background-repeat: no-repeat;
    background-size: cover;
}
div {
    background: blue;
}
p {
    color: blue;
}
```

## border
```css
h1 {
    border-color: purple;
    border-width: 5px;
    border-style: solid;
    /* the same */
    border: purple 5px solid;
}
```

## selector
```css
/* element selector */
li {
}
/* id selector */
#special {
}
/* class selector */
.completed {
}
/* Star */
* {
}
/* Descendant Selector */
li a{ 
}
/* Adjacent Selector */
h4 + ul {
}
/* Attribute Selector */
a[href="http://"] {
}
/* nth of type */
li:nth-of-type(3) {
}

```

## font styles
```css
p {
    font-family: Arial;/* 字体 */
    font-size: 200px;
    font-weight: 200;/* 100-800  */
    font-weight: bold; /* 加粗 */
    line-height: 2; /* 两倍行距 */
    text-decoration: underline; /* 下划线 */
    text-transform: uppercase; /* 变大写字体 */
	letter-spacing: 0.2rem; /* 根据网页root element */
}
span {
	font-size: 2.0em; /* 根据parent element */
}0em;
}
```

## Box Model
```css
p {
	width: 200px; /* 边框 */
	height: 10%; /* 边框 */
	max-width: 300px; /* 边框 */
	border: 2px solid blue; /* 边框 */
	padding: auto; /* 边框内边距 */
	margin: 200px 300px 500px 300px; /* 边框外边距 上右下左 */
}

img {
    float: left; 
    /* float CSS属性指定一个元素应沿其容器的左侧或右侧放置，允许文本和内联元素环绕它。该元素从网页的正常流动(文档流)中移除，尽管仍然保持部分的流动性（与绝对定位相反）。
 */
}

```

## hover
```css
#mainNavbar .nav-link:hover {
    color: #EA1C2C;
}
```
hover: （盘旋）鼠标指向的时候颜色发生变化