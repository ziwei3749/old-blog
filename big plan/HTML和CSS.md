## 答案

### HTML
- 1.DOCTYPE是什么意思?

DOCTYPE的意思就是document type,文档类型声明。我自己的理解就是,它就是告诉浏览器,我这个页面是用哪个版本的html写的,让浏览器正确的渲染 HTML

我了解到的就是,浏览器渲染方式分为怪异模式和标准模式,如果不写doctype文档类型声明,浏览器就使用怪异模式渲染,可能会出一些奇怪的旧版本的问题。

- 2.HTML5新增的所有属性

 新增了一些更加语义化的标签,视频和音频标签
 
 新增了一些选择器和classList类名操作
 
 设置全屏显示的API:  requestFullscreen  exitFullscreen
 
 新增了localStorage和sessionStorage
 
 新增了postMessage这样跨域方式
 

### CSS
    
- 1.在宽高不确定的情况下,水平垂直居中的4种实现方式,以及他们的区别。

- 2.两栏自适应 / 三栏自适应的5种实现方式,以及区别比较

- 3.css盒模型的慨念以及区别,css中如何切换2中盒模型模式

CSS有2中盒模型,分别是IE和模型和W3C标准盒模型。区别就是IE盒模型中盒子的宽高从border开始计算,而标准盒模型的宽度是不包括padding和border的

切换盒模型模式的方法,可以通过CSS3的box-sizing属性,设置box-sizing:border-box,切换成IE盒模型了。

- 4.根据盒模型的BFC,解释边距的重叠

```
(1) 解释margin重叠 : 我们知道两个兄弟div,垂直方向上会发生重叠就是因为BFC,那哪里来的BFC呢? 因为根元素html就是一个BFC,所以里面的元素会有BFC内部元素的特点。

(2) 解释overflow:hidden可以清除浮动 : 就是因为overflow:hidden后形成BFC,里面浮动的元素也参与高度的计算。
 
(3) 解释两栏自适应:  2个div,左侧固定、右侧自适应。就左侧左浮动,右侧overflow:hidden形成BFC,两者就不会重叠,所以自适应了

```



- 5.详细的解释BFC

BFC翻译成中文是块级格式化上下文。和堆叠上下文一下,我们不必知道它的定义,只需要知道一个元素如何创建BFC 、 BFC有什么特点就可以了。

> BFC的特点:
```
规则1: BFC内部的元素,垂直方向上会发生margin的重叠,html根元素就最大的BFC ( 可以解释为什么margin合并 )
规则2: BFC是一个独立的容器,里面和外面的元素互不影响,所以BFC元素和浮动的元素不会互相重叠 (解释两栏自适应的原理)
规则3: 计算BFC元素的高度时,内部浮动元素的高度也会被计算 (overflow:hidden 可以清除浮动的原理。)

```

> 如何创建BFC

```
1. float不为none的元素
2. position不为static或者relative的都可以
3. overflow: auto/hidden
4. display: table-cell等,一些表格相关的

```

- 6.堆叠上下文对z-index的影响

>记住1:堆叠顺序: (如何记不住,可以现场测试,展示你的实践能力就好了)
```
    background
    border
    块级
    浮动
    内联
    z-index: 0
    z-index: +
如果是兄弟元素重叠，那么后面的盖在前面的身上。
```

记住堆叠上下文是什么? 我们说不清,但是它具有以下特点就行了。

>记住2:一个元素是相对定位或者绝对定位,并且有z-index时,这个元素就是堆叠上下文 (z-index默认是auto,为auto时不行)
如果一个元素是fixed,z-index都需不要给,就触发了层叠上下文

堆叠上下文出现后,对z-index有什么影响?
>记住3:

比较层级关系时,在同一个堆叠上下文里,我们就比较background、border、块级、浮动、内联。如果非同一个堆叠上下文的话,你不仅要看各自的z-index,还需要看父元素的z-index

比如a比b的z-index高,但是B比A的z-index高,那么B里所有的元素都压A里面的元素一头。

就比如,你就阿里巴巴董事会的最低级的董事,我是搬砖小分队里最牛逼的包工头,那肯定也是低级董事,压我这个高级包工头一头的。




