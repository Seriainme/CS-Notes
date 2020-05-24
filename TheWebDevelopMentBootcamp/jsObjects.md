# Javascript Objects

```javascript 1.8
let person = {  // Object
    name : "Cindy",
    age : 32,
    city : "Missouri",
    add: function(x,y) { // 可以在 object 上加入 方法
      return x+y;
    }
}
person.add(1,2) // 3
```

## 关键词 this
```javascript 1.8
let comments = {};
comments.data = ["Good job!","bye bye","Lame..."];
comments.print = function() {
    this.data.forEach(function (value) { console.log(value) });
}
comments.print() // Good job!   bye bye   Lame...
```


