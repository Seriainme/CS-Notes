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