- 7.css3用法细节
  + (1) 新增选择器
  + (2) 盒模型
  + (3) 背景和边框、文字特效、字体图标
  + (4) ***过渡 transition***
  + (5) ***转换 transform***
  + (6) ***动画 animation***
  + (7) flex布局
  + (8) rem和媒体查询
  
  
  (1) 新增选择器
  
  包括  
  ```
  :nth-child(1n)
  :nth-child(2n)
---  
  :first-child
  :last-child
---
  选中所有,被选中的Input
  :checked
---
  属性选择器
  [title] {}            // 选中有title属性的元素
  [title = lvziwei] {}  // 选中有title属性,并且title属性为lvziwei的元素
  
  ````
  
  (2) 盒模型
  
  默认是box-sizing的默认是content-box
  从border开始计算宽度,这种方式,看IE盒模型的计算方式一样。
  
  box-sizing: border-box
  box-sizing: content-box
  
  (3)背景、边框、字体图标、文字特效
  
  >***background相关知识***
  
  举例默认顺序,第一个是地址 、 第二个是不重复 、 第三个设置水平方向的对齐方式 、 第四个设置垂直方向的对齐方式
  ```
  background : url('bg.png') no-repeat left top
  
  ```
  
  background-size: 可以设置背景的尺寸。可以设置具体像素、百分比或者cover/contain
  ```
  background-size: 70px 50px    设置背景图片的宽高
  background-size: 100% 100%    设置背景图片,宽100% 高100%
  background-size: cover        设置背景图片覆盖 div (就算你这个div很小,我图片也不变形,可能覆盖你div)
  background-size: cotain       设置背景图片包含 div (不管你div多大,我图片哪怕变形,也自适应你的div)
  
  ```
  
  background-origin: 设置背景图片从哪个区域开始绘制,origin嘛。默认是padding-box,背景图片从padding开始显示。
  ```
  background-origin: padding-box   // 默认从padding开始绘制背景图片
  background-origin: content-box   // 从content开始绘制背景图片
  background-origin: border-box    // 从边框开始绘制背景图,但是border如果不是透明色,默认是会被边框挡住的
  ```
  
  background-clip: 设置从哪个区域开始显示。它无法解决背景图片从哪里开始绘制,但是不管你怎么绘制,clip可以控制背景图从哪些区域开始显示。
  
  ```
    background-origin: padding-box   // 默认从padding开始绘制背景图片
    background-origin: content-box   // 从content开始绘制背景图片
    background-origin: border-box    // 从边框开始绘制背景图,但是border如果不是透明色,默认是会被边框挡住的
  
  ```
  
   区分  background-origin 和 background-clip 
  
  - 前者是从哪里作为origin开始绘制,后者是从哪里开始显示。
  - 默认值不同。background-origin的默认值是padding-box,background-clip默认是border-box
  
  background-image:可以写多个背景图片,用逗号分隔
  ```
    background-image: url('1.png') , url('2.png')
  ```
     
  >***border相关知识***
  
  border-radius: 圆角。可以设置半分比,也可以设置具体的px,设置px第一个值表示水平半径,第二个值表示垂直半径
  
  也可以设置分别设置border的四个角。
  
  box-shadow : 设置边框阴影
  
  前2个参数必需。分别表示水平阴影长度,垂直阴影的长度
  后面4个选填参数,分别表示 模糊程度、阴影大小、阴影颜色、内阴影还是外阴影。
  
  ```
  box-shadow: 10px 10px 10px 10px red 
  ```
  
  >***字体图标相关知识***
  
  1. 矢量化：字体是矢量格式，因此能够轻松的适配不同的设备，而不必为不同分辨率的屏幕准备不同的图片资源。 
  2. 轻量性：字体相对于一系列的图片资源要更加的轻量，一旦图标字体加载了，图标就会马上渲染出来，不需要下载一个图像。可以减少HTTP请求，还可以配合HTML5离线存储做性能优化。 
  3. 灵活性：由于图标字体，本质上是一种字体而非一种图标，因此我们可以设置各种字体属性，例如通过设置font-weight, font-size, font-color来改变图标的粗细、大小、颜色。 
  4. 兼容性: 比较好
  
  登录一个可以制作font-face的网址,你可以在网站上挑选图标或者导入设计师给你的图标,然后生成四种格式的图片。
  
  用@font-face里设置font-family,用src引入那不同格式的文件。
  设置类名里带有icon的元素,font-family都是这个,之后就引用类名就可以了。
  
  ```
          @font-face {
              font-family: 'icomoon';
              src:  url('fonts/icomoon.eot?9splek');
              src:  url('fonts/icomoon.eot?9splek#iefix') format('embedded-opentype'),
              url('fonts/icomoon.ttf?9splek') format('truetype'),
              url('fonts/icomoon.woff?9splek') format('woff'),
              url('fonts/icomoon.svg?9splek#icomoon') format('svg');
              font-weight: normal;
              font-style: normal;
          }
  
          [class^="icon-"], [class*=" icon-"] {
              /* use !important to prevent issues with browser extensions that change fonts */
              font-family: 'icomoon' !important;
              speak: none;
              font-style: normal;
              font-weight: normal;
              font-variant: normal;
              text-transform: none;
              line-height: 1;
  
              /* Better Font Rendering =========== */
              -webkit-font-smoothing: antialiased;
              -moz-osx-font-smoothing: grayscale;
          }
  
          .icon-facebook2:before {
              content: "\ea91";
          }
  ```
  
  
>***字体特效相关知识***

text-shadow,设置文本阴影。参数含义跟box-shadow一样,表示水平阴影、垂直阴影、模糊程度、颜色

```
 text-shadow: 5px 5px 5px #FF0000;
