> #### computed

1. computed内部的函数在调用时不加()。

1. computed是依赖vm中data的属性变化而变化的，也就是说，当data中的属性发生改变的时候，当前函数才会执行，data中的属性没有改变的时候，当前函数不会执行。

1. computed中的函数必须用return返回。

1. 在computed中不要对data中的属性进行赋值操作。如果对data中的属性进行赋值操作了，就是data中的属性发生改变，从而触发computed中的函数，形成死循环了。

1. 当computed中的函数所依赖的属性没有发生改变，那么调用当前函数的时候会从缓存中读取。

- computed 用来监控自己定义的变量，该变量不在data里面声明，直接在computed里面定义，然后就可以在页面上进行双向数据绑定展示出结果或者用作其他处理。举例：购物车里面的商品列表和总金额之间的关系，只要商品列表里面的商品数量发生变化，或减少或增多或删除商品，总金额都应该发生变化。这里的这个总金额使用computed属性来进行计算是最好的选择

```
computed: {
    reversedMessage () {
        return this.message.split('').reverse().join('') + this.number
    }
}
```
> #### watch

1. watch中的函数名称必须要和data中的属性名一致，因为watch是依赖data中的属性，当data中的属性发生改变的时候，watch中的函数就会执行。

1. watch中的函数有两个参数，前者是newVal，后者是oldVal。

1. watch中的函数是不需要调用的。

1. watch只会监听数据的值是否发生改变，而不会去监听数据的地址是否发生改变。也就是说，watch想要监听引用类型数据的变化，需要进行深度监听。"obj.name"(){}------如果obj的属性太多，这种方法的效率很低，obj:{handler(newVal){},deep:true}------用handler+deep的方式进行深度监听。

1. 特殊情况下，watch无法监听到数组的变化，特殊情况就是说更改数组中的数据时，数组已经更改，但是视图没有更新。更改数组必须要用splice()或者$set。this.arr.splice(0,1,100)-----修改arr中第0项开始的1个数据为100，this.$set(this.arr,0,100)-----修改arr第0项值为100。

1. immediate:true    页面首次加载的时候做一次监听。

1. 支持异步操作。

- watch一般用于监控路由、input输入框的值特殊处理等等，它比较适合的场景是一个数据影响多个数据

```
//监听普通对象

export default {
  name: 'test1',
  data () {
    return {
      number: 1
    }
  },
  created () {
    setTimeout(() => {
      this.number = 100
    }, 2000)
  },
  watch: {
    number (newVal, oldVal) {
      console.log('number has changed: ', newVal)
    }
  }
}
```

```
//监听对象
export default {
    data () {
        return {
            test: {
                text: ''
            }
        }
    },
   
    watch:{
        test: {
            deep: true,
            handler: function (newVal,oldVal){
                console.log('newValue', newVal);
                console.log('oldValue', oldVal.text);
            }
        }
    }
}
```

```
//监听对象的属性

export default {
    data () {
        return {
            test: {
                text1: '',
                text2: ''
            }
        }
    },
     
    watch:{
        'test.text1': {
            deep: true,
            handler: function (newVal,oldVal){
                // code
            }
        }
    }
}
```


> #### 二者应用场景

1. computed相当于多对一的关系，用多个data里面的属性值计算得出结果用computed返回。函数体内不能有异步操作，若想改变data属性值需要用get和set方法。

1. watch相当于一对多的关系，只要data里监听的属性值发生变化就会触发watch函数的执行，函数体内可以有异步操作。

1. computed应用场景：计算购物车价格。watch应用场景：搜索框搜索结果。
