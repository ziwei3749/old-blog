# vue-cli升级后没有dev-server.js如何做mock模拟数据踩坑记录

## 用Node的express实现mock数据模拟的思路

思路很简单
- 首先,我们需要一个.json文件,
- 然后本地启动express服务,用Node写一个接口,接口被访问时,Node将Json数据响应给前端

## 旧版本在dev-server.js里的写法

那么在旧版本的vue-cli生成的项目结构里,build/dev-server.js写的就是node启动服务的一些配置

```
var app = express()

var goodsData = require('../mock/goods.json')     // 引入json文件
var router = express.Router()                   
router.get("/goods/list", function (req,res) {     // 接口的地址是/goods/list
  res.json(goodsData)                              // 返回json数据
})

app.use(router)                                     // 使用router


```

这种方法其实就是在写node代码,自己给前端返回数据,来实现数据mock

## 新版本vue-cli实现mock数据

升级了vue-cli后,我们会发现dev-server.js这个文件找不到了。

但是,我们知道以前npm run dev 这个命令能启动服务,就是因为运行了dev-server.js。

那现在它是如何启动服务的呢,于是我打开了Package.json去查看

```
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "build": "node build/build.js"
  },
```

查找资料后,发现webpack-dev-server是一个插件,现在是用它来启动node服务的。

如果我们需要去配置服务,修改服务,插件自然也是要提供一些配置选项的。

所以,无非就是由自己写Node代码,改成了使用webpack插件,你通过插件的配置项,来返回Mock数据

根据文档,就是在webpack.dev.config.js里的devServer里去配置before函数

```
    // 这里是配置后端路由,返回response数据,做mock模拟
    before(app){
      app.get('/goods/list',(req,res,next) => {
        res.json(goodsData)
      })
    }

```


可以看到,写法更简单了,也不需要我们去直接写Node代码了。