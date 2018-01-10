# JS面向对象之二 【原型链】(对象和对象之间的关系)

注意这个系列文章,要经常站在JS之父的视角去思考。

***牢记我们的需求,我要在JS没有class的情况下,那么利用JS现有的东西,搞出类似class的东西。***

回一下JS有什么? 有7种数据类型:

- 6个基本数据类型 : string number boolean null undefined symbol
-  1个引用类型 :  obj (你需要知道前6个基本数据类型是存储在栈中的, 1个引用类型obj存储是的引用,它的值存储在堆中的.) 

对象分成3类: 普通对象 、数组对象 、函数对象。

思考问题1: 我们都知道array function都属于object,那么既然他们都是对象,一定有某些相同之处吧,对象和对象之间有什么关联呢?
  
如果说你没有思考过这个问题,那么可以换一个更具体的问题。

思考问题2: 打印这段代码,到浏览器控制台,你会发现我们空对象,"自带"了一个toString()? 

```
var obj = {
    name: 'ziwei',
    sex : '男',
    eat : function(){}
}
obj.toString()

```

问题1: array funciton obj 都是对象,那么为什么说他们都是对象,通过什么关联起来的?

问题2: 为什么一个空对象,"自带"toString()方法?
  
如果你不清楚以上问题的答案,恭喜你,这篇文章的主题【原型链】就可以清楚的解答你的疑惑。

## 原型链

### 什么场景下,需要原型链。

还是上面这段代码
```
var obj = {
    name: 'ziwei',
    sex : '男',
    eat : function(){}
}
obj.toString()

```
如果我们想自己实现,让obj,具有toString()怎么办? 直觉肯定是添加在obj上咯。

这样因为toString(),是函数对象,所以需要在内存中开辟一块空间,存放toString()

现在,我们要声明100个obj对象,他们都需要具备toString()怎么办?

如果循环100次给每个obj都添加toString(),需要在内存空间开辟100个空间,存在100个相同的toString(),势必造成内存空间的浪费,在JS被创造时,内存还是十分宝贵的资源。


所以JS之父想出一个办法。把对象需要共享的属性和方法,放到一个内存空间里,然后每一个对象都有一个属性都指向这一个空间。

所以原型链被发明,最朴素的想法就是节省内存空间。


### JS里如何设计原型链规则的呢?


1.所有对象都有的这个属性长得很奇怪,叫做 __proto__

```
var obj = {}
console.log(obj.__proto__ )  访问,你就可以查看所有object都共享的属性。
```

2.这个__proto__所指向的那块内存空间里的属性,并不是在var obj = {}时,才被添加进去的,而是一直都存在的。

```
通过 window.Object.prototype  你同样可以访问到这个共享的内存空间

```


3.__proto__里面还可以有__proto__,毕竟被称为原型链嘛。

之所以需要"链",你可以看这样一个例子

```
var arr = []
数组作为一个数组是有push()方法的, 可是我们查看 window.Object.prototype里并没有共享push方法

```
我们为了实现数组有push方法,但是不能让所有的对象都有push方法,为了实现这个需求,JS是这样设计的。

![内存图]('./neicun.png')

JS另外开辟了一个内存空间,用来专门存储数组的共有方法和属性。你可以用过window.Array.prototype来访问它

这样JS数组,arr通过__proto__继承了Array.prototype的方法,有通过Array.prototype.__proto__继承了Object.prototype上的访问。

类似的我们访问obj.name时,JS会在obj内部寻找name属性,如果没有,就会到__proto__中去寻找name,一直到Object.prototype如果还是没有name,就会返回undefined

所以这种依靠原型来继承的方式,很想一个链条,一层一层的向上寻找,所以叫做原型链,原型链的顶端就是Object.prototype






  

