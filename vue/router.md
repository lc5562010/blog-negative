### 路由的原理
#### 1. hash模式

‘#’本身以及它后面的字符称之为hash可通过window.location.hash属性读取.
- hash虽然出现在url中，但不会被包括在http请求中，它是用来指导浏览器动作的，对服务器端完全无用，因此，改变hash不会重新加载页面。

-  可以为hash的改变添加监听事件：

    `window.addEventListener("hashchange",funcRef,false)`

- 每一次改变hash(window.location.hash)，都会在浏览器访问历史中增加一个记录

- push()方法最主要的是对window的hash进行了直接赋值：

    `window.location.hash=route.fullPath`

    hash的改变会自动添加到浏览器的访问历史记录中。

- replace()方法与push()方法不同之处在于，它并不是将新路由添加到浏览器访问历史栈顶，而是替换掉当前的路由：

    `调用window.location.replace方法将路由进行替换`

#### 2. history模式

- popstate监听地址栏的变动：

    ```
    window.addEventListener('popstate', e => {
        todo--list
    })
    ```
- push方法

    ```
    window.history.pushState(stateObject,title,url)
    ```
- replace方法

    ```
    window.history,replaceState(stateObject,title,url)
    ```
- 代码结构以及更新视图的逻辑与hash模式基本类似，只不过将对window.location.hash()直接进行赋值window.location.replace()改为了调用history.pushState()和history.replaceState()方法。

#### 3. 两种模式比较

一般的需求场景中，hash模式与history模式是差不多的，根据MDN的介绍，调用history.pushState()相比于直接修改hash主要有以下优势：

- pushState设置的新url可以是与当前url同源的任意url,而hash只可修改#后面的部分，故只可设置与当前同文档的url

- pushState设置的新url可以与当前url一模一样，这样也会把记录添加到栈中，而hash设置的新值必须与原来不一样才会触发记录添加到栈中

- pushState通过stateObject可以添加任意类型的数据记录中，而hash只可添加短字符串

- pushState可额外设置title属性供后续使用


#### 参考链接

[vue：路由实现原理](https://segmentfault.com/a/1190000014822765?utm_source=tag-newest)