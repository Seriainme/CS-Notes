# Array

```javascript 1.8
// initialize an empty array
let friends = [];
let friends = new Array();
let random_collection = [49, true, "Hermione"]; // different types



let friends = ["Charlie","Liz","David","Mattias"];
//                 0       1      2         3
friends[0]; // Charlie
friends[0] = "Chuck";  //    ["Chuck","Liz","David","Mattias"];
friends[4] = "Amelie"; //    ["Chuck","Liz","David","Mattias","Amelie"];
// stack push pop
friends.push("Smith"); //    ["Chuck","Liz","David","Mattias","Amelie","Smith"];
friends.pop(); // "Smith"    ["Chuck","Liz","David","Mattias","Amelie"];
// queue addFirst removeFirst
friends.unshift("First"); // ["First","Chuck","Liz","David","Mattias","Amelie"];
friends.shift(); // "First"  ["Chuck","Liz","David","Mattias","Amelie"];
friends.indexOf("Liz") //   1
friends.indexOf("Liz1") // -1
// 深拷贝 slice(start,end)  不包括end
friends.slice(1,3); // ["Liz","David"]  friends 本身不变化

// splice(startIndex,Count) 删除元素，返回一个list(所删除的元素)
friends.splice(1,2); // ["Liz", "David"]  friends 变为 ["Chuck", "Mattias", "Amelie"]

```

## ForEach
```arr.forEach(someFunction)```

```javascript 1.8
let colors = ["red","blue","orange","yellow"];
let printColor = function(color) {
    console.log(color);
}
colors.forEach(printColor);
// the same as:
colors.forEach(function(color){console.log(color)});
```
- forEach 中的 function 最多可以用三个参数，依次含义为：
  - foreach被调用的数组中的每个元素(每次循环迭代)。
  - 第二个表示该元素的索引。
  - 第三个表示foreach被调用的数组(对于循环的每次迭代都是相同的)。
  
### For Each 的实现原理  
```javascript 1.8
// 面向过程
function myForEach(arr, func){
	for (var i = 0; i < arr.length; i++) {
		func(arr[i],i,arr);
	}
}

var colors = ["red", "orange", "yellow", "green", "blue", "PURPLE"];
myForEach(colors, function(color,index,arr){
	console.log(color,index,arr);
});

// 面向对象
// prototype: 给函数的原型对象添加新属性 (以字典形式)
// https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain
// 访问属性时，先查看函数内有无这个属性->再查看__proto__内有无这个属性，再查看__proto__的__proto__，直到彻底找不到
Array.prototype.myForEach = function(func){
  for(var i = 0; i < this.length; i++) {
   func(this[i],i,this);
  }
};

var colors = ["red", "orange", "yellow", "green", "blue", "PURPLE"];
colors.myForEach(function(color,index,arr){
	console.log(color,index,arr);
});

```