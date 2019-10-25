> #### 判断对象类型的方法

- Object.prototype.toString.call()：

```
const b = []
Object.prototype.toString.call(b);
// >= "[object Array]"
const c = {}
Object.prototype.toString.call(c);
// >= "[object Object]"

const d = new Date()
Object.prototype.toString.call(d);
// >= "[object Date]"

const e = new RegExp();
Object.prototype.toString.call(e);
// >="[object RegExp]"
```
---

> #### 类数组对象转数组

- Array.prototype.slice.call(arguments)

- let array = \[...nodeList]

- Array.from(arguments);

> #### 判断数组类型的方法

- arr instanceof Array

- [].constructor == Array

- Object.prototype.toString.call(arr) === '[object Array]'

- Array.isArray([])