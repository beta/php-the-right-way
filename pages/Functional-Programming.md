---
layout: page
title:  PHP 中的函数式编程
sitemap: true
---

# PHP 中的函数式编程

PHP 支持一等函数，也就是说可以把函数赋值给一个变量。不论是用户定义的还是内置的函数都可以用一个变量来引用并且动态地调用。一个函数可以作为参数传递给其他的函数，函数也可以返回其他的函数，这个特性叫做高阶函数。

PHP 也支持递归，也就是允许一个函数调用自己的特性，但是大部分 PHP 代码都更着重于迭代。

PHP 5.3（2009）引入了匿名函数（以及对闭包的支持）。

PHP 5.4 增加了将一个闭包绑定到一个对象的范围中的能力，同时也改善了对回调类型（Callable）的支持，使其与匿名函数在几乎任何情况下都可以相互替代。

高阶函数最常见的用例是实现策略模式。内置的 `array_filter()` 函数既要求一个输入的数组（数据），同时又要求提供一个函数（策略，或者一个回调函数）用作对每个数组元素的过滤函数。

{% highlight php %}
<?php
$input = array(1, 2, 3, 4, 5, 6);

// 建立一个匿名函数并赋值给一个变量
$filter_even = function($item) {
    return ($item % 2) == 0;
};

// 内置的 array_filter 函数接受数据和函数
$output = array_filter($input, $filter_even);

// 函数不一定要赋值给变量，这样也是可以的：
$output = array_filter($input, function($item) {
    return ($item % 2) == 0;
});

print_r($output);
{% endhighlight %}

闭包是一个不需要使用任何全局变量就可以访问从外部作用域导入的变量的匿名函数。理论上来讲，闭包是一个函数，伴随着一些在定义闭包时由环境封闭（例如固定）起来的参数。闭包可以以一种干净的方式来处理变量的作用域。

接下来的例子中，我们使用闭包定义了一个为 `array_filter()` 返回一个过滤器的函数，这个过滤器使用一个过滤器函数族群产生。

{% highlight php %}
<?php
/**
 * 建立一个匿名过滤函数，接受大于 $min 的元素
 *
 * 从一个「大于 n」的过滤函数族群中返回一个过滤器
 */
function criteria_greater_than($min)
{
    return function($item) use ($min) {
        return $item > $min;
    };
}

$input = array(1, 2, 3, 4, 5, 6);

// 使用 array_filter 以及选定的过滤函数来处理输入
$output = array_filter($input, criteria_greater_than(3));

print_r($output); // items > 3
{% endhighlight %}

族群中的每一个过滤函数只会接受大于某一个最小值的元素。`criteria_greater_than` 所返回的一个过滤器是一个具有 `$min` 参数的闭包，这个参数被封闭在作用域中（在调用 `criteria_greater_than` 时传递进来）。

向创建的函数中导入 `$min` 变量时默认使用的是早期绑定。要使用后期绑定的、真正的闭包来说，应当在导入时使用一个引用。想象有这么一个模板库或者一个输入验证库，闭包用来将变量捕获到作用域中，并且在匿名函数被调用时才回进行访问。

* [了解匿名函数][anonymous-functions]
* [闭包 RFC 的更多信息][closures-rfc]
* [了解如何使用 `call_user_func_array()` 动态调用函数][call-user-func-array]


[anonymous-functions]: http://php.net/functions.anonymous
[closures-rfc]: https://wiki.php.net/rfc/closures
[call-user-func-array]: http://php.net/function.call-user-func-array
