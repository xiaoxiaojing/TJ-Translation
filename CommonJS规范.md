[原文链接](http://wiki.commonjs.org/wiki/Modules/1.1.1#Module_Identifiers)

本规范说明了如何编写模块，为了使得模块系统可互相操作，这些模块系统可以是客户端也可以是服务端，安全或不安全，已经实现或者通过扩展语法被未来系统支持。
这些模块有自己的作用域，可以导入其他模块的单例对象，可以导出自己的API。
也就是说，本规范定义了模块系统为支持可互操作模块而必须提供的最小功能。

## Require
require是一个函数
```
require(identifier)
```
1. require函数接收一个模块标识
2. require函数的返回值是外部函数导出的API
3. 如果有循环依赖，外部模块当其被需要时可能还没有执行完成。这种情况下，`require()`返回的对象至少要包含使得当前模块执行的exports，这些exports由外部模块提前准备。
4. 如果所请求的模块不能返回，`require`将抛出一个错误
5. require的`main`属性
	* 这个属性是只读的，不能被删除
  * main的值可以是`undefined`，或者与当前被加载模块的`module`对象相同
6. require的`paths`属性，它是一个有优先级的路径数组
  * 在`sandbox`（一个安全的模块系统）中不能有paths属性
  * 所有模块中的paths属性必须在引用上相同
  * 用另外一个对象替代paths对象可能没有效果
  * 如果paths存在, in-place modification of the contents of "paths" 必须通过相应模块的搜索行为来反应
  * 如果paths存在，则它可能不是搜索路径的详尽列表，因为加载程序可能会根据已有的paths查找其他位置
  * 如果paths存在，it is the loader's prorogative to resolve, normalize, or canonicalize the paths provided.
 
 
## Module context
