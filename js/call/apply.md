> #### 手写call

```

Function.prototype.mycall = function(obj) {
 
    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
       args.push('arguments[' + i + ']');
    }
    obj = obj || window;
    obj.fn = this;
    var result = eval('obj.fn('+ args +')’);
    delete obj.fn;
    return result;
}
```

**需要注意的点**

- 如果第一个参数为null，this指向window
- 函数调用是有返回值的

> ####手写apply

```

Function.prototype.myapply = function(obj, arr) {
 
    obj = obj || window;
    obj.func = this;
    var result;
    if(!arr) {
        result = obj.func();
    } else {
        var args = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            args.push('arr[' + i + ']');
        }
        result = eval('obj.func('+ args +')');
    }
    delete obj.func;
    return result;
}
```