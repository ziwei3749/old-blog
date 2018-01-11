# JS面向对象之四 【new】 (创建特定对象的语法糖)

为了讲清楚面向对象,实际上是需要几个前置知识的。
包括前面的几篇文章【原型链】 【this】 和今天要说的【new】

还是先说结论: ***new只是一个语法糖,这个语法糖被设计出来,使用场景是批量创建对象***

## 从场景说起: 假设这个世界没有new,我们如何批量创建一百个士兵obj?

通篇文章,我们都是在实现这个需求,不断优化,最终实现一个new语法糖。

类似这样的士兵ojb
```
var 士兵 = {
    ID: 1,
    兵种: '美国大兵',
    攻击力: 5,
    生命值: 42,
    攻击: function(){console.log('攻击')},
    防御: function(){console.log('防御')},
    死亡: function(){console.log('死亡')},
}
```

### 版本v1.0: 最直觉的方法当然是循环100次
```
var 士兵
var 士兵们 = []
for (var i = 0 ; i < 100 ; i++){
     士兵 = {
        ID: 1,
        兵种: '美国大兵',
        攻击力: 5,
        生命值: 42,
        攻击: function(){console.log('攻击')},
        防御: function(){console.log('防御')},
        死亡: function(){console.log('死亡')},
    }
    士兵们.push(士兵)
}

最终我们得到了array士兵们
```

根据第二篇文章 [JS面向对象之二【原型链】(对象和对象的关系)](./JS面向对象之二【原型链】(对象和对象之间的关系).md),我们知道这里的攻击、防御、死亡都是匿名函数,在内存空间都会占据空间。
这些函数的内容完全相同,300个匿名函数却要占用300个内存空间,十分浪费。

自然地,我们就想到JS的原型链就是专门解决这个问题的,我们把共同的属性放到__proto__里,所有士兵的__proto__都指向相同的内存空间。


### 版本v1.1: 优化内存空间,使用__proto__保存共同的属性

```
var 士兵
var 士兵们 = []

for (var i=0 ; i<100 ; i++){
    士兵 = {
        ID: 1,
        生命值: 42
    }
   士兵.__proto__ = 士兵共
   士兵们.push(士兵)
}

var 士兵共 = {
    兵种: '美国大兵',     // 如果兵种和攻击力也是一样的话,那么也需要放到__proto__里去共享
    攻击力: 5,
    攻击: function(){console.log('攻击')},
    防御: function(){console.log('防御')},
    死亡: function(){console.log('死亡')},
}

最终依旧得到了array士兵们

```
(以上代码可粘贴运行)
现在我们省掉了之前297个匿名函数所占用的空间。让他们的__proto__都指向'士兵共'这个内容空间就OK了

### 版本v1.2: 变量名称优化成英文

```
var soldier
var soldiers= []

for (var i=0 ; i<100 ; i++){
    soldier = {
        ID: 1,
        生命值: 42
    }
   soldier.__proto__ = soldierCommon
   soldiers.push(soldier)
}

var soldierCommon = {
    兵种: '美国大兵',     // 如果兵种和攻击力也是一样的话,那么也需要放到__proto__里去共享
    攻击力: 5,
    攻击: function(){console.log('攻击')},
    防御: function(){console.log('防御')},
    死亡: function(){console.log('死亡')},
}


```

### 版本v2.0: 封装成createSoldier函数,避免意大利式面条,让代码更加集中。

````

    var soldierCommon = {
        兵种: '美国大兵',     // 如果兵种和攻击力也是一样的话,那么也需要放到__proto__里去共享
        攻击力: 5,
        攻击: function(){console.log('攻击')},
        防御: function(){console.log('防御')},
        死亡: function(){console.log('死亡')},
    }
    
    function createSoldier(){
        var obj = {
            ID: 1,
            生命值: 42
        }
        obj.__proto__ = soldierCommon
        return obj
    }
    
------分割线以上是创建构造函数, 分割线以下,是使用构造函数------

    var soldiers = []
    for(var i = 0 ;i<100 ; i++){
        soldiers.push(createSoldier()) 
    }

````

### 版本v2.1: 让开发者一眼就知道soldierCommon和createSoldier是有关系的,让代码进一步集中


````

    // 让soldierCommon成为构造函数的属性(因为函数也是obj,当然可以有属性)
    // soldierCommon可以改叫xxx,只是一个名字,只是为了让这个对象和createSoldier产生联系
    
    createSoldier.xxx = {
        兵种: '美国大兵',     
        攻击力: 5,
        攻击: function(){console.log('攻击')},
        防御: function(){console.log('防御')},
        死亡: function(){console.log('死亡')},
    }
    
    function createSoldier(){
        var obj = {
            ID: 1,
            生命值: 42
        }
        obj.__proto__ = createSoldier.xxx
        return obj
    }
    
------分割线以上是创建构造函数, 分割线以下,是使用构造函数------

    var soldiers = []
    for(var i = 0 ;i<100 ; i++){
        soldiers.push(createSoldier()) 
    }

````

那么现在还可以优化吗? 上面这段代码,唯一的缺点就是xxx不太容易理解。

所以,JS之父为了容易理解将这个xxx,也就是共有属性的引用,统一叫做prototype,虽然prototype对于中国人来说依旧不好理解。

### 版本v2.2: 完美的代码,将xxx改为prototype (只是一个名字,但是JS之父将它命名成了prototype)


````
    
    createSoldier.prototype = {
        兵种: '美国大兵',     
        攻击力: 5,
        攻击: function(){console.log('攻击')},
        防御: function(){console.log('防御')},
        死亡: function(){console.log('死亡')},
    }
    
    function createSoldier(){
        var obj = {
            ID: 1,
            生命值: 42
        }
        obj.__proto__ = createSoldier.prototype
        return obj
    }
    
------分割线以上是创建构造函数, 分割线以下,是使用构造函数------

    var soldiers = []
    for(var i = 0 ;i<100 ; i++){
        soldiers.push(createSoldier()) 
    }

````


好了,仔细看看上面的代码,还有什么可优化的地方吗?

内存、变量名、代码集中都做了优化,也就是范例式的代码。

那么,JS之父也认为这个就是最好的代码,他希望,当JS开发者们需要批量创建特定对象时,都这样写。

于是,JS之父创造了new这个语法糖。

### 版本v3.0 让我们看看JS之父创造的new语法糖

```
 function Soldier(){
     this.ID  = 1
     this.生命值 = 42
 }
 
 Soldier.prototype.兵种 = '美国大兵'
 Soldier.prototype.攻击力 = '65
 Soldier.prototype.攻击 = function(){console.log('攻击')}
 Soldier.prototype.防御 = function(){console.log('防御')}
 Soldier.prototype.死亡 = function(){console.log('死亡')}
 
 ------分割线以上是创建构造函数, 分割线以下,是使用构造函数------
 
 var soldiers = []
 for(var i = 0 ;i<100 ; i++){
     soldiers.push( new Soldier()) 
 }

```
### 对比v2.2 和 v3.0 这两个版本,就是new帮我们做的事情。

> v2.2 没有使用new语法的函数createSoldier
```
    function createSoldier(){
        var obj = {
            ID: 1,
            生命值: 42
        }
        obj.__proto__ = createSoldier.prototype
        return obj
    }
```
> v3.0 使用了new语法的函数Soldier
```
     function Soldier(){
     //   this = {}                                          JS偷偷做的第1个事情                               
     //   this.__proto__ = Soldier.prototype                 JS偷偷做的第2个事情 
     
        this.ID  = 1
        this.生命值 = 42
         
     // return this                                          JS偷偷做的第3个事情 
         
     }
     
     Soldier.prototype = {                                  JS偷偷给Soldier.prototype加了个constructor属性
        constructor : Soldier
     }
```     

> 对比一下,当你使用new语法,JS之父帮你做了什么事情 

- 第一 , JS帮你在函数里创建了一个this空对象,并且这个this指向是构造函数的实例
- 第二 , JS帮你让this的__proto__ 指向 构造函数的原型,也就是prototype
- 第三 , JS帮你偷偷return了 this
- 第四 , JS偷偷给Soldier.prototype加了个constructor属性

> 其他注意事项

- 构造函数首字母大写,习惯写法
- __proto__不能在生产环境出现,因为会严重影响性能
- 用new调用的时候,也没法用call了,因为JS之父帮你call了,this的指向就像你见到的指向实例


所以使用了new,就可以让代码没有废话。

你只需要设置对象的自有属性和共有属性。
JS帮你创建空对象,帮你搞定this指向,帮你改变this的__proto__,帮你return this










 