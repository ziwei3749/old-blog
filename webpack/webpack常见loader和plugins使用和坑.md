# webpack常见loader和plugins的使用和坑

>目录

- 1.css引用图片
- 2.设置css分离和设置publicPath(css分离是指的不把css引入到js里,而是单独引入css的方式加载)
- 3.html中图片的打包
- 4.打包和分离less(设置不适用css in js)
- 5.打包和分离sass(设置不适用css in js)
- 6.使用postcss,实现自动添加css前缀
- 7.消除冗余无用的css代码
- 8.babel转义es6 / es7 /jsx
- 9.打包后的代码调试

## 1.css引用图片

(1) 需要使用的loader

- file-loader : 用来做src下的图片打包到dist后,所有图片路径的转换
- url-loader :  可以设置多大的图片自动转换成base64

(2) 使用loader的方法

loader只需要下载后在webpack.config.js配置,不需要引入

- 下载
```
npm install --save-dev file-loader url-loader
```

- 配置webpack.config.js
```
    module: {
        rules: [
            {
                test: /\.css$/,   // 找到css文件
                use: [{             // 使用哪些loader
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }],
            },
            {
                test: /\.(png|jpg|gif)/,
                use: [{
                    loader: 'url-loader',
                    options: {
                        limit : 5000      // 图片大于5000b,就自动把图片拷贝过去,如果小于5000b就转成base64
                    }
                }]
            }
        ]
    },
```

- 执行打包命令即可

```
webpack
```

(3) 注意点

- url-loader依赖了file-loader,所以实际上我们只需要下载url-loader即可
- 也不需要针对file-loader去配置,毕竟从src打包dist下,图片url的自动转换是必需的需求。
- url-loader的limit参数的默认单位是b,如果图片小于这个大小,会帮你自动转成base64,从而减少http请求,提高性能。

## 2.设置css分离和设置publicPath(css分离是指的不把css引入到js里,而是单独引入css的方式加载)

刚才我们是通过import将css in js的方法打包的
如果需要将css分离,单独打包成一个css文件, 需要使用插件:extract-text-webpack-plugin

(1) 需要使用plugin

- extract-text-webpack-plugin

(2) 使用extract-text-webpack-plugin

- 下载插件
```
npm install --save-dev extract-text-webpack-plugin

```
- 引入并且配置webpack.config.js

```
const extractTextPlugin = require('extract-text-webpack-plugin')   //css分离

```

```
同时插件要求,上面的css loader也需要按照插件要求修改

{
      test: /\.css$/,   // 找到css文件
      use: extractTextPlugin.extract({     // 修改的部分
          fallback: "style-loader",
          use: 'css-loader'
      })
      
      
       // use: [{             // 使用插件之前
       //     loader: "style-loader"
       // }, {
       //     loader: "css-loader"
       // }],

   },

插件里new一下
 new extractTextPlugin("/css/index.css")  // 打包后就在dist下生成css文件夹,下生成一个Index.css
```
- 执行打包命令

```
webpack
```

ok,现在打包之后,你会发现有这样的路径dist/css/index.css

```
#picture {
    background: url(9a87f14552b68ee2a4ef6974ede837a6.jpg);
    width: 500px;
    height: 300px;
}

```
但是问题是,index.css里的url路径不对(file-loader修改的路径只针对css in js),我们需要设置publicPath,告诉webpack去哪找css

在出口设置图片publicPath
```
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: '[name].js',       // [name]的作用是,出口文件的文件名,默认就是跟入口文件名对应。打包出来entry和entry2
        publicPath: "http://10.0.1.10:1717/"
    },

```

(3) 注意点

- extract-text-webpack-plugin插件除了需要设置plugin,还需要配置loader
- 通过publicPath修改路径,保证css被正确加载

## 3. HTML中的图片打包

实际上,webpack不推荐在html里写Img,但是我们可以通过loader去打包html里的图片

(1) 需要下载和使用loader
- html-withimg-loader
```
npm install --save-dev html-withimg-loader
```

(2) 配置html-withimg-loader

```
    {
                test: /\.(htm|html)$/i,
                loader: 'html-withimg-loader'
    }
```

(3) 注意点

- 没什么注意的,loader不需要引入


## 4.打包和分离less / sass ,以less为例

