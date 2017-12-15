# JavaScript函数

## 定义

## 词法作用域

关于词法作用域,需要知道几个点:

- JS在执行代码的时候,简单来说可以分为2步,解析和执行。解析的时候JS要做词法分析 


## this&arguments

1.this是隐藏的第一个参数,而且必须是一个对象,如果不是对象,让会包装成对象

```
    fn.call()
    
    第一个参数是 this  
    第二个参数是arguments , 如果你不传递,就是[]
    
    可以把fn.call()理解为fn(), fn()就是阉割版的fn.call()
```   
    
2.为什么this必须是对象

因为this,被发明出的意义,就是成为函数和对象的羁绊

```
为了person.sayHi(),不穿参数,JS就知道是person要sayHi

而不必这样写person.sayHi.call(person)

```


## call & apply

- call和apply是函数正常的调用方式,第一个参数,必须显示的指定this,第二个参数是前面调用call函数的函数的参数


## bind

- bind就是返回一个新函数,这个新函数里的this由你指定,新函数的函数体跟之前一样

## 柯里化

   柯里化,就是把一个需要传入多个参数的函数,转换成只需要传入一个参数的函数,(只不过可能是多个需要传入一个参数的函数)
   
   本来是需要多个参数的函数,第一次调用时,其实什么都没做,只是return回来一个函数。
   
   比例说在前端模板引擎的里的应用
   
   ```
    var handlebar = function(template,obj){
        return template.replace('{{name}}',obj.name)
    }
    
    一个需要传递2个参数的函数,被柯里化之后
    
    var handlebar2 = function(template){
        return function(obj){
            return template.replace('{{name}}',obj.name)
        }
    }
    
   
   ```

## 回调函数

把函数作为参数的函数,叫做回调函数

## 构造函数

返回一个对象的函数,就是构造函数

## 箭头函数

当你需要内里的this,和外面的this一样时,就用箭头函数

因为箭头函数本身没有this,所以他就会去父作用域里去找this

## 高阶函数

在数学中,满足三个条件,就是高阶函数

- 把函数作为参数
    + array.sort((a,b) => {return a-b})
    + array.forEach((item) => {}) 
    + array.map((val) => {return val*2})      // 返回新数组,对原数组遍历一遍,return执行了某个逻辑元素
    + array.filter((val) => {return val>2})   // 返回一个新数组,对原数组遍历一遍,return符合条件的元素
    + array.reduce((a,b) => {a+b})            // 返回一个值,一般用来累加
- return一个函数
    + bind
- 同时满足上面2点

