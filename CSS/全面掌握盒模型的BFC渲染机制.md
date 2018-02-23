# 前端人人都要懂的盒模型BFC渲染机制

## 为什么我们说,前端工程师有必要需要了解BFC渲染机制?

因为如果你一个前端压根没听说过BFC,那你是如何理解下面这几个css现象的呢?

>现象一:  垂直方向上元素margin的合并现象

首先,有父子嵌套关系的2个元素,代码和示例如下:

![margin1](./margin1.png)

```
    <style>
        .father {
            width: 200px;
            height: 200px;
            background: skyblue;
        }
        
        .son {
            width: 100px;
            height: 100px;
            background: red;
        }
    </style>
    

  ---- html部分--- 
  
    <body>
        <div class="father">
            <div class="son"></div>
        </div>
    </body>

```

然后,我们给子元素添加一个margin-top: 50px时

```
      .son {
            width: 100px;
            height: 100px;
            background: red;
            margin-top: 50px;
        }
```

我们神奇的发现父子元素同时"掉下来了50px",如图所示

![margin2](./margin2.png)

***所以这里的问题是: 为什么2个div一起向下掉呢?***

>现象二: 垂直方向上元素margin的合并现象

现在,我们有2个兄弟关系的元素,代码和示例如下:

![margin3](./margin3.png)

```
    <style>
        .bother1 {
            width: 100px;
            height: 100px;
            background: skyblue;
        }
        
        .bother2 {
            width: 100px;
            height: 100px;
            background: cadetblue;
        }
    </style>
    
     ---- html部分--- 
     
    <body>
        <div class="bother1"></div>
        <div class="bother2"></div>
    </body>

```

然后,我们给上边的元素添加 margin-bottom:50px , 下边的元素添加 margin-top : 50px。

```
        .bother1 {
            width: 100px;
            height: 100px;
            background: skyblue;
            margin-bottom: 60px;
        }
        .bother2 {
            width: 100px;
            height: 100px;
            background: cadetblue;
            margin-top: 40px;
        }

```
如图所示:

![margin4](./margin4.png)

如果你眼力不错,或者亲自试试,会发现2个div之间设置了100px的距离,但是他们现在实际的间距却是50px。

***所以这里的问题是: 为什么2个div的间距是50px,而非100px呢?***

>现象三: overflow:hidden,可以清除浮动造成的副作用

一对父子关系的div,父元素的高度是通过子元素的高度撑开的,如图

![float1](./float1.png)

```
    <style>
        .father {
            width: 150px;
            border: 2px solid red;
        }
        .son {
            width: 100px;
            height: 100px;
            background: skyblue;
        }
    </style>
    
     ---- html部分---
          
    <body>
        <div class="father">
            <div class="son"></div>
        </div>
    </body>
```

然后,我们给子元素float:left之后,子元素脱离的文档流,于是父元素的高度为0了,如图

```
     .son {
            width: 100px;
            height: 100px;
            background: skyblue;
            float : left;
        }

```
![float2](./float2.png)

这个现象,我相信大家都知道如何解决,不就是需要"清除浮动"吗? 

我这里想说的是,"清楚浮动"有2种原理,一类是clear: both,一类就是依靠BFC原理.
 
***所以这里的问题是: 为什么给父元素添加 overflow: hidden 就可以"清除浮动"呢?***

> 现象四: overflow:hidden 配合浮动,可以实现2栏自适应布局

如图所示,我们已经实现了左侧固定300px,右侧自适应的布局

***所以这里的问题是: 为什么添加 overflow: hidden 和 浮动就可以实现2栏自适应布局呢?***

![wrapper](./wrapper.png)

```
    <style>
        .wrapper, * {
            padding: 0;
            margin: 0;
        }

        .left {
            width: 300px;
            height: 100px;
            background: red;
            float: left;
        }

        .right {
            height: 100px;
            background: skyblue;
            overflow: hidden;
        }

    </style>
    
     ---- html部分---
    
    <div class="wrapper">
        <div class="left">left</div>
        <div class="right">right</div>
    </div>
```



好了,以上四个看似毫无关系的"奇葩"现象,不知道是否理解出现这些现象的原因呢? 

如果回答不上来,那就建议你好好看下这篇文章了。

当然如果你没有见过这些现象的,那你就赚到了,这么多"奇葩"问题,你看一篇文章就全解决了,省了你不少力气呢!




## CSS盒模型


- CSS盒模型的基本概念: 盒模型分为W3C盒模型和IE盒模型,他们的区别是计算width和height的方式不同,IE盒模型的width是从border开始计算的。
- 如何设置CSS盒模型的类型 : 通过CSS3的 box-sizing:border-box就可以设置为IE盒模型了,默认是W3C盒模型


## BFC渲染机制

### BFC基本慨念

BFC是英文缩写,翻译为"块级格式化上下文"。

说白了BFC就是一种css盒模型的渲染规则。既然说了是渲染规则,那你自然需要去记住这些规则了,没法解释为什么。

### BFC渲染规则

BFC其实有很多渲染规则,我们这里说4条比较重要的规则,知道这些规则,你就可以回答上面的4个现象了。

- 规则1: 创建了BFC的元素中,在垂直方向上的margin会发生重叠。根元素<html>就是一个BFC元素 (这个可以解释margin重叠)
- 规则2: BFC元素在页面上是一个独立的容器,外面的元素和里面的元素互不影响,所以BFC元素不会和浮动的元素重叠(这个可以解释两栏自适应)。
- 规则3: 计算BFC元素的高度时,里面浮动元素的高度也会参与计算 (用来解释overflow:hidden可以清除浮动)

### 普通元素如何创建BFC

首先我们根元素html,就是最大的BFC容器。

创建BFC的方式很多,常见包括:

- float不为none的都可以
- overflow: hidden  / auto
- position: 不为static 、 relative都可以
- display: table-cell ... 表格相关的

不过我觉得用到最多的还是
```
overflow : hidden

```
毕竟其他的position float display都是很容易影响页面布局的,我们一般也不想为了创建BFC区域就引发页面布局的变动吧。

### 补充

不知道各位看了BFC的渲染规则和创建方式后,是否能够自行去解释前面的4个现象了呢?

补充2点:

- 关于margin的重叠规则。在现象二中,他们的重叠规则是,margin-bottom和margin-top会重叠,重叠之后取较大的margin值
- 关于"清除浮动"的说法。实际上"清除浮动"的说法不太准确,因为清除浮动,你直接删掉float:left就行了。更准确的说是"闭合浮动",或者说清除浮动带来的副作用。









