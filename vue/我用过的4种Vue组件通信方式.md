# 我用过的4种Vue组件通信方式

重点是梳理了前两个,父子组件通信和eventBus通信,我觉得Vue文档里的说明还是有一些简易,我自己第一遍是没看明白。

- 父子组件的通信
- 非父子组件的eventBus通信
- 利用本地缓存实现组件通信
- Vuex通信


## 第一种通信方式:父子组件通信



### 父组件向子组件传递数据

- 父组件一共需要做4件事
    + 1.import son from './son.js' 引入子组件 son
    + 2.在components : {"son"} 里注册所有子组件名称
    + 3.在父组件的template应用子组件, <son></son>
    + 4.如果需要传递数据给子组件,就在template模板里写 <son :num="number"></son>
   
    
```
 // 1.引入子组件
 
     import counter from './counter'
     import son from './son'
```
    
```
// 2.在ccmponents里注册子组件

    components : {
        counter,
        son
    },
```
 ```
 // 3.在template里使用子组件
 
    <son></son>
 ```   
 ```
  // 4.如果需要传递数据,也是在templete里写
  
    <counter :num="number"></counter>

  ```
 
    
    
- 子组件只需要做1件事
    + 1.用props接受数据,就可以直接使用数据 
    + ***2.子组件接受到的数据,不能去修改。如果你的确需要修改,可以用计算属性,或者把数据赋值给子组件data里的一个变量***

```
   // 1.用Props接受数据
      props: [
               'num'
           ],
   
   ```
   
   ```
   // 2.如果需要修改得到的数据,可以这样写
      props: [
               'num'
           ],
     data () {
           return {
               number : this.num
           }
       },
   ```


### 子组件向父组件传递数据

- 父组件一共需要做2件事情
    + 在template里定义事件
    + 在methods里写函数,监听子组件的事件触发
    
```
// 1. 在templete里应用子组件时,定义事件changeNumber
      <counter :num="number"
                 @changeNumber="changeNumber"
      >
      </counter>
```

```
// 2. 用changeNumber监听事件是否触发
        methods: {
            changeNumber(e){
                console.log('子组件emit了',e);
                this.number = e
            },
        }
```
    
    
- 子组件一共需要1件事情
    + 在数据变化后,用$emit触发即可
```
// 1. 子组件在数据变化后,用$emit触发即可,第二个参数可以传递参数
        methods: {
            increment(){
                    this.number++
                    this.$emit('changeNumber', this.number)
                },
        }
```    
    

## 第二种通信方式: eventBus

eventBus这种通信方式,针对的是非父子组件之间的通信,它的原理还是通过事件的触发和监听。

但是因为是非父子组件的关系,他们需要有一个中间组件来连接。

我是使用的通过在根组件,也就是#app组件上定义了一个所有组件都可以访问到的组件,具体使用方式如下

使用eventBus传递数据,我们一共需要做3件事情

- 1.给app组件添加Bus属性 (这样所有组件都可以通过this.$root.Bus访问到它,而且不需要引入任何文件)
- 2.在组件1里,this.$root.Bus.$emit触发事件
- 3.在组件2里,this.$root.Bus.$on监听事件


```
// 1.在main.js里给app组件,添加bus属性
import Vue from 'vue'

new Vue({
  el: '#app',
  components: { App },
  template: '<App/>',
  data(){
    return {
      Bus : new Vue()
    }
  }
})

```

```
// 2.在组件1里,触发emit

increment(){
        this.number++
        this.$root.Bus.$emit('eventName', this.number)
    },

```

```
// 3.在组件2里,监听事件,接受数据

mounted(){
    this.$root.Bus.$on('eventName', value => {
        this.number = value
        console.log('busEvent');
    })
},

```

## 第三种通信方式: 利用localStorage或者sessionStorage

这种通信比较简单,缺点是数据和状态比较混乱,不太容易维护。

通过window.localStorage.getItem(key) 获取数据
通过window.localStorage.setItem(key,value) 存储数据

注意用JSON.parse() / JSON.stringify() 做数据格式转换。


## 第四种通信方式: 利用Vuex

Vuex比较复杂,可以单独写一篇

