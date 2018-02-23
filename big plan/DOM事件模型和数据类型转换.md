## DOM事件
    
1.DOM事件级别的了解
```
- dom0标准: element.onclick = function(){}
- dom2标准: element.addEventListener('click',function(){},true)    指定了是冒泡还是捕获,默认是捕获true
- dom3标准: 增加了更多的事件类型
```

2.DOM事件模型、并说出DOM事件流的完整过程

```
DOM事件模型: 捕获/冒泡

DOM事件流:

 捕获 : window -- document -- html -- body -- 父元素 -- 子元素 -- 目标元素
     
 冒泡 : 反过来
```


3.event对象里常见的属性和应用

```
event.preventDefault()    点击a标签会自动跳转,如果我们不希望它跳转的话,通过这个方法,可以阻止a标签的默认跳转行为

event.stopPropgation()    你给父元素和子元素绑定了事件A和B,点击子元素时,如果不阻止冒泡,会同时触发事件A和B。

event.stopImmediatePropagation()  事件响应优先级的API

event.currentTarget : 表示ul
event.target : 表示li

给window绑定keydown,当 event.keyCode == 13 时,说明点击了回车键



```

4.如何自定义事件,这个和Vue的Vuex和父子组件通信结合回答。

```
    var ul = document.querySelector('ul')

    var eventObj = new Event('sayHiEvent')
    ul.addEventListener('sayHiEvent',function () {   // 自定义事件的绑定
        console.log('hi lvziwei');
    })

    ul.dispatchEvent(eventObj)  // 自定义事件的触发,注意传递参数是实例eventObj


```

## 数据类型转换

JS一共有7种数据类型,其中基本数据类型6种,引用数据类型1种

基本类型: string number boolean null undefined symbol 

引用类型: object


#### 显式数据转换

> Number()
 
- number : 还是number
- string : 分3个情况: 如果是''字符串转化为0 ; 如果是数值型字符串转化成对应数值 ; 如果是乱七八糟的字符串转化为NaN
- boolean : true就是1,false就是0
- undefined : 转化为NaN
- null : 转换为0
- object:
 
```
先调用obj.valueOf(),返回值若是基本数据类型,则对它Number(); 返回值若是复杂数据类型,则对它调用.toString()。

调用.toString(),返回值若是基本数据类型,则对它Number() ; 返回值若是复杂数据类型,则报错
```
 

> String()

- number: 直接对number加''
- string: 直接加''
- boolean: 直接对true或者false加''
- null: 直接对null加'' 
- undefined: 直接对undefined加''
- object: 

```
先调用对应的obj.toString,如果是基本数据类型,就对它String(),如果是复杂数据类型就对它valueOf()

调用valueOf(),如果是基本数据类型,就对它String(),如果是复杂数据类型就报错

```

> Boolean()

```
除了下面5个返回false,其他都是true

0 '' undefined null NaN 是false
```



#### 隐式数据转换

- 四则运算
- 逻辑语句判断(if语句或者三目运算符)
- Native调用(调用console.log和alert)

面试题:

(1) [] + []

```
    对[]+[],这里因为是四则运算,等于隐式把数组转化成Number类型了
    []转Number,先调用valueOf(),返回[],[]是复杂数据类型,所以调用toString(),返回''
    
    '' + '' 
    
    结果还是''
```

(2) [] + {}

```
   因为是四则运算,所以还是需要做隐式转化,把[]和{}转化成Number类型。
   转化规则是先调用valueOf(),得到是是自身,因为是复杂数据类型,所以继续调用toString()
   
   [].toString() 是 ''
   {}.toString() 是 '[object Object]'
   
   所以'' + '[object Object]' = '[object Object]'  

```

(3) {} + []   这里的{}被解析成了花括号,而不是obj

```
    这里的{}被解析成块级作用域
    所以就是对[]转化成Number,先调用[].valueOf() 得到 []
    [].toString() 得到的是 ''
    +'',相当于把''转化成数字,所结果是0

```

(4) {} + {}

```
    {}.valueOf()的结果是{},因为是复杂数据,所以需要继续执行toString()
    {}.toString()得到的结果是'[object Object]'
    
    '[object Object]' + '[object Object]' = '[object Object][object Object]'
    

```


(5) true + true

```
    1 + 1 = 2
```
(6) 1 + {a:1}

```
    对{a:1}转化成数字类型,先调用valueOf()得到的结果是{a:1},因为是复杂数据类型,所以继续调用toString()
    得到'[object Object]'
    
    1 + '[object Object]' = '1[object Object]' 

```

 