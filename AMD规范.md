异步模块定义（AMD）的API定义了一种机制，以便于模块和模块的依赖可以被异步加载。异步加载非常适合浏览器环境，与同步加载相比，同步加载有性能、可用性、调试和跨域访问问题。

## AMD规范
### `define()`
这个规范定义了只定义了一个函数：`define`，他是一个全局变量，它的语法如下
```js
define(id?: String, dependencies?: String[], factory: Function|Object);
```

#### id
id是一个字符串字面量，指定了被定义的模块的id，是可配置的。如果没有设置该参数，模块的id默认为模块加载器请求的指定脚本的名字。如果提供了该参数，模块的id必须是“top-level”或者“absolute id”

id的格式：模块的id是用来标识被定义的模块的，也可以在依赖数组中使用。AMD模块id的规范是[CommonJS模块名规范](http://wiki.commonjs.org/wiki/Modules/1.1.1#Module_Identifiers)的超集。引用如下：
* 模块标识由“items”组成，这些items由正斜杠分隔
* 一个item必须为：驼峰形式、“.”或“..”
* 模块标识可以没有文件后缀名：“.js”
* 模块标识可以是“relative”或者“top-level”。如果模块标识的第一个item是“.”或“..”，那么这个模块标识是“relative”，如：`../index`
* “top-level”标识会相对模块系统的基础路径来解析
* “Relative”标识相对于通过“require”写入和调用的模块的标识符进行解析

上面引用的CommonJS模块id属性通常用于JavaScript模块。

Relative模块id解析示例
* if module `"a/b/c"` asks for `"../d"`, that resolves to `"a/d"`
* if module `"a/b/c"` asks for `"./e"`, that resolves to `"a/b/e"`

如果AMD的实现支持[Loader Plugins](https://github.com/amdjs/amdjs-api/blob/master/LoaderPlugins.md)，那么“!”被用于分隔加载器插件模块名和插件资源名。由于插件资源名可以非常自由地命名，大多数字符都允许在插件资源名使用。

#### dependencies
