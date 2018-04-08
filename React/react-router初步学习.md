# react-router-dom初步学习

react-router 和 react-router-dom是有区别的。我们一般用后者

react-router-dom里有几个核心慨念: 

- BrowserRouter : 常用属性basename 
- Route  : 指明某个path对应的component。
    + path和component : 指明是哪个路径对应哪个组件
    + exact : 精确匹配
- Link
    + to : 接受一个stirng或者obj,来控制url
- Switch
    + 一般用来包括Route,不能放其他元素,作用就是指显示一个路由
- match: 是在使用router后被放入props中的一个属性,在class创建的组件中,我们需要通过
this.props.match来获取到match之中的信息。
    
