- macro-task(宏任务)：包括整体代码script，setTimeout，setInterval

- micro-task(微任务)：Promise，process.nextTick

执行原则:

- 事件循环的顺序，决定js代码的执行顺序。进入整体代码(宏任务)后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务。

- settimeout的回调函数放到宏任务队列里，等到执行栈清空以后执行;

- promise.then里的回调函数会放到相应宏任务的微任务队列里，等宏任务里面的同步代码执行完再执行;

- **async**函数表示函数里面可能会有异步方法，await后面跟一个表达式，async方法执行时，遇到await会立即执行表达式，然后把表达式后面的代码放到微任务队列里，让出执行栈让同步代码先执行。
---
#### 测试

```
console.log('1');

setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})
```

```
1，7，6，8，2，4，3，5，9，11，10，12
```
```
let p = new Promise(function (resolve, reject) {
   console.log('Promise');
   resolve();
}).then(() => {
   console.log('Promise end');
});

async function async1() {
   console.log('async1 start');
   await p;
   console.log('async1 end');
}

console.log('script start');
async1();
console.log('script end');
```

```
Promise
script start
async1 start
script end
Promise end
async1 end
```

#### 参考链接

- [掘金](https://juejin.im/post/59e85eebf265da430d571f89)
- [知乎](https://zhuanlan.zhihu.com/p/33058983)
