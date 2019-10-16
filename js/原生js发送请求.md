#### 原生js发请求步骤

- 创建XMLHttpRequest对象(new)

- 连接服务器(open)

- 发送请求(send)

- 接收响应数据(onreadystatechange)

> **get方式**

```
   #1创建一个xhr对象
    var xhr = new XMLHttpRequest ( ) ;
    #2监听状态的改变
    xhr.onreadystatechange=function(){
          if(xhr.readyState===4){//请求成功
                    if(xhr.status===200){//响应成功
                          doResponse(xhr);//调用一个函数
                  }
            }


  }
    #3打开一个链接
    xhr.open('get','xxx.php',true);
    //1.请求方式  2.发送到哪去  3.是否为异步请求 是true 否 flase
    #4发送数据
    xhr.send(null);
```

> **post方式**

```
    #1创建一个xhr对象
    var xhr = new XMLHttpRequest ( ) ;
    #2监听状态的改变
    xhr.onreadystatechange=function(){}
    #3打开一个链接
    xhr.open('post','xxx.php',true);
    #3.5 修改请求消息头部
      xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
  
    #4发送数据
    xhr.send('uname=xxx&upwd=123');

```

> **readyState状态值**

- 0-未初始化，尚未调用open()方法；

- 1-启动，调用了open()方法，未调用send()方法； 服务器连接已建立；

- 2-发送，已经调用了send()方法，未接收到响应； 请求已接收；

- 3-接收，已经接收到部分响应数据； 请求处理中；

- 4-完成，已经接收到全部响应数据； 请求已完成；

#### 参考链接

[原生JS发送Ajax请求、JSONP](https://www.cnblogs.com/dingjiaoyang/p/11022412.html)
