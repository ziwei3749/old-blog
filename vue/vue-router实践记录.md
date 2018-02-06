# vue-router实践记录

## 关于前端路由的理解

路由就是根据不同url匹配不同的页面内容,前端路由就是这个事情交给前端来做,而不是之前的让服务器根据url不同,渲染不同的html

## 前端路由的优缺点:

优点: 
- 因为不同每次都从服务器获取整个页面,所以用户体验好。

缺点:

- 不利于seo
- 使用浏览器的前进和后退时,会重新发起请求,没合理利用缓存
- 单页应用无法记住之前滚动的位置

## vue-router

- 动态路由
- 嵌套路由
- 编程式路由
- 命名路由

### 1 动态路由

在router.js这个配置文件里,new一个vue-router实例

配置的时候可以这样写

```
 /user/:username
```
通过$route.params可以获取到对应的到username参数,展示不同的页面

### 2 嵌套路由

 通过<router-view></router-view>和路由配置文件里表现的嵌套关系,就可以把对应的组件渲染到router-view里
 
 ### 3 编程式路由
       
也就是写JS代码，来实现页面的跳转,这里也可以看到vue-router跟window.history很像,实际上也是对history的封装

$router.push(‘name’)
$router.push({path:’’name})

$router.push({path:’name?a=123’})
$router.push({path:’name’,query:{a:123}})
$router.go(1)


注意: $router.push({path:’name’,query:{a:123}}) 传递字符串的这种路由参数,是通过$route.query来获取

### 4 命名路由

路由是可以设置name的,在你的路由配置文件里,跳转的时候,可以根据name来跳到对应的路由

