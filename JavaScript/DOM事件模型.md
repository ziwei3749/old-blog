# DOM事件模型

DOM事件模型是什么? 其实说的就是事件冒泡和事件捕获。

### DOM事件历史

- dom0标准:  规则了btn.onclick = function(){}这种语法
- dom2标准: 规则了btn.addEventListener('click',function(){},true)    第三个参数true表示在捕获时触发,false表示在冒泡触发,默认是false
- dom3标准: 增加了大量事件,例如keyup

### DOM事件流的完整过程

先从上到下捕获,然后从下向上冒泡的过程,具体顺序就是

- 捕获过程 : window ---> document ---> html ---> body ---> 父元素 ---> 一直到目标元素
- 冒泡过程 : 目标元素 ---> 父元素 ---> body ---> html ---> document ---> window

### event对象常用的属性

```
1. event.preventDefault()  : 比如阻止a标签的默认事件
2. event.stopPropagation() :  阻止冒泡,比如父子元素都绑定了click事件,如果你不希望点击子元素,触发父元素的事件,那么就可以用这个
3. event.target  :  事件代理里,表示被点击的那个li
4. event.currentTarget : 事件代理里,表示当前绑定事件的元素,比如ul

```

### JS如何自定义事件

new Event() / new CustomEvent()两种方式可以自定义事件,一种是可以传递参数对象的,一种不能

自定义事件一般分三步:

- 通过构造函数,创建事件实例
- 绑定事件
- 触发事件


```
var eve = new Event('fakeClick')      // 创建一个事件实例

dom.addEventListener('fakeClick',function(){})     // 绑定fakeClick事件

dom.dispatchEvent(eve)                // 触发事件,这里需要注意触发事件,传递的不是事件名称,而是事件实例

```

使用 CustomEvent 没太大区别,注意传递参数的写法和接受参数的写法

传递参数用:{detail:obj}的形式
想用你传递来的参数需要通过detail属性: params.detail

```
var eve = new CustomEvent('fakeClick',{detail:paramsObj})      // 创建一个事件实例

dom.addEventListener('fakeClick',function(params){                   // 绑定fakeClick事件
        console.log(params.detail)
})     

dom.dispatchEvent(eve)                // 触发事件,这里需要注意触发事件,传递的不是事件名称,而是事件实例

```