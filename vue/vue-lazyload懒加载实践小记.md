# vue-lazyload懒加载实践小记

今天实践了一下vue的一个懒加载插件vue-lazyload

安装
```
npm install vue-lazyload --save
```

在main.js这个入口文件引用,并设置loading图片

```
import VueLazyLoad from 'vue-lazyload'  // 引入lazyload图片

Vue.use(VueLazyLoad,{
  loading: 'static/loading/loading-spin.svg'
})

```

最后,使用需要注意的是,你还需要将之前的图片:src改为v-lazy

它会在你的图片被加载出来loading,加载后显示正确的图片路径

```
    <div class="pic">
        <a href="#"><img v-lazy="'static/' + item.prodcutImg" alt=""></a>
    </div>
```