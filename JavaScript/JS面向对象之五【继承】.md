# JS面向对象之五 【继承】

我们已经准备了很多前置知识,包括
- 原型链,对象和对象之间的关系
- this,对象和函数之间的关系
- new, 用函数批量创建特定的对象的语法糖

> JS面向对象的前世今生

我们说,面向对象是一种写代码的套路。因为如果满足 封装、继承、多态就是面向对象的话,那JS天生就有这3个特点。

那我们JS搞的这个面向对象其实是模仿java的面向对象,因为网景公司在发明JavaScript时,希望依托java让自己的语言火起来,
为此必须吸引java开发者来使用,所以要模仿java搞出相当于class的东西出来。

好的,现在JS和Java已经诞生了。但是***Java中的继承可以通过类实现,而JS中没有类,JS里的继承是通过原型链实现的。***

至此,我们做的事情就是,让java里用类能做的事情,我们JS也必须想办法搞出来,而且尽可能的像java (就ES6里class的语法糖)


> ***明确需求: 我们JS要实现的就是,java的类能做的事情,我们也要能做。***

## 首先,JS为了模拟类创建对象的功能,搞出了构造函数。

构造函数就像class一样可以,创建特定的对象,但是java程序员认为你这构造函数,不是类

因为java的类可以这样写,也就是子类可以继承父类

 ```
 class Human extend Animal
 
 ```
 但是JS最初没有extend这样的语法糖,来实现子构造函数继承父构造函数
 
 > ***所以,JS没法用extend关键字来直接继承,于是苦思冥想出了其他方法来模拟子类继承父类***
 
 ## 于是,我们学习JS是如何模拟子类继承父类的
 
 JS里没有类,所以我们肯定看的是JS是如何让子构造函数继承父构造函数的

### 版本1: 让Soldier继承Human

```
      function Human(options) {
          this.名字 = options.name,
          this.肤色 = options.肤色
      }
      Human.prototype.eat = function () {}
      Human.prototype.drink = function () {}
  
  
      function Soldier(options) {
          Human.call(this,options)   // 为了得到human里的自有属性
          this.生命值 = 42
          this.ID = options.id
      }
      Soldier.prototype.__proto__ = Human.prototype    // 为了得到human里的公有属性
      Soldier.prototype.攻击力 = 5
      Soldier.prototype.兵种 = '美国大兵'
      Soldier.prototype.攻击 = function () {}
      Soldier.prototype.防御 = function () {}
      Soldier.prototype.死亡 = function () {}
  
      var soldier = new Soldier({name:'ziwei',肤色:'yellow',id:'8'})
      console.log(soldier);  

```

> 这个方案是OK的,问题在于__proto__不允许在生产环境使用,文档明确有要求的,影响性能

### 版本2 : 不用__proto__,让Soldier继承Human


其实这个思路是很对的,JS肯定要用__proto__才能继承。

现在的问题就是,我要用__proto__,但是JS不让我用。

所以JS开发者就十分拧巴的用new来用__proto__,怎么用呢,看下面代码:

```
Soldier.prototype.__proto__ = Human.prototype
```
改成
```
function Fn(){}

Fn.prototype = Human.prototype

Soldier.prototype = new Fn()

```

怎么解释呢? 

- 因为 Soldier.prototype是Fn构造函数的实例,

- 所以 Soldier.prototype.__proto__ ==== Fn.prototype

- 而 Fn.prototype === Human.prototype

- 推论  Soldier.prototype.__proto__  === Human.prototype



完整的Soldier继承Human

```

      function Human(options) {
          this.名字 = options.name,
          this.肤色 = options.肤色
      }
      Human.prototype.eat = function () {}
      Human.prototype.drink = function () {}
  
  
      function Soldier(options) {
          Human.call(this,options)              //  这里是继承Human里的自有属性
          this.生命值 = 42
          this.ID = options.id
      }
      function Fn(){}
      Fn.prototype = Human.prototype
      Soldier.prototype = new Fn()                // 成功继承Human的共有属性 ,私有属性不要           
      
      Soldier.prototype.攻击力 = 5
      Soldier.prototype.兵种 = '美国大兵'
      Soldier.prototype.攻击 = function () {}
      Soldier.prototype.防御 = function () {}
      Soldier.prototype.死亡 = function () {}
  
      var soldier = new Soldier({name:'ziwei',肤色:'yellow',id:'8'})
      console.log(soldier);  


```

### 版本2.1: ES5提供Object.create()的语法糖,让开发者避免借助new,这种拧巴的方式来用__proto__

> 介绍Object.create()

- 接收一个参数obj1,return一个结果obj2
- obj2会继承obj1的属性。也就是obj2.__proto__ = obj1

```

      function Human(options) {
          this.名字 = options.name,
          this.肤色 = options.肤色
      }
      Human.prototype.eat = function () {}
      Human.prototype.drink = function () {}
  
  
      function Soldier(options) {
          Human.call(this,options)              //  这里是继承Human里的自有属性
          this.生命值 = 42
          this.ID = options.id
      }
      
      Soldier.prototype = Object.create(Human.prototype)    // 这里继承 Human 的共有属性,ES5的方法
      
      Soldier.prototype.攻击力 = 5
      Soldier.prototype.兵种 = '美国大兵'
      Soldier.prototype.攻击 = function () {}
      Soldier.prototype.防御 = function () {}
      Soldier.prototype.死亡 = function () {}
  
      var soldier = new Soldier({name:'ziwei',肤色:'yellow',id:'8'})
      console.log(soldier);  

```