(1) 需要下载使用less / less-loader

- less
- less-loader
```
npm install --save-dev less-loader  less

```

(2) 配置过程
- 先写less代码

```
html里

<div class="less"></div>

```

```
less里

@base: #000;
.less {
  width: 300px;
  height: 200px;
  background-color: @base;
}
```

```
js里,用Import引入css

import css from './css/index.css';
import less from './css/black.less';
```
- 在webpack.config.js配置
```
在loader里配置less的

        {
                test: /\.less$/i,
                use: [{
                    loader: "style-loader"
                },{
                    loader: 'css-loader'
                },{
                    loader: 'less-loader'
                }]
            }
```

- 目前less代码是被引入到js里的,如果希望将less和css打包合并成一个index.css文件,依旧是使用extract-text-webpack-plugin

```
          {
                test: /\.less$/i,
                use: extractTextPlugin.extract({
                    use : [
                        {loader: 'css-loader'},
                        {loader: 'less-loader'},
                    ],
                    fallback: 'style-loader'
                })
            }
```

这样配置之后,我们less代码就会被合并到index.css里面

(3) 注意点

- sass除了下载sass / sass-loader,还需要下载node-sass,因为sass-loader依赖了它

## 4.使用postcss自动添加css属性前缀

(1) 使用loader

- postcss-loader
- autoprefixer

(2) 配置过程
- 在根目录创建postcss.config.js文件,它是postcss的配置文件(因为可以有不少配置项)

在postcss.config.js里写
```
module.exports = {
    plugins: [
        require('autoprefixer')
    ]
}
```

配置webpack.config.js,在rules里继承加loader


```
            {
                test: /\.css$/,   // 找到css文件
                use: extractTextPlugin.extract({
                    fallback: "style-loader",
                    use: [{
                        loader: 'css-loader'
                    },{
                        loader: 'postcss-loader'
                    }]
                })
            },
```

(3) 注意点

- 注意使用postcss的话,这里是单独创建了postcss.config.js这个配置文件的
- webpck的配置文件里也需要写postcss

## 5.消除无用的css

>使用场景

- 你使用了ui框架,大量css代码可能是无用的
- 你的css代码也存在一部分冗余代码

(1) 使用plugin

- purify-css 插件

(2) 配置webpack.config.js过程

- 引入
```
引入glob这个模块,以及插件purifycss-webpack
const glob = require('glob')    // node的模块

const purifyCssPlugin = require('purifycss-webpack');  // 消除冗余代码

```

- 配置
```
    plugins: [
        new purifyCssPlugin({
            paths: glob.sync(path.join(__dirname, 'src/*.html'))     // 搜索src下的所有html文件,没用到的css自动删除
        })
    ],
```

- 打包

```
webpack
```

(3) 注意点

- 注意我们使用时,引入了Node模块glob

## 6.babel转换ES6/ES7/ES8 react

(1) 可能需要用到的loader

- babel-core            // 核心包
- babel-loader
- babel-preset-es2015
- babel-presset-react
- babel-preset-env     // 转义es6 7 8

(2) 下载后配置

- 配置.babelrc
```
{
  "presets":["react","env"]
}
```
- 配置webpack.config.js

```
  {
            test: /\.(jsx|js)$/,
            use: {
                loader: 'babel-loader',
            },
            exclude: /node_modules/       // 去掉node_modules,里面的js代码不需要转换
  },
```

(3) 注意点

- .babelrc这个配置文件可以单独出来
- 需要对ES7/ES8可以使用babel-preset-env

## 7.如何代码调试

支持调试,设置很简单,需要在entry上面加一个配置

devtool: ''

它有四个取值
- source-map :  独立文件 报错会提示行数和字符,  但打包很慢
- cheap-moudle-source-map   : 独立文件,报错只提示行数,不提示字符 
- eval-source-map : 不生成独立文件,直接在js里生成map, 只用于开发阶段,
- cheap-moudle-eval-source-map : 


```
module.exports = {
    devtool: '',
    entry: {},
    output: {},
    module: {},
    plugins: [],
    devServer: {}
}
```



## 补充: 启动webpack时,如何让浏览器自动打开


只需要在Package.json里的script里配置 --open
```
  "scripts": {
    "server": "webpack-dev-server --open",
  },
```


