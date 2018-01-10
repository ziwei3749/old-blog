# JS面向对象之三【this】 (对象和函数之间的关系)

上一篇,谈了对象和对象的关系,现在我们谈谈对象和函数的关系

先说结论,也就是观点1

## 观点1: JS里函数和对象没有关系,JS之父通过this将他们强行关联起来

首先我们根据场景,去理解this的出现可以解决什么问题。

```
    var obj = {
        name : 'ziwei',
        sayName: function(x){
            return console.log('hello,' + x.name)
        }
    }
   
```

现在我们不用this,我们就只能这样调方法
obj.sayName(obj)

但是一般而言,我们用obj调用sayName,肯定就是希望让这个对象说自己的名字

为了能够obj.sayName(),让这个obj自己的传递进去,JS之父发明了this.


***this的出现,就可以解决obj.sayName(obj)时,不用自己传递obj进去,而是直接obj,sayName(),让JS会帮你传递obj参数进去***


你可以理解为JS做了一件事情,就是你每次调用函数时,都偷偷给你传递了一个参数,你用this就可以拿到这个参数。
```
    var obj = {
        name : 'ziwei',
        sayName: function(){
            return console.log('hello,' + this.name)
        }
    }
    obj.sayName()   
```
但是这样,其实对于新手很不友好。我obj.sayName(),根本没传递参数,那他是怎么输出obj的name的呢?

所以JS偷偷的传递this参数这件事情,我们将它显式的展出来就好理解了。


实际上,JS就是做了这样一件事情,显式的指定this是obj.  obj.sayName.call(obj)


回顾一下:

隐式模式:   obj.sayName()
显示模式:   obj.sayName.call(obj)


##  观点2: JS里所有函数都接受2个参数,第一个this,第二个是arguments

JS函数被调用时,一定会有这2个参数

如果你用call()调用函数,就是显式的传递this和arguments
如果你用()语法直接调用函数,那JS就去帮你偷偷的传递this。它怎么帮你传递呢? 这个问题暂时不管,下面会说。

JS其实为了用this,想了很多折中方案。
包括(1)函数调用有两种语法 , 把函数的参数划分为this和arguments


> 一个三段论

- 论点1: JS函数的参数的值,只有在传参时才确定的
- 论点2: this是函数的参数
- 推论: ***this的值,只有在函数调用时才确定***


> JS偷偷帮你传递参数的规则 (也就是你使用()语法调用时)

- 在全局作用域下调用函数,默认是window  (这个是历史遗留bug),或者你也可以理解为window对象调用的函数
- 哪个对象调用的,这个this就指向谁
- 构造函数里this指向构造函数的实例
- call、apply、bind都是显式的传递this了,不用多说
- 箭头函数里没有this,如果你非要写一个this,那这个this跟外面的this一致


## 做几个题目

```
这个this是谁?
function a(){
    console.log(this)
}

答案: 不确定,this是参数,函数没有调用,怎么确定参数的?
```


`````
function a(){
    'use strict'
    console.log(this)
}

a()

答案: undefined, 严格模式下, 全局作用域下this不再是window,而是undefiend


`````

新手不会this的主要原因,是不清楚函数的另一个调用方式call(),
 
因为用call()就是自己传递this, 用()就是JS偷偷帮你传递this,既然是JS按照他自己的规则,偷偷给你传递的,你肯定要懵逼搞不清的

就像手动档和自动挡开车一样

