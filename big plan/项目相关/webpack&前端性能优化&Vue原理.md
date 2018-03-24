# webpack&前端性能优化&Vue原理

## webpack相关的问题集合


- 说说你项目里是如何使用webpack的? 如何利用webpack提高页面的性能,提高构建速度的?
- 为什么选择webpack? webpack是解决什么问题而生的?  gulp/grunt 与 webpack的区别是什么?
- webpack 中loader和plugin区别？常见的loader和plugin
- 如果让你写一个写一个webpack loader,你的思路是什么
- webpack的dev-server的原理是什么?



1. 说说你项目里是如何使用webpack的? 如何利用webpack提高页面的性能,提高构建速度的?

- 1.选择合适的devtool:souce-map。一开始项目里的配置的是inline-souce-map. (它会给每一个文件名添加一个source-map的dataurl,是base64转码后的字符串。我自己也测试了几次不同配置)
阮一峰推荐的生产环境,cheap-module-eval-source-map,cheap-moudle-source-map

- 2.使用插件开启gzip压缩。使用compressionWebpackPugin
- 3.把CSS和JS分离打包,避免你仅仅修改了CSS部分,但是JS缓存也失效了。 使用的extract-text-webpack-plugin
- 4.使用插件去掉CSS冗余的代码。 因为你自己可能写了一些没有用的CSS代码,更多可能是引用UI库的无效的CSS代码。purifyCssPlugin



2. 为什么选择webpack? webpack是解决什么问题而生的?  gulp/grunt 与 webpack的区别是什么?

因为webpack虽然配置可能有些繁杂,但是用户和生态很多,人们踩过的的坑也多。比较主流的模块化解决方案嘛。

gulp和webpack是没有可比性的。

- gulp是一个优化前端工作流程的任务管理工具。它不像webpack提供模块化解决方案。
- gulp的工作方式时,在配置文件里,指明文件要做的步骤,先编译/压缩、再合并之类的步骤,然后输入gulp就执行这个流程。
- webpack的话,他会根据地配置文件,因为设置了入口、出口、loader/plugin的处理方法,他就会帮你把文件去做打包成浏览器可以解析的文件。

3. webpack 中loader和plugin区别？常见的loader和plugin

- 最直接的使用上的区别感觉。就是loader不需要引入,plugin需要require()引入。就不用下载了
- loader一般是文件处理,plugin一般是对整个构建流程处理。有一个插件可以把css单独打包,而不是和JS在一起。

4. 如果让你写一个写一个webpack loader,你的思路是什么

loader本质上就是接受字符串或者buffer,处理之后返回回来处理好的格式嘛。

比如可以px2rem的loader,可能就是用正则匹配到是px结尾的字符串,然后替换成rem。

5. webpack的dev-server的原理是什么?

dev-server直观作用就是,代码修改后不用刷新浏览器就能看到变化嘛。

用websocket让服务器和JS通信,当代码修改时,服务器可以监听到代码的变化,变化之后就让浏览器渲染着部分内容。

## 前端性能优化相关的问题集合

- 构建层面: 资源的合并和压缩 、 图片的优化处理
- 浏览器渲染层面
    + 根据render tree的形成过程。我们可以理解,那我们需要减少重绘和回流
    + 根据css/js的执行顺序、阻塞的规则。我们想到要如何安排代码的顺序
    + 浏览器存储
    + 一些技巧。懒加载、预加载的方式
- 网络层面
    + HTTP里,其实也是基于浏览器和服务器协商的缓存设置
- 服务器层面
    + 可能还涉及到一个服务端渲染
    
    
(1) 资源的合并和压缩,主要就是css/js/html的压缩、混淆、合并。需要注意的就是文件的合并可以减少HTTP请求,但是也会导致一些问题。
    + 比如合并一个超大的js文件,一开始资源加载速度可能会很慢
    + 缓存失效的问题。
    
(2) 图片优化处理。主要包括图片压缩 ; 选择合适的图片格式 ; 减少http请求数量处理 ; webp如果是安卓机的话。
    + base64转码的问题
    + 雪碧图,也是要分几个雪碧图的。所有图片集中到一个雪碧图,也可能导致这一行图片太大,加载也很慢
    
(3) 我们知道render tree的渲染过程后,知道他有Layout和paint绘制的过程,所以要减少这个过程。
    + 比如用className替换style
    + 也可以先隐藏这个dom元素,对它进行100次操作,然后显示出来。
    + 减少会触发重绘和回流的css属性。比如要平移的话,用translate替换left
(4) 了解到css 和 js的执行顺序、阻塞规则后。可以把css放到头部;js放到底部。
(5) 利用浏览器存储,可以做离线版本的产品。或者利用sessionStorage,让用户刷新页面也不丢失数据。
(6) 利用懒加载,预加载的机制,给用户一个好的体验
(7) 利用分级缓存策略
(8) 服务器渲染,来解决首屏渲染速度和seo的问题。
    


## Vue原理相关的问题集合