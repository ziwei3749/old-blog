# JS核心基础

- 1.JavaScript 的 typeof 返回哪些数据类型？

```
返回基础数据类型 : number string boolean Symbol undefined

返回复杂数据类型 : function object
```

- 2.请写出以下运算结果：

```
alert(typeof null);
alert(typeof undefined);
alert(typeof NaN);
alert(NaN == undefined);
alert(NaN == NaN);
var str = "123abc";
alert(typeof str++);
alert(str);
```

注意str++,会有一个先Number(str)的操作,所以结果是NaN,typeof是number

