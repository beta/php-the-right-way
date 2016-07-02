---
title: 编程范式
isChild: true
anchor:  programming_paradigms
---

## 编程范式 {#programming_paradigms_title}

PHP 是一个灵活的、动态的语言，支持多种编程技术。多年以来 PHP 取得了许多显著的进展，例如在 PHP 5.0 版本（2004）引入了面向对象模型，在 PHP 5.3 版本（2009）引入了匿名函数和命名空间，而在 PHP 5.4 版本（2012）又引入了性状。

### 面向对象编程 {#object-oriented-programming}

PHP 完整地支持面向对象编程的各种特性，包括类、抽象类、接口、集成、构造器、克隆、异常以及更多。

* [了解面向对象的 PHP][oop]
* [了解性状][traits]

### 函数式编程 {#functional-programming}

PHP 支持一等函数，也就是说可以把函数赋值给一个变量。不论是用户定义的还是内置的函数都可以用一个变量来引用并且动态地调用。一个函数可以作为参数传递给其他的函数（这个特性叫做**高阶函数**），函数也可以返回其他的函数。

PHP 也支持递归，也就是允许一个函数调用自己的特性，但是大部分 PHP 代码都更着重于迭代。

PHP 5.3（2009）引入了匿名函数（以及对闭包的支持）。

PHP 5.4 增加了将一个闭包绑定到一个对象的范围中的能力，同时也改善了对回调类型（Callable）的支持，使其与匿名函数在几乎任何情况下都可以相互替代。

* 继续阅读有关 [PHP 中的函数式编程](pages/Functional-Programming.html) 的内容
* [了解匿名函数][anonymous-functions]
* [了解闭包（Closure）类][closure-class]
* [闭包 RFC 的更多内容][closures-rfc]
* [了解回调类型（Callable）][callables]
* [了解如何使用 `call_user_func_array()` 动态调用函数][call-user-func-array]

### 元编程

PHP 通过一些机制，例如反射 API 和魔术方法，来为多种方式的元编程提供支持。有许多类似 `__get()`、`__set()`、`__clone()`、`__toString()` 以及 `__invoke()` 等等的魔术方法可供使用，开发者可以对类的行为一探究竟。Ruby 开发者经常会说 PHP 缺少 `method_missing`，但实际上 PHP 有 `__call()` 和 `__callStatic()`。

* [了解魔术方法][magic-methods]
* [了解反射][reflection]
* [了解重载][overloading]


[oop]: http://php.net/language.oop5
[traits]: http://php.net/language.oop5.traits
[anonymous-functions]: http://php.net/functions.anonymous
[closure-class]: http://php.net/class.closure
[closures-rfc]: https://wiki.php.net/rfc/closures
[callables]: http://php.net/language.types.callable
[call-user-func-array]: http://php.net/function.call-user-func-array
[magic-methods]: http://php.net/language.oop5.magic
[reflection]: http://php.net/intro.reflection
[overloading]: http://php.net/language.oop5.overloading

