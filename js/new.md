> #### js中new一个对象的过程
首先了解new做了什么，使用new关键字调用函数（new ClassA(…)）的具体步骤：

1. 创建一个新对象：

   - var obj = {};

1. 设置新对象的constructor属性为构造函数的名称，设置新对象的__proto__属性指向构造函数的prototype对象；

   - obj.__proto__ = ClassA.prototype;

1. 使用新对象调用函数，函数中的this被指向新实例对象：

1. 将初始化完毕的新对象地址，保存到等号左边的变量中

**注意：若构造函数中返回this或返回值是基本类型（number、string、boolean、null、undefined）的值，则返回新实例对象；若返回值是引用类型的值，则实际返回值为这个引用类型。**

> #### 原生js实现new

```
function Person (name, age) {
  this.name = name;
  this.age = age;
}
Person.prototype.sayName = function () {
  console.log(this.name);
};
var person = new Person('Hellen', 23);
person.sayName();
console.log(person instanceof Person);
 
function New () {
  let obj = new Object();
  let fn = [].shift.call(arguments);
  let args = arguments;
  var ret = fn.apply(obj, args);
  obj.__proto__ = fn.prototype;
  return typeof ret === 'object' ? ret : obj;
}
let person1 = New (Person, 'Bob', 23);
person1.sayName();
console.log(person1 instanceof Person);
```