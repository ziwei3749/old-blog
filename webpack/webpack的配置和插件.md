# webpack3.x的插件和配置

## 几个小知识点

> 如何升级webpack

- 修改package.json里的webpack版本号
- 然后删除node_modules
- 执行npm install

> 手动建立demo的基本目录结构

- src文件夹: 用于开发环境
- dist文件夹: 用于生产环境

> 打包命令 (后面我们会使用webpack的配置文件来打包)

直接用命令指定打包的入口文件和出口文件的路径,2个参数,第一个是入口文件的路径,第二个要出口文件的路径

```
webpack src/entry.js dist/bundle,js
```

> live-server(本地开发常常有搭建临时服务的需求)

live-server集合了三个插件的功能

- http-server : 搭建服务器
- live-reload : 热更新
- opener : 自动打开浏览器

使用命令
```
live-server
```

## 插件与配置

本文主要介绍下面5个功能的使用和配置方法

- 一、入口和出口 (entry和output部分)
- 二、启动服务和热更新 (devServer部分)
- 三、用loader打包CSS文件 (module部分)
- 四、用plugins压缩JS文件 (plugins部分)
- 五、用plugin打包html文件 (plugins部分)

### 一、入口和出口(entry和output部分)

- 1.学习webpack.config.js的基本结构
- 2.入口配置
- 3.出口配置
- 4.多入口、多出口配置

#### webpack.config.js的基本结构

webpack.config.js是webpack的配置文件,通过export.module暴露接口出去

主要分5个部分: entry / output / module / plugins / devServer

```
module.export = {
    entry: {},
    output: {},
    module: {},
    plugins: [],
    devServer: {}
}

```

#### 学习entry入口配置

入口的配置就在entry对象里,写键值对就好了

属性是入口名,值是入口路径

```
module.export = {
    entry: {
        entry : './src/entry.js'    // 这里前面那个作为键的entry,是可以自己随意命名的
    },
    output: {},
    module: {},
    plugins: [],
    devServer: {}
}

```


#### 学习output出口配置

出口的配置,是写在output里,需要注意分为path和filename两个部分

path.resolve(__dirname) 是node的语法,表示当前文件的绝对路径

```
module.export = {
    entry: {
        entry : './src/entry.js'                    // 这里前面那个作为键的entry,是可以自己随意命名的
    },
    output: {
         path: path.resolve(__dirname,'dist'),      // 打包到这个dist文件夹下
         filename: 'bundle.js'                      // 打包出来的出口文件名称
    },
    module: {},
    plugins: [],
    devServer: {}
}

```


#### 学习多入口、多出口的配置方法

多入口的写法,就是增加键值对
多出口的写法,就是设置 filename: '[name].js',这样打包出来的文件名,是跟打包前的文件名相同的。不过是在dist文件夹下的文件。

```
module.export = {
    entry: {
        entry : './src/entry.js'                    // 这里前面那个作为键的entry,是可以自己随意命名的
    },
    output: {
         path: path.resolve(__dirname,'dist'),      // 打包到这个dist文件夹下
         filename: '[name].js'       // [name]的作用是,出口文件的文件名,默认就是跟入口文件名对应。打包出来entry和entry2
    },
    module: {},
    plugins: [],
    devServer: {}
}

```

### 二、启动服务和热更新(devServer部分)

- 1.使用webpack启动临时服务器,可以用webpack-dev-server这个包,需要用npm下载
```
npm install --save-dev webpack-dev-server

```

- 2.webpack.config.js里需要配置devServer的参数
```
module.export = {
    entry: {},
    output: {},
    module: {},
    plugins: [],
    devServer: {
        contentBase: '',      // 设置基础内容的路径
        host: '10.0.1.10',    // 设置主机号, 可以通过ifconfig查看自己本机的ip地址
        compress: true,       // 设置是否启用服务器压缩
        port: 1717            // 设置端口号
    }
}

```

- 3.在package.json里配置命令alias

```
  "scripts": {
    "server": "webpack-dev-server"
  },

```

- 4.启动服务,只需要执行命令npm run server

```
npm run server 
```

关于热更新:在webpack3.5之后安装webpack-dev-server后就实现热更新了

如果是之前的版本,需要去做一些配置


### 三、用loader打包css(module部分)

loader可以做的事情:

- 将sass编译成css
- 将ES6编译成浏览器兼容的语法
- 将typescript编译成js

这里我们只是将css打包到js里,需要用style-loader和css-loader

- 1.用import将css引入到js里

```
import css from './css/index.css';
```

- 2.用npm下载这2个loader

```
npm install style-loader --save-dev
npm install css-loader --save-dev
```

- 3.设置webpack的配置文件webpack.config.js

写法1:
```
    module:{
        rules: [
            {
                test: /\.css$/,   // 找到css文件
                use: ['style-loader','css-loader'],    // 使用哪些loader

            }
        ]
    },
```

这里webpack还提供了其他写法

写法2:

```
    module:{
        rules: [
            {
                test: /\.css$/,   // 找到css文件
                loader: ['style-loader','css-loader'],    // 使用哪些loader
            }
        ]
    },
```

写法3: 也是最常用的,因为可以给每一个loader单独添加参数

```
    module:{
        rules: [
            {
                test: /\.css$/,   // 找到css文件
                use : [{
                        loader: "style-loader"
                    },
                    {
                        loader: "css-loader"
                    }]
            }
        ]
    },
```

这时候你在重新npm run server,你会发现发现css已经被应用上了,打开源码你也看不到link标签,因为css被打包到js里了。

### 四、用plugins压缩js(plugins部分)

压缩js代码,需要用到uglifyjs插件,这个插件webpack内置的,所以不用下载,但是依旧需要在webpack.config.js里引入

```
const uglify = require('uglifyjs-webpack-plugin')  // 需要引入插件

```

之后需要在plugins里配置

```
    plugins:[
        new uglify()
    ],
    
```

执行webpack,就发现它已经打包了

可以看到打包后,entry1从20kb,压缩到7kb了,大概压缩到了之前的三分之一

### 五、用plugins打包html (plugins)

打包html的意思,就是在src下的index.html打包后,在dist目录下也生成html,另外也有一些配置项可以压缩html

需要下载插件html-webpack-plugins,并且在webpack.config.js里引入

```
npm install --save-dev html-webpack-plugins

```

在webpack.config.js里引入并且配置

```
const htmlPlugin = require('html-webpack-plugin')  // 打包html,需要下载再引入
```

```
    plugins:[
        // new uglify()
        new htmlPlugin({     
            minify: {
                removeAttributeQuotes: true             // 去掉属性的引号
            },
            hash: true,                                 // 每次给他不同的hash,就避免有缓存
            template: './src/index.html'                // 给一个需要被打包的html的路径
        })
    ],
```

最后在命令行执行webpack打包

```
webpack
```
