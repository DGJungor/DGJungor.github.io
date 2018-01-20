---
title: PHP注释标准
date: 2017-12-28 14:23:17
tags: 代码规范
---

# PHP中注释的规范与标准 #

```
/**
* @name 名字
* @abstract 申明变量/类/方法
* @access 指明这个变量、类、函数/方法的存取权限
* @author 函数作者的名字和邮箱地址
* @category 组织packages
* @copyright 指明版权信息
* @const 指明常量
* @deprecate 指明不推荐或者是废弃的信息
* @example 示例
* @exclude 指明当前的注释将不进行分析，不出现在文挡中
* @final 指明这是一个最终的类、方法、属性，禁止派生、修改。
* @global 指明在此函数中引用的全局变量
* @include 指明包含的文件的信息
* @link 定义在线连接
* @module 定义归属的模块信息
* @modulegroup 定义归属的模块组
* @package 定义归属的包的信息
* @param 定义函数或者方法的参数信息
* @return 定义函数或者方法的返回信息
* @see 定义需要参考的函数、变量，并加入相应的超级连接。
* @since 指明该api函数或者方法是从哪个版本开始引入的
* @static 指明变量、类、函数是静态的。
* @throws 指明此函数可能抛出的错误异常,极其发生的情况
* @todo 指明应该改进或没有实现的地方
* @var 定义说明变量/属性。
* @version 定义版本信息
*/

```





文档标记的使用范围是指该标记可以用来修饰的关键字，或其他文档标记。
所有的文档标记都是在每一行的 * 后面以@开头。如果在一段话的中间出来@的标记，这个标记将会被当做普通内容而被忽略掉。

1. @access
使用范围：class,function,var,define,module
该标记用于指明关键字的存取权限：private、public或proteced

2. @author
指明作者

3. @copyright
使用范围：class，function，var，define，module，use
指明版权信息

4. @deprecated
使用范围：class，function，var，define，module，constent，global，include
指明不用或者废弃的关键字

5. @example
该标记用于解析一段文件内容，并将他们高亮显示。Phpdoc会试图从该标记给的文件路径中读取文件内容

6. @const
使用范围：define
用来指明php中define的常量

7. @final
使用范围：class,function,var
指明关键字是一个最终的类、方法、属性，禁止派生、修改。

8. @filesource
和example类似，只不过该标记将直接读取当前解析的php文件的内容并显示。

9. @global
指明在此函数中引用的全局变量

10. @ingore
用于在文档中忽略指定的关键字

11. @license
相当于html标签中的,首先是URL，接着是要显示的内容
例如[url=http://blog.jungor.com/”http://www.jungor.com”]DGJungor's blog [/url]
可以写作 @license http://www.jungor.com DGJungor's blog

12. @link
类似于license
但还可以通过link指到文档中的任何一个关键字

13. @name
为关键字指定一个别名。

14. @package
使用范围：页面级别的-> define，function，include
类级别的->class，var，methods
用于逻辑上将一个或几个关键字分到一组。

15. @abstrcut
说明当前类是一个抽象类

16. @param
指明一个函数的参数

17. @return
指明一个方法或函数的返回指

18. @static
指明关建字是静态的。

19. @var
指明变量类型

20. @version
指明版本信息

21@todo
指明应该改进或没有实现的地方

22. @throws
指明此函数可能抛出的错误异常,极其发生的情况
上面提到过，普通的文档标记标记必须在每行的开头以@标记，除此之外，还有一种标记叫做inline tag,用{@}表示，具体包括以下几种：

23. {@link}
用法同@link

24. {@source}
显示一段函数或方法的内容
