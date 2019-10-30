> #### for in遍历

- 遍历的是键，即如果是数组遍历的是数组的下标，如果是对象遍历的是对象的属性。

- 可以遍历数组或者对象原型上的属性和方法

**注意**

使用for in 也可以遍历数组，但是会存在以下问题：

1. index索引为字符串型数字，不能直接进行几何运算

2. 遍历顺序有可能不是按照实际数组的内部顺序

3. 使用for in会遍历数组所有的可枚举属性，包括原型。例如上栗的原型方法method和name属性

```
所以一般for in用来遍历遍历对象
```

```
Array.prototype.sayHello = function(){
    console.log("Hello")
}

Array.prototype.str = 'world';

var myArray = [1,2,10,30,100];

myArray.name='数组';

for(let index in myArray){
    console.log(index);
}

// 0,1,2,3,4,name,str,sayHello
```

```
Object.prototype.sayHello = function(){
    console.log('Hello');
}

Object.prototype.str = 'World';

var myObject = {name:'zhangsan',age:100};

for(let index in myObject){
    console.log(index);
}

// name,age,str,sayHello
```

> #### for of遍历

- 遍历的是值,不能用来遍历对象，如果想要遍历对象的话要配合Object.keys()使用。

- 遍历的是可迭代对象(包括 Array, Map, Set, String, TypedArray，arguments 对象等等)。

- 不会遍历原型上的属性和方法 

```
Array.prototype.sayHello = function(){
    console.log("Hello");
}

var myArray = [1,200,3,400,100];

for(let key of myArray){
    console.log(key);
}

// 1,200,3,400,100
```

```
Object.prototype.sayHello = function(){
    console.log('Hello');
}

var myObject = {
    name:'zhangsan',
    age:10
}

for(let key of myObject){
    consoloe.log(key);
}

// typeError 报错
```