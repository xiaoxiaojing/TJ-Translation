[原文链接](https://github.com/amdjs/amdjs-api/blob/master/AMD.md)

异步模块定义（AMD）的API定义了一种机制，以便于模块和模块的依赖可以被异步加载。异步加载非常适合浏览器环境，与同步加载相比，同步加载有性能、可用性、调试和跨域访问问题。

## AMD规范
### `define()`
这个规范定义了只定义了一个函数：`define`，他是一个全局变量，它的语法如下
```js
define(id?: String, dependencies?: String[], factory: Function|Object);
```

#### id
id是一个字符串字面量，指定了被定义的模块的id，是可配置的。如果没有设置该参数，模块的id默认为模块加载器请求的指定脚本的名字。如果提供了该参数，模块的id必须是“top-level”或者“absolute id”

id的格式：模块的id是用来标识被定义的模块的，也可以在依赖数组中使用。AMD模块id的规范是[CommonJS模块名规范](http://wiki.commonjs.org/wiki/Modules/1.1.1#Module_Identifiers)的超集。

Relative模块id解析示例
* if module `"a/b/c"` asks for `"../d"`, that resolves to `"a/d"`
* if module `"a/b/c"` asks for `"./e"`, that resolves to `"a/b/e"`

如果AMD的实现支持[Loader Plugins](https://github.com/amdjs/amdjs-api/blob/master/LoaderPlugins.md)，那么“!”被用于分隔加载器插件模块名和插件资源名。由于插件资源名可以非常自由地命名，大多数字符都允许在插件资源名使用。

#### dependencies
dependencies是第二个参数，是模块id数组。这个依赖必须在模块执行之前被解析，解析后的值会作为传输传递给`factory`函数。

如果dependencies ids是相对的，要相对于被定义的模块来进行解析。（The dependencies ids may be relative ids, and should be resolved relative to the module being defined. In other words, relative ids are resolved relative to the module's id, and not the path used to find the module's id）

This specification defines three special dependency names that have a distinct resolution. If the value of "require", "exports", or "module" appear in the dependency list, the argument should be resolved to the corresponding free variable as defined by the CommonJS modules specification.

dependencies参数是可配置的，如果省略，默认为：`["require", "exports", "module"]`. However, if the factory function's arity (length property) is less than 3, then the loader may choose to only call the factory with the number of arguments corresponding to the function's arity or length.

#### factory
factory是第三个参数，是一个函数或对象。如果factory是一个函数，它只应该被执行一次。如果factory是一个对象，这个对象作为module的导出






