本规范说明了应当如何编写在浏览器环境中执行的模块。也就是说，本规范定义了模块系统能够在浏览器环境执行所需要的最小特征，如下：
* 模块是单例的
* 模块中的自由变量不应该被暴露出去
* 模块是按需执行的

## 模块的定义
通过关键字`define`定义一个模块，`define`是一个全局函数
```js
define(factory)
```
1. `define`函数接收一个参数：`factory`
2. `factory`可以是一个函数或其他有效值
3. 如果`factory`是函数，那么它的前三个参数按次序分别是`require`、`exports`、`module`
4. 如果`factory`不是函数，那么module.exports的值就是factory

## 模块的上下文
在一个模块中，有三个自由变量：`require`、`exports`、`module`
```
define(function(require, exports, module) {

  // The module code goes here

})
```

### require
1. `require`是一个函数
```
api = require(identifier)
```
  * `require`接收一个模块的标识
  * `require`返回的是被引入的模块暴露的API
  * 如果被引入的模块没有返回值，`require`的值为`null`
2. `require.async`是一个函数
```
api = require.async(identifier, callback?)
```
  * `require.async`接收一个模块标识数组和一个可配置的回调函数
  * 回调函数将引入的模块的导出作为其参数，参数的顺序和模块标识数组中模块的顺序一致
  * 如果被引入的模块没有返回值，回调函数对应位置的参数为`null`

### exports
在一个模块中，`exports`是一个对象，模块可以将要暴露给外部的变量或方法添加到这个对象上。

### module
1. `module.uri`：The full resolved uri to the module
2. `module.dependencies`：模块所需的模块标识符列表
3. `module.exports`：模块的导出API，功能和`exports`类似

## 模块标识
1. 模块标识必须是一个string
2. 模块标识可以没有文件后缀`.js`
3. 模块标识应该用连字符标识，如：`foo-bar`
4. 模块标识可以是相对路径，如：`./foo`、`../bar`

## 示例代码
### 一个典型的例子
math.js
```
define(function(require, exports, module) {
  exports.add = function() {
    var sum = 0, i = 0, args = arguments, l = args.length;
    while (i < l) {
      sum += args[i++];
    }
    return sum;
  };
});
```
increment.js
```
define(function(require, exports, module) {
  var add = require('math').add;  // 引入了math.js模块
  exports.increment = function(val) {
    return add(val, 1);
  };
});
```
program.js
```
define(function(require, exports, module) {
  var inc = require('increment').increment;
  var a = 1;
  inc(a); // 2

  module.id == "program";
});
```

### 使用非函数包裹模块
object-data.js
```
define({
    foo: "bar"
});
```
array-data.js
```
define([
    'foo',
    'bar'
]);
```
string-data.js
```
define('foo bar');
```
