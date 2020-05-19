# bootstrap4

## resources
Document: https://getbootstrap.com/docs/4.4/getting-started/introduction/
中文版：https://code.z01.com/v4/docs/index.html

## color
[bootstrap4 colors zh](https://code.z01.com/v4/utilities/colors.html)  
[bootstrap4 colors en](https://getbootstrap.com/docs/4.4/utilities/colors/)

[bootstrap4 背景颜色](https://code.z01.com/v4/utilities/colors.html#background-color)  
```html
<div class="p-3 mb-2 bg-primary text-white">bg-primary</div>
```
[bootstrap4 文字颜色](https://code.z01.com/v4/utilities/colors.html#color)
```html
<p class="text-primary">text-primary</p>
```
## typographical 排版
[排版](https://code.z01.com/v4/content/typography.html)
```html
<!-- 大字体 -->
<h1 class="display-2">Display 2</h1>
<!-- 引用 -->
<blockquote class="blockquote">
    <p class="mb-0">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer posuere erat a ante.</p>
</blockquote>

<blockquote class="blockquote">
    <p class="mb-0">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer posuere erat a ante.</p>
    <!-- 注脚作者名 -->
    <footer class="blockquote-footer">Written by my cat <cite title="Blue Steele">Blue Steele</cite></footer>
</blockquote>
```
## img
```html
<img class="img-fluid" src="imgs/hand2.png">
<!-- class="imgs-fluid: { height:auto; max-width: 100% } -->
```

## border
[border 边框](https://code.z01.com/v4/utilities/borders.html)

```html
<h5 class="border border-danger rounded-top"> GIVE ME A BORDER </h5>
<!-- border 边框 -->
<!-- border-danger 警示边框 -->
<!-- rounded-top 上圆角 -->
<h5 class="border-top border-info"> GIVE ME A BORDER </h5>
<!-- border-info 消息边框 -->
<!-- border-top 上边框 -->
<h5 class="border border-left-0 border-warning"> GIVE ME A BORDER </h5>
<!-- border-left-0 取消左边框 -->
```
## margin and padding
[spacing间隔规范（Margin与Padding）](https://code.z01.com/v4/utilities/spacing.html)

- m : 这个Class属性会设定 margin值
- p : 这个Class属性会设定 padding值
- t : top
- b : bottom
- l : left
- r : right
- x : left and right
- y : top and bottom


## breakpoint
[spacing间隔规范（Margin与Padding）](https://code.z01.com/v4/utilities/spacing.html)

- 对于 sm、 md、lg、 xl使用 ```{property}{sides}-{breakpoint}-{size}```格式命名CSS方法。
```html
<button class="btn btn-primary p-4 mx-0 mx-sm-2 mx-md-3 mx-lg-4 mx-xl-5">
```

## Navbars
![Navbars](https://code.z01.com/v4/components/navbar.html#navbar)

## display
![display](https://code.z01.com/v4/utilities/display.html#display)
```html
<h1 class="border border-danger d-inline">HELLO</h1>
<!-- d-inline 只显示包含文字的边框 -->
```

## jumbotron
[jumbotron](https://code.z01.com/v4/components/jumbotron.html#jumbotron)


## flex
[flex](https://code.z01.com/v4/utilities/flex.html#flex)

```html
<div class="d-flex p-2">I'm a flexbox container!</div>
<!-- d-flex 启动 flex -->


<div class="border border-dark d-flex justify-content-between align-items-center" style="height:200px">
    <button class="btn btn-info btn-lg">LG</button>
    <button class="btn btn-warning btn-sm">sm</button>
    <button class="btn btn-primary btn-sm">sm</button>
</div>
<!-- justify-content-between 横向居中对齐  -->
<!-- align-items-center 水平居中 -->
```

### direction
flex-row    横向方向排列
flex-column 垂直方向排列
```html 
<div class="border border-dark d-flex flex-row-reverse justify-content-start align-items-end" style="height:200px">
    <button class="btn btn-info btn-lg">LG</button>
    <button class="btn btn-warning btn-sm">sm</button>
    <button class="btn btn-primary btn-sm">sm</button>
</div>
<!-- flex-row-reverse 横向反方向排列 -->
<!-- flex-column 垂直方向排列 -->
```
### nav
![navs](https://code.z01.com/v4/components/navs.html#navs)
nav 可以使用 flex 的元素
fixed-top 使导航栏一直在顶部

### grid
[grid](https://code.z01.com/v4/layout/grid.html#grid)

grid 基于 flexbox
```html
  <div class="row border justify-content-around align-items-center">
    <div class="col-sm align-self-start">
      三分之一空间占位
    </div>
    <div class="col-sm">
      三分之一空间占位
    </div>
    <div class="col-sm">
      三分之一空间占位
    </div>
  </div>
```

## 总结:
常用class

```html
<nav id="mainNavbar" class="navbar navbar-dark navbar-expand-md py-0 fixed-top">
<!-- navbar-dark 改导航栏字体为白色 -->
<!-- py-0 y轴padding 为0 -->
<!-- fixed-top 固定首位 -->
  <a href="#" class="navbar-brand">CANDY</a>
<!-- navbar-brand  -->
      <button class="navbar-toggler" data-toggle="collapse" data-target="#navLinks" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navLinks">
        <ul class="navbar-nav">
          <li class="nav-item">
            <a href="" class="nav-link">HOME</a>
          </li>
          <li class="nav-item">
            <a href="" class="nav-link">ABOUT</a>
          </li>
          <li class="nav-item">
            <a href="" class="nav-link">TICKETS</a>
          </li>
        </ul>
      </div>
    </nav>
```

```html
    <section class="container-fluid px-0">
    <!-- container-fluid 不留边距 -->
      <div class="row align-items-center">
      <!-- row 这些在同一行 -->
      <!-- align-items-center 水平居中 -->
        <div class="col-lg-6">
        <!-- col-lg-6 在>lg时占6栏 -->
          <div id="headingGroup" class="text-center text-white d-none d-lg-block mt-5">
          <!-- text-center 文字居中 -->
          <!-- d-none 文字消失 -->
          <!-- d-lg-block 当大于lg时文字出现（整行） -->
          <!-- mt-5 上边距 margin 5 -->
            <h1 class="">MUSEUM<span>/</span>OF<span>/</span>CANDY</h1>
            <h1 class="">MUSEUM<span>/</span>OF<span>/</span>CANDY</h1>
            <h1 class="">MUSEUM<span>/</span>OF<span>/</span>CANDY</h1>
          </div>
        </div>
        <div class="col-lg-6">
          <img class="img-fluid" src="imgs/hand2.png">
          <!-- imgs-fluid 图片全部显示 -->
        </div>
      </div>
    </section>
```

```html
    <section class="container-fluid px-0">
      <div class="row align-items-center content">
        <div class="col-md-6 order-2 order-md-1">
        <!-- order-2 order-1 显示次序 -->
          <img src="imgs/milk.png" class="img-fluid">
        </div>

        <div class="col-md-6 text-center order-1 order-md-2">
          <div class="row justify-content-center">
          <!-- justify-content-center 横向对齐 -->
            <div class="col-10 col-lg-8 blurb mb-5 mb-md-0">
            <!-- mb-5 margin bottom - 5 -->
              <h2>MUSEUM OF CANDY</h2>
              <img src="imgs/lolli_icon.png" class="d-none d-lg-inline">
              <!-- d-lg-inline 文字显示（不是整行，而是只有文字） -->
              <p class="lead">Lorem ipsum dolor sit amet.</p>
            </div>
          </div>
        </div>
      </div>
    </section>
```

### 常用css
```css 
#mainNavbar {
    font-size: 1.5rem;
    /* 文字大小 */
    font-weight: 100;
    /* 文字粗细 */
}

.blurb p {
    color: #f498b8;
    /* 文字颜色 */
    font-weight: 100;
    font-size: 1.125rem;
    line-height: 2;
    /* 行距 */
}

.content {
    margin-top: 100px;
    margin-bottom: 100px;
    /* 上下margin : margin 框外距离 */
}
```

 





