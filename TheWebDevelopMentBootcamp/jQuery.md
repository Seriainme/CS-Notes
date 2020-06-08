# jQuery

一个 DOM 处理的库

## 选择元素
```$(selector) / .css / .html / .attr / .val / .addClass / .first```

- ```$('selectorGoesHere') ```
  - return a list
  
- ```.css()```
  - ```$(selector).css(property,value)```
    - 会把 list 中所有的 items 的 property 改为 value
- ```.text()```
  - ```$(selector).text()```
    - 去除所有 html 标签，只选择文本
    - return one string
  - ```$(selector).text(value)```
    - 把 选择的文本(所有标签内的) 都改为 value
- ```.html()```
  - ```$(selector).html()```
    - 选择的文本包含 html 标签
  - ```$(selector).html(value)```
    - 用来修改包含 html 标签的文本
- ```.attr(property)```
  - ```$(selector).attr(property)```
    - 选择属性（如src）
  - ```$(selector).attr(property,value)```
    - 把属性改成 value (如改变 img 的 src)
- ```.val()```
  - 提取值(如 input / select 内的)
  - ```$(selector).val(value)```
    - 把值改为 value
- ```.addClass()```
  - ```$(selector).addClass("myClass yourClass")```
  - ```$(selector).removeClass("myClass").addClass("yourClass")```
  - ```$(selector).toggleClass("myClass yourClass")```
- ```.first() / .last()```
  - 选第一个 / 最后一个
  - ```$('div').first()``` 
  - ```$('div').last()```

## events
```click() / keypress() / on()```
- ```click()```
  - ```$(selector).click(function(){})```
- ```keypress() / keydown() / keyup()```
  - ```keydown() / keyup()``` 指使哪个 key 被 pressed
  - ```keypress()``` 指使哪个 character 被 entered
  -  ```$(selector).keypress(function(){})```
- ```on()```
  - ```$(selector).on(action,function(){})```
  - 例如： 
    - ```$(selector).on("click",function(){})```
  - 一些 action
    - ```mouseenter / mouseleave```
- ```click()``` 与 ```on("click")``` 的区别
  - ```click()``` 只给已经存在的 elements 增加 listeners
  - ```on()``` 会给以后所有潜在的 elements 增加 listeners
    - ```on()``` 用得更多

## effects
一些动画：
- ```.fadeOut() / .fadeIn() / .fadeToggle()```
  - ```$(selector).fadeOut() // 直接消失 (其实是添加 style="display: none")```
  - ```$(selector).fadeOut("slow",function(){})  // 缓慢消失```
  - ```$(selector).fadeOut(1000,function(){})   // 1000ms, function 在 fadeOut 结束后运行```
- ```.slideDown() / .slideToggle() / .slideUp()```

## this 
JQuery 中的 this
- ```$("this")```
  - ```$("this").remove() // 删除元素```