```

text-overflow: ellipsis 可以设置文本溢出之后,有一个...

word-wrap:break-word  可以设置文本换行

word-wrap:keep-all    单词拆分换行

(4) ***过渡 transition***

transition我记得是有4个属性。

常用的,也是至少必须要有的2个属性是,过渡中要改变哪些属性,过渡时间是多少


```
transition: all 1s   // 也可以不用all,而是指明具体的属性
```
(5) ***转换 transform***

包括平移、旋转、缩放

transform:translate(-50%,-50%)   可以用于实现水平垂直居中

transform: rotate(30deg)         如果需要顺时针旋转的话是正值,逆时针旋转的话是负值

transform: scale(2,3)           如果需要放大,就用正值,缩小用负值。这个表示宽度放大到之前的2倍,高度放大到之前的3倍

 (6) ***动画 animation***
 
 用@keyframes定义你这个动画的名称,
 之后用这个动画的时候,就用animation: 动画名称 5s ; 跟过渡很相似 transition: all 1s
 
 ```
     <style>
         div {
             width: 50px;
             height: 50px;
             background: red;
             border: 2px solid black;
             padding: 40px;
             margin-top: 220px;
             margin-left: 200px;
         }
 
         @keyframes meFirst{
             from {
                 background: blue;
             }
             to {
                 background: yellow;
             }
         }
         .active {
             animation: meFirst 3s
         }
 
 
     </style>
 </head>
 <body>
     <div class="active"></div>
 </body>
 
 ````
 

(7) flex布局

flex布局有2个大的慨念,容器和子项目。

给一个元素设置display:flex,就可以成为"容器",每一个子项目如果你设置display:flex都可以成为容器

举例子来说,用flex实现水平垂直居中或者是一个上下固定,中间自适应的布局

注意:设为Flex布局后，子元素的float、clear、vertical-alian属性都将失效
    
    

(8) rem和媒体查询

@media关键字 + 媒体类型 + and关键字 + 查询条件
```
    @media screen and (min-width:480px) {
        屏幕宽大于480px,就执行这里的CSS
    }
    
```

rem的原理,就是所有的css单位改用rem,而rem是由html元素的font-size来决定。

所以当屏幕尺寸变化时,我们只需要修改html的font-size,对应的所有css单位的尺寸也响应的变化,从而实现的屏幕适配。

接下来,需要实现的就是,屏幕尺寸变化时,根据什么来修改html的font-size呢?

这个比例关系:

设计稿的宽度/100px = 测出来的屏幕宽度 / ? 
 
 ? = 测出来的屏幕宽度 * 100px / 设计稿



