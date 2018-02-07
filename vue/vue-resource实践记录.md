# vue-resource和axios实践记录

Vue-resource是一个异步请求插件

它的请求API时rest风格设计的，提供了7中API

* get(url, [options])
* head(url, [options])
* delete(url, [options])
* jsonp(url, [options])
* post(url, [body], [options])
* put(url, [body], [options])
* patch(url, [body], [options])


vue-resource重点掌握以下几点

- get请求
- post请求
- jsonp请求
- 全局拦截器
- 提取请求的共同前缀


## get请求和post的请求的区别

使用get请求和post请求都是用this.$http.get() / this.$http.post()的方式

每一个vue实例都挂载了$http对象,所以大概vue-resource插件是把$http对象写到了Vue构造函数的prototype上了。

get请求的参数和Post请求参数都是一个对象的形式,但是get请求这个对象是params:{}的形式,post是直接传递一个obj

但是这里说的都是最常用最简易的配置,你还可以继续传递参数,比如可以设置请求头里传递token,或者自定义其他字段

## jsonp请求


跨域的jsonp实际上就是一种用scirpt标签实现的get请求  

通过拼接一个请求到src里面，传递一个callback给后端

后端拿到callback后，会把结果封装到callback里面去，扔到前端。

因为前端在内存里面定义了callback这个方法，所以会收到这个结果。


```
        this.$http.jsonp('https://order.imooc.com/pay/check').then((response) => {
            this.msg = response.data
        })
```
## 全局拦截器


通过interceptors的push方法,这个方法传递一个回调

回调的参数是request和next回调

next回调表示,下一步,也就是执行到这个next回调函数内了的话,就是响应已经回到客户端了

这样我们就可以在next函数外做请求之前的Loading

next函数内做一个请求的错误统一处理

```
Vue.http.interceptors.push((request,next) => {
    console.log('请求前');
    next((response) => {
     console.log('响应后');
    })
})
```

## 提取请求的共同前缀

全局配置
Vue.http.options.root = 'http://192.168.2.74:81/api'

或者组件内配置
```
    http(){
        root : 'http://192.168.2.74:81/api'
    }

```

## axios和vue-resource的对比

目前vue-resource已经停止更新,比较流行的是vue-axios,它也是基于axios的,这里我对axios和vue-resource做了一些对比

首先他们的api是十分类似的,

比如都提供了7、8种发起请求的方式,主要区别是axios没有jsonp的请求方式
比如他们两者传递请求参数,获取响应的数据都是用response.data,这些也都是几乎一致的

需要注意的不同

- axios是暴露在window下的对象,并没有帮你挂在到vue实例上
- axios的全局拦截器的使用api的不太一样
- axios和vue-resource的使用95%都是类似的,但是存在一点区别


1.我们可以自己把axios挂在到vue实例上,也就是Vue构造函数的prototype上,或者引用vue-axios可能更规范。
2.全局拦截器的写法

```
           axios.interceptors.request.use((config) => {
                console.log('请求之前,可以做loading处理');
                return config
            })

            axios.interceptors.response.use((response) => {
                console.log('response init');
                return response
            })
```









