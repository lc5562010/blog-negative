#### promise

- promise链式调用的时候上一个回调函数一定是return一个promise实例

- promise链式调用只需要在最后写一个catch就可以捕捉到前边的任何一个错误

- promise.all里面的promise实例最好没一个都写上自己的catch方法，这样其中任何一个实例报错都不会进入到promise.all的catch中，也就是说不会影响其他实例的返回结果

#### async await

- 调用async函数会立即返回一个Promise对象

- async函数返回的 Promise 对象，必须等到内部所有await命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。也就是说，只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。

- 正常情况下，await命令后面是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值。

- 任何一个await语句后面的 Promise 对象变为reject状态，那么整个async函数都会中断执行。

    解决办法：
    
    1. 将第一个await放在try...catch结构里面，这样不管这个异步操作是否成功，第二个await都会执行。
    
    1. await后面的 Promise 对象再跟一个catch方法，处理前面可能出现的错误。

- 多个await命令后面的异步操作，如果不存在继发关系，最好让它们同时触发。

    解决办法：
    ```
    // 写法一
    let [foo, bar] = await Promise.all([getFoo(), getBar()]);
    
    // 写法二
    let fooPromise = getFoo();
    let barPromise = getBar();
    let foo = await fooPromise;
    let bar = await barPromise;
    ```
    
- await命令只能用在async函数之中，如果用在普通函数，就会报错。

