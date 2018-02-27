# HTTP协议

>1.http协议的主要特点
```
HTTP协议是无状态、无连接的。

就是发送过去的请求,下次再发送,服务器单从HTTP协议上是无法做到,识别这个请求是否是刚才那个请求的。
```
>2.http协议的组成部分

```
 Http协议,是由请求报文和响应报文组成的,
 
 请求报文包括: 请求行、 请求头 、 空行 、 请求体
 响应报文包括: 状态行、 响应头 、 空行 、响应体
 
 请求行包含 : 请求方法 、 请求地址 、 HTTP协议版本号
 请求头包含 : 很多的keyvalue值,告诉服务器我要什么
 
 状态行包含:  状态码、 HTTP协议版本号 、 状态信息OK
 响应头包含:  很多键值对、一些服务器的信息
 
```
>3.http方法

- GET     获取资源
- POST    传输资源
- PUT     更新资源
- DELETE  删除资源
- HEAD    获得报文首部


>4.post和get请求的区别

- 1.get参数通过url传递,post放到请求体中
- 2.get请求在url中传递参数是有长度限制的,而post没有。(2kb,浏览器各不相同,太长会被浏览器截断,因为http协有规定) 所以url拼接的参数不能太长,否则会被浏览器截断。
- 3.get请求会被浏览器主动缓存,而post不会,除非手动设置
- 4.get在浏览器回退时是无害的,而post会再次提交请求





>5.http状态码

- 1XX : 指示信息
- 2XX : 成功
- 3XX : 重定向
- 4XX : 客户端错误
- 5XX : 服务端错误

```
200 : 请求成功
206 : 客户端发送了一个带有Range头的get请求,服务器完成了它
当视频音频文件很大时

301 : 永久重定向
302 : 临时重定向
304 : 浏览器有缓存

400 : 客户端有语法错误
403 : 资源禁止被访问,没有权限
404 : 请求资源不存在

500 : 服务器发生错误
```
>6.什么是持久链接

HTTP一般情况下,是无连接、无状态的
当使用Keep-Alive模式,是可以实现持久链接的,HTTP1.1版本支持


>7.HTTP2.0

## Web通信

>1.什么是同源策略,有什么限制?

同源策略: 就是浏览器的一个安全机制,不是同一个源的话没有权利去操作另一个源的文档。
跨域: 协议号、域名、端口号不同就是跨域。

同源策略限制包括:

- 无法获得cookie/localStorage/indexDB的获取
- 无法获得另一个源的DOM
- ajax请求限制,ajax只能做同源的通信

>2.前后端如何通信

一共常见就3种:

- Ajax,但是Ajax本身只能做同源下的通信
- 使用HTML5提出的WebSocket协议通信 : 不限制同源策略
- CORS:支持跨域通信,也支持同源策略。是一个新的通信标准

```
WebSocket协议被提出,主要是HTTP协议只能由客户端向服务端发送请求。服务端如果有什么变动无法主动通知客户端。
如果你有这个实时通讯的需求,可以考虑用WebSocket技术。
它的协议标识符是ws,不受同源策略限制,客户端可以和服务器任意通信,
```

```
CORS需要客户端和服务器同时支持,目前所有客户端都支持该机制。听说兼容性有问题?

COSRS就是新增了一组 HTTP 首部字段Access-Control-Allow-Origin,允许服务器去设置哪些源可以访问服务器。

然后客户端在发起请求时,如果是非简单请求的话,会先发起一个options预检查请求,如果是这个请求OK,就真正的发请求
```

>3.如何手写原生ajax

- 考察XMLHttpRequest对象的工作流程。
- 考察你是否去思考IE兼容性的问题。(看你思维是否周全)
- 事件的触发条件
- 事件的触发顺序

```

var xhr = XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject('Microsoft.XMLHTTP');  // 考虑IE兼容性

xhr.open('GET','index.php')
xhr.send()

xhr.onload = function(){
    if(xhr.status === 200 || xhr.status === 304) {
            res = xhr.responseText;    // 拿到后端返回的字符串
         if (typeof res == 'string') {
             res = JSON.parse(res);    //  把JSON转化成对象
         }
    }
}



```


>4.跨域的5种方式

- JSONP
- Hash
- Window.postMessage
- WebSocket
- CORS