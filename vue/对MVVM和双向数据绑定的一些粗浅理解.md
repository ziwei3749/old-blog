# 对MVVM和双向数据绑定的一些粗浅理解

## MVC于MVVM

从名称上直观的看，mvc是model view controller ，而mvvm，则是把controller换成了viewModel


>前端MVC的特点：

- 1.model和view分离
- 2.controller控制dom
- 3.模仿了后端一些MVC模式


>MVVM框架的特点就是：

- 1.model绑定view
- 2.没有控制器的概念，也不需要操作dom了，
- 3.数据驱动、组件化


## 对Vue的理解的纠正

首先要知道vue并不是框架。

但是vue结合自身生态,可以构成一个灵活的、渐进性框架


## 我对于双向数据绑定的一些粗浅理解

vue双向数据绑定的实现，那ES5的一个api，Obect.defineProperty（）起到了关键作用

双向数据绑定分2个方面

- 视图修改,数据跟着改变。这个我们通过input的onkeyup就可以模仿,并不神奇。
- 数据修改,视图跟着改变。这个比较神奇,是通过ES5提供的Object.defineProperty()里的get set方法来实现的。


下面是一个简易的双向数据绑定

```
<body>
<input type="text" id="userName">
<br>
<span id="userText"></span>

<script>
    var obj = {}

    Object.defineProperty(obj,'username',{
        get:function () {
            console.log('get触发');
        },

        set: function (val) {
            console.log('set触发',val);
            document.querySelector('#userText').innerText = val    // obj属性改变时,触发set,我们给dom赋值
            document.querySelector('#userName').value = val
        }
    })

    document.querySelector('#userName').addEventListener('keyup',function (e) {
        obj.username = e.target.value    // 用户修改input是,我们修改数据,也就是obj属性

    })
</script>
</body>
```





具体来说就是，当用户修改视图时，触发Keyup事件，在回调里我们可以去修改对象的属性值。

当对象的属性值被修改，会触发set方法，在set方法里，我们可以去做dom赋值操作。
