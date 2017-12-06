# 全局api

### 2017/11/28  学习Vue.extend()  Vue.nextTick()   Vue.Set()

1. Vue.extend() 传入 参数对象,返回 一个实例构造器(用这个构造器new出来的实例,都是有一些扩展的实例)

注意: 区分Vue.extend()和 new Vue() 和 Vue.component()之间的区别


(1)  new Vue() 和 Vue.extend()
new Vue()就是创建出一个普通的Vue实例, 
而我们用Vue.extend()返回的是一个构造器也可以创建vue实例,只不过这种Vue实例,是带有一些拓展的Vue实例


```
可以看到,下面这个例子中,new extendInstance()得到的Vue实例被挂在#app上面,这个实例,就自带了'<div>被new出来的拓展实例</div>',这个就是拓展的地方

<div id="#app"> </div>

<script>

    const extendInstance = Vue.extend({
        template: '<div>被new出来的拓展实例</div>'
    })

    new extendInstance().$mount('#app')

</script>

```
(2) Vue.extend() 和 Vue.component()

Vue.extend()可以搞出一些预定义的vue实例,而如果我们想复用vue实例或者说vue组件,就可以选择把它注册为全局的组件,这时候,可以用Vue.component

```
// 注册组件，传入一个扩展过的构造器
Vue.component('my-component', Vue.extend({ /* ... */ }))
// 注册组件，传入一个选项对象 (自动调用 Vue.extend)
Vue.component('my-component', { /* ... */ })
// 获取注册的组件 (始终返回构造器)
var MyComponent = Vue.component('my-component')

```

2. Vue.nextick()  在下次dom更新后,执行回调。

如例子: 之所以需要nextTick(),data选项里的属性都是被watch监控的,当我们修改data属性时,并不是立即就更新的,而是被放到一个watcher异步队列中,当前任何空闲时,才会去执行。

例子中,我们改变了data选项里的属性,然后查看dom文本内容,发现没有成功的被修改

而在nextTick()的回调里就可以被成功修改了。

还有一些应用场景,比如使用swiper时,好像要求这个dom容器必须存在,你可以用nextTick来保证dom出现后,再执行其他操作
```
<div id="app">
    <example></example>
</div>
</body>
<script>
    Vue.component('example', {
        template: '<span>{{ message }}</span>',
        data: function () {
            return {
                message: '没有更新'
            }
        },
        mounted () {
            this.updateMessage();
        },
        methods: {
            updateMessage: function () {
                this.message = '更新完成'
                console.log(this.$el.textContent) // => '没有更新'
                this.$nextTick(function () {
                    console.log(this.$el.textContent) // => '更新完成'
                })
            }
        }
    })
    new Vue({
        el:'#app'
    })
</script>
</html>


```

3.Vue.Set()  这个API的存在的原因,是为了有时候Vue无法检测到属性被添加

```
<div id="app">
    {{message.sex}}
</div>
</body>
<script>

    new Vue({
        el: '#app',
        data(){
            return {
                message: {name: 'ziwei', age: 18}
            }
        },
        mounted(){
            // this.message.sex = 'man'   // 失败。Vue没有检测到后添加的属性,页面无法显示
            Vue.set(this.message, 'sex', 'man')         
        }
    })
</script>

```

### 2017/11-29  今天学习Vue.directive()  Vue.filter()  Vue.delete()

1. Vue.directive() 全局注册自定义指令

使用场景: 在Vue里,代码的复用和抽离,一般是用组件来做,但是如果对dom元素做一些底层操作,可以选择注册自定义指令

自定义指令的生命周期有5个,bind() unbind() insert() update() componentUpdated(),可以在不同周期做不同的逻辑

```
<div id="app">
    <div v-jspang="color" id="aaa">
        {{num}}
    </div>
    <p>
        <button @click='jia'>加分</button>
    </p>
    <p>
        <button onclick='unbind()'>解绑</button>
    </p>
</div>

<script type="text/javascript">

    function unbind(){
        app.$destroy();
    }

    //自定义指令
    Vue.directive('jspang',{
        bind:function(el,binding,vnode){//被绑定
            /**
             var s=JSON.stringify;
             el.innerHTML =
             'name:'        + s(binding.name) +'<br>' +
             'value:'       + s(binding.value) +'<br>' +
             'expression:'  + s(binding.expression) +'<br>' ;
             **/
            el.style='color:'+binding.value;


            console.log('1 - bind');
        },
        inserted:function(){//绑定到节点
            console.log('2 - inserted');
        },
        update:function(){//组件更新
            console.log('3 - update');
        },
        componentUpdated:function(){//组件更新完成
            console.log('4 - componentUpdated');
        },
        unbind:function(){//解绑
            console.log('5 - unbind');
        }

    })

    var app=new Vue({
        el:'#app',
        data:{
            color:'green',
            num:10
        },
        methods:{
            jia:function(){
                this.num++;
            }
        }
    })
</script>


```

2. Vue.filter()  全局注册过滤器

用法如下,我自定义了一个全局过滤器,可以将值保留两位小数
注意: 

 - 过滤器只可以出现在2个地方, 双花括号里,或者v-bind里
 - Vue2.0中所有的内置过滤器都被删除了
 - 过滤器可以串联
 - 过滤器可以是全局的也可以是局部的

```

<div id="app">
    <div>{{message | two}}</div>
</div>

<script type="text/javascript">
    Vue.filter('two',(val) => {
        return val.toFixed(2)
    })
    let app = new Vue({
        el: '#app',
        data(){
            return {
                message: 2
            }
        }
    })
</script>


```

3. Vue.delete()

跟Vue.set()类似,都是如果对象是响应式的,为了确保删除属性后,视图更新,可以用这个Vue.delete()

这个api存在的意义跟Vue.set()相同,都是为了避开Vue不能检测属性被删除的限制

但是既然已经是响应式的了,Vue就可以检测到这个属性,不太清楚这个api的使用场景。文档只是说这个api不常用


### 2017/11/30 今天学习 Vue.component() Vue.use()  Vue.mixin()

1. Vue.component() 就是注册全局组件,或者获取全局组件

如何注册呢? 看例子,一般是要跟Vue.extend()配合使用
```
<div id="app">
    <haha></haha>
</div>


Vue.component('haha', Vue.extend({
    template: '<div>我是哈哈组件</div>'
}))

new Vue({
    el: '#app'
})

```

2. Vue.use() 

使用插件。 Vue的插件必须有install方法。

如果这个插件是一个对象,那这个对象必须有install方法,如果是一个函数,就直接把这个把这个函数当做install方法

install方法将作为Vue的参数调用


```
import vue-router from 'vue-router'

Vue.use('vue-router')


```

3. Vue.mixin()

全局注册混合对象,需要谨慎使用,一旦注册,将会对之后创建的所有 Vue对象造成影响

如例:
```

Vue.mixin({
    mounted(){
        console.log('haha')
    }

})

new Vue({
    el : '#app'
})

这个之后创建的所欲的Vue对象,在moutned()钩子里,都会打印console.log('haha')

```


注意区分Vue.mixin 和 在Vue.prototype上添加属性、方法

https://segmentfault.com/q/1010000008554425/a-1020000008556804
