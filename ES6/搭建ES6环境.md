# 搭建ES6环境

因为除了谷歌浏览器之外,其他浏览器对es6的支持还存在问题,所以要用babel把ES6转成ES5,确保能运行。

```

一共三个步骤

npm install -g babel-cli

npm install babel-preset-es2015

在根目录创建 .babelrc文件,并做好简单的JSON格式的配置



```
在终端输入 babel src/index.js -o dist/index.js   

这时候dist会多出已经被转换成ES5的index.js文件


```
为了简化每次的命令行语句,我们可以在package.json里的script字段修改

  "scripts": {
    "build": "babel src/index.js -o dist/index.js"
  },

这样下次需要转换ES6时,我们在命令行,输出npm run build即可
```