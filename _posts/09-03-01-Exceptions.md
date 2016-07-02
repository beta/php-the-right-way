---
title:   异常
isChild: true
anchor:  exceptions
---

## 异常 {#exceptions_title}

异常是很多流行的编程语言中一个标准的部分，但是 PHP 开发者们通常都会忽略它。类似 Ruby 的语言极度依赖异常，因此只要出了什么问题，比如 HTTP 请求失败了，或者数据库查询出错了，更或者找不到一个图片资源，Ruby（或者所使用的 gem）都会向屏幕抛出一个异常，来立刻让你知道发生了错误。

PHP 对异常的处理可谓是相当马虎，调用 `file_get_contents()` 通常只会给你一个 `false` 以及一个警告。许多早期的 PHP 框架，例如 CodeIgniter，只会返回一个 `false`，向它专有的日志记录一条信息，然后可能允许你使用类似 `$this->upload->get_error()` 的方法来查看究竟发生了什么错误。这样做有个问题，就是你必须去亲自查找一个错误并且翻看文档来了解某个类查询错误的方法是什么，而不是让它来得更加明显一些。

如果类自动向屏幕抛出异常并且停止执行，那么就会带来另外一个问题：如果你这样做，那么另一个开发者就无法动态地处理这个错误。抛出异常的目的是让开发者知晓一个错误的发生，他们就可以选择是否来处理这个错误。例如：

{% highlight php %}
<?php
$email = new Fuel\Email;
$email->subject('我的主题');
$email->body('你究竟怎么样？');
$email->to('guy@example.com', '某人');

try
{
    $email->send();
}
catch(Fuel\Email\ValidationFailedException $e)
{
    // 验证失败
}
catch(Fuel\Email\SendingFailedException $e)
{
    // 驱动无法发送邮件
}
finally
{
    // 不论是否抛出异常都要执行的代码，在正常的执行继续之前执行
}
{% endhighlight %}

### SPL 异常

通用的 `Exception` 类为开发者的调试提供了非常有限的上下文。不过要改善这一点的话，可以通过建立 `Exception` 类的子类来创建一个专门的异常类型。

{% highlight php %}
<?php
class ValidationException extends Exception {}
{% endhighlight %}

你可以用多个 `catch` 代码块来分别处理不同种类的异常。这样做会导致创建**许多**自定义的异常，其中某些异常是可以用 [SPL 扩展][splext]中提供的 SPL 异常来替代的。

比如说，如果你使用魔术方法 `__cal()` 请求了一个无效的方法，那么你可以直接 `throw new BadMethodCallException;`，而不是抛出一个标准的、模糊的 `Exception`，也不用为之建立一个自定义的异常类。

* [了解异常][exceptions]
* [了解 SPL 异常][splexe]
* [PHP 中的嵌套异常][nesting-exceptions-in-php]
* [PHP 5.3 使用异常的最佳实践][exception-best-practices53]


[splext]: #standard_php_library
[exceptions]: http://php.net/language.exceptions
[splexe]: http://php.net/spl.exceptions
[nesting-exceptions-in-php]: http://www.brandonsavage.net/exceptional-php-nesting-exceptions-in-php/
[exception-best-practices53]: http://ralphschindler.com/2010/09/15/exception-best-practices-in-php-5-3
