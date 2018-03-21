# JS模块化


JS有很多模块化的方案,主要还是因为JS自身没有模块化的语法或者规范。在ES6之前。

- AMD：requirejs 在推广过程中对模块定义的规范化产出，提前执行，推崇依赖前置
- CMD：seajs 在推广过程中对模块定义的规范化产出，延迟执行，推崇依赖就近
- CommonJs：模块输出的是一个值的 copy，运行时加载，加载的是一个对象（module.exports 属性），该对象只有在脚本运行完才会生成
- ES6 Module：ES6的模块规范


```
import a from ''
import {name,b} from ''

export default  ,但是在一个文件里export default只能有一个
```


> 区别

ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西