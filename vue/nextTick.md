### 原理

- 简单来说，Vue在修改数据后，视图不会立刻更新，而是等同一事件循环中的所有数据变化完成之后，再统一进行视图更新。
```
//改变数据
vm.message = 'changed'

//想要立即使用更新后的DOM。这样不行，因为设置message后DOM还没有更新
console.log(vm.$el.textContent) // 并不会得到'changed'

//这样可以，nextTick里面的代码会在DOM更新后执行
Vue.nextTick(function(){
    console.log(vm.$el.textContent) //可以得到'changed'
})
```
[查看图片](https://upload-images.jianshu.io/upload_images/12693563-a8a7b9c46eddd449.png?imageMogr2/auto-orient/strip|imageView2/2/w/423/format/webp)

#### 事件循环解析：

1. 第一个 tick（图例中第一个步骤，即'本次更新循环'）：

首先修改数据，这是同步任务。同一事件循环的所有的同步任务都在主线程上执行，形成一个执行栈，此时还未涉及 DOM 。
Vue 开启一个异步队列，并缓冲在此事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。

2. 第二个 tick（图例中第二个步骤，即'下次更新循环'）：

同步任务执行完毕，开始执行异步 watcher 队列的任务，更新 DOM 。Vue 在内部尝试对异步队列使用原生的 Promise.then 和 MessageChannel 方法，如果执行环境不支持，会采用 setTimeout(fn, 0) 代替。

3. 第三个 tick（图例中第三个步骤）：

此时就是文档所说的

#### 简单总结事件循环：

同步代码执行 -> 查找异步队列，推入执行栈，执行Vue.nextTick[事件循环1] ->查找异步队列，推入执行栈，执行Vue.nextTick[事件循环2]...
总之，异步是单独的一个tick，不会和同步在一个 tick 里发生，也是 DOM 不会马上改变的原因。

### 什么时候需要用的Vue.nextTick()

- 你在Vue生命周期的created()钩子函数进行的DOM操作一定要放在Vue.nextTick()的回调函数中。原因是什么呢，原因是在created()钩子函数执行的时候DOM 其实并未进行任何渲染，而此时进行DOM操作无异于徒劳，所以此处一定要将DOM操作的js代码放进Vue.nextTick()的回调函数中。与之对应的就是mounted钩子函数，因为该钩子函数执行时所有的DOM挂载和渲染都已完成，此时在该钩子函数中进行任何DOM操作都不会有问题 。

- 在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的DOM结构的时候，这个操作都应该放进Vue.nextTick()的回调函数中。

- 具体原因在Vue的官方文档中详细解释：
```
Vue 异步执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。Vue 在内部尝试对异步队列使用原生的Promise.then和MessageChannel，如果执行环境不支持，会采用setTimeout(fn, 0)代替。
```

```
例如，当你设置vm.someData = 'new value'，该组件不会立即重新渲染。当刷新队列时，组件会在事件循环队列清空时的下一个“tick”更新。多数情况我们不需要关心这个过程，但是如果你想在 DOM 状态更新后做点什么，这就可能会有些棘手。虽然 Vue.js 通常鼓励开发人员沿着“数据驱动”的方式思考，避免直接接触 DOM，但是有时我们确实要这么做。为了在数据变化之后等待 Vue 完成更新 DOM ，可以在数据变化之后立即使用Vue.nextTick(callback)。这样回调函数在 DOM 更新完成后就会调用。
```

### 实例如下

```
//点击获取元素宽度
<div id="app">
    <p ref="myWidth" v-if="showMe">{{ message }}</p>
    <button @click="getMyWidth">获取p元素宽度</button>
</div>

new Vue({
  el: '#app',
  data:{  
        showMe:false,
        message:''
        }, 
    methods:{
      getMyWidth() {
        this.showMe = true;
        //this.message = this.$refs.myWidth.offsetWidth;
        //报错 TypeError: this.$refs.myWidth is undefined
        this.$nextTick(()=>{
            //dom元素更新后执行，此时能拿到p元素的属性
            this.message = this.$refs.myWidth.offsetWidth;
      })
    }
    }
})
```

```
//点击按钮显示原本以 v-show = false 隐藏起来的输入框，并获取焦点。
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>获取焦点</title>
<style type="text/css">
    .closesou{font-size:30px;color:red;cursor: pointer;}
</style>
<script src="https://cdn.bootcss.com/vue/2.2.2/vue.min.js"></script>
</head>
<body>
<div id="app">
    <div class="soubox">
      <button class="showsearch" @click="showsou">搜索</button>
      <div v-if="showit">
        <input type="text" name="" id="keywords">
        <div class="closesou" @click="hidesou">&times;</div>
      </div>
    </div>
</div>
<script>
new Vue({
  el: '#app',
  // created:function(){
  //    this.$nextTick(function () {
  //         // DOM 更新了
  //         document.getElementById("keywords").focus()
  //       })
  // },
  data:{  
        showit:true
        }, 
    methods:{
      showsou(){
        this.showit = true
        this.$nextTick(function () {
          // DOM 更新了
          document.getElementById("keywords").focus()
        })
      },
      hidesou(){
        this.showit = false
      }
    }
})
</script>
</body>
</html>
```
[Vue之nextTick 的原理和用途](https://www.jianshu.com/p/b8b35ccf6c60)