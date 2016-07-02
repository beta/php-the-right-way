---
title:   使用 UTF-8
isChild: true
anchor:  php_and_utf8
---

## 使用 UTF-8 {#php_and_utf8_title}

_这一节最初由 [Alex Cabal](https://alexcabal.com/) 在 [PHP 最佳实践](https://phpbestpractices.org/#utf-8)中编写，并且被用作我们针对 UTF-8 的建议的基础。_

### 这不是什么简单事。小心对待，仔细处理，始终如一。

现阶段 PHP 并没有为 Unicode 提供低层的支持。有很多方法可以确保 UTF-8 字符串的正确处理，但这并不简单，而且贯穿 web 应用的每一层，从 HTML 到 SQL 再到 PHP。我们会做一个简明实用的总结。

### PHP 层面的 UTF-8

基础的字符串操作，例如拼接两个字符串以及将字符串赋值给变量，对于 UTF-8 而言都不需要额外的改动。不过除此之外大多数的字符串函数，例如 `strpos()` 和 `strlen()`，都需要特殊对待。这些方法通常都有一个对应的 `mb_*` 版本，例如 `mb_strpos()` 和 `mb_strlen()`。这些 `mb_*` 函数是通过[多字节字符串扩展]来实现的，并且都是为了处理 Unicode 字符串而特别设计的。

当你操作一个 Unicode 字符串时，你必须使用这些 `mb_*` 方法。例如，如果你在一个 UTF-8 字符串上使用 `substr()`，结果中有很大的可能性会出现一些错乱的半字符。正确的方法是使用多字节的版本，也就是 `mb_substr()`。

困难的是每次都要记住去使用这些 `mb_*` 函数。如果你仅仅忘记一次，你的 Unicode 字符串就可能会在后续的处理中变得乱七八糟。

并不是所有的字符串函数都有一个 `mb_*` 的版本。如果你需要的函数没有多字节的版本，那么你可能就不走运了。

在你所写的每一个 PHP 脚本的最顶端（或者在你的全局导入脚本的开头），你都应该使用 `mb_internal_encoding()` 函数。如果你的脚本将要输出到浏览器，那么你还需要紧随其后调用 `mb_http_output()` 函数。在每个脚本中显式地定义字符串的编码将会为你解决大量的麻烦事。

除此之外，许多操作字符串的 PHP 函数都有一个用来指定字符编码的可选参数，只要有这个选项，你需要每次都显式地指定 UTF-8。例如，`htmlentities()` 函数有一个用于字符编码的选项，只要你处理的是 UTF-8 编码的字符串，你就需要指定这个参数。注意，从 PHP 5.4.0 开始，`htmlentities()` 和 `htmlspecialchars()` 的默认编码都是 `UTF-8`。

最后，如果你构建的是一个分布式的程序，无法确定是否启用了 `mbstring` 扩展，那么你可以考虑使用 [patchwork/utf8] 这个 Composer 包，当 `mbstring` 可用时它会使用这个扩展，否则就会回退到非 UTF-8 的函数。

[多字节字符串扩展]: http://php.net/book.mbstring
[patchwork/utf8]: https://packagist.org/packages/patchwork/utf8

### 数据库层面的 UTF-8

如果你的 PHP 脚本要访问 MySQL 数据库，即使你做好了上述的预防措施，你的字符串也有可能会以非 UTF-8 字符串的形式存储在数据库中。

要保证你的字符串从 PHP 到 MySQL 始终都是 UTF-8 编码，确保你的数据库和表都使用了 `utf8mb4` 字符集和整理方式，并且在 PDO 连接字符串里也使用 `utf8mb4` 字符集。参考下面的例子。这一点**非常重要**。

注意，要得到完整的 UTF-8 支持，你必须使用 `utf8mb4` 字符集，而不是 `utf8`！原因见延伸阅读。

### 浏览器层面的 UTF-8

使用 `mb_http_output()` 函数来确保你的 PHP 脚本向浏览器输出 UTF-8 编码的结果。

你还需要使用 HTTP 响应告诉浏览器，这个页面需要按照 UTF-8 来处理。很久之前的解决方法是在页面的 `<head>` 标签里加上[字符集 `<meta>` 标签](http://htmlpurifier.org/docs/enduser-utf8.html)，这个方法绝对有效，但是在 `Content-Type` 头中指定字符串是[速度快得多](https://developers.google.com/speed/docs/best-practices/rendering#SpecifyCharsetEarly)的做法。

{% highlight php %}
<?php
// 告诉 PHP 直到脚本结束我们都会使用 UTF-8 字符串
mb_internal_encoding('UTF-8');

// 告诉 PHP 我们要向浏览器输出 UTF-8 内容
mb_http_output('UTF-8');

// 用于测试的 UTF-8 字符串
$string = 'Êl síla erin lû e-govaned vîn.';

// 使用多字节函数来用某种方式变换这个字符串
// 注意，为了演示我们在一个非 ASCII 字符的位置分割字符串
$string = mb_substr($string, 0, 15);

// 连接到一个数据库从而存储变换后的字符串
// 参见这篇文档中的 PDO 例子来获取更多信息
// 注意数据源名（Data Source Name，DSN）中的 charset=utf8mb4
$link = new PDO(
    'mysql:host=your-hostname;dbname=your-db;charset=utf8mb4',
    'your-username',
    'your-password',
    array(
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_PERSISTENT => false
    )
);

// 将变换后的字符串以 UTF-8 编码存储到数据库中
// 你的数据库和表用的都是 utf8mb4 字符集和整理方式，对吧？
$handle = $link->prepare('insert into ElvishSentences (Id, Body) values (?, ?)');
$handle->bindValue(1, 1, PDO::PARAM_INT);
$handle->bindValue(2, $string);
$handle->execute();

// 获取我们刚刚存储的字符串从而证明它被正确地存储了
$handle = $link->prepare('select * from ElvishSentences where Id = ?');
$handle->bindValue(1, 1, PDO::PARAM_INT);
$handle->execute();

// 把结果存入一个对象，稍后会输出到 HTML 中
$result = $handle->fetchAll(\PDO::FETCH_OBJ);

header('Content-Type: text/html; charset=UTF-8');
?><!doctype html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>UTF-8 test page</title>
    </head>
    <body>
        <?php
        foreach($result as $row){
            print($row->Body);  // 这应该会正确地输出变换后的 UTF-8 字符串
        }
        ?>
    </body>
</html>
{% endhighlight %}

### 延伸阅读

* [PHP 手册：字符串运算符](http://php.net/language.operators.string)
* [PHP 手册：字符串函数](http://php.net/ref.strings)
    * [`strpos()`](http://php.net/function.strpos)
    * [`strlen()`](http://php.net/function.strlen)
    * [`substr()`](http://php.net/function.substr)
* [PHP 手册：多字节字符串函数](http://php.net/ref.mbstring)
    * [`mb_strpos()`](http://php.net/function.mb-strpos)
    * [`mb_strlen()`](http://php.net/function.mb-strlen)
    * [`mb_substr()`](http://php.net/function.mb-substr)
    * [`mb_internal_encoding()`](http://php.net/function.mb-internal-encoding)
    * [`mb_http_output()`](http://php.net/function.mb-http-output)
    * [`htmlentities()`](http://php.net/function.htmlentities)
    * [`htmlspecialchars()`](http://php.net/function.htmlspecialchars)
* [PHP UTF-8 备忘条](http://blog.loftdigital.com/blog/php-utf-8-cheatsheet)
* [使用 PHP 处理 UTF-8](http://www.phpwact.org/php/i18n/utf-8)
* [Stack Overflow：是什么让 PHP 不兼容 Unicode？](http://stackoverflow.com/questions/571694/what-factors-make-php-unicode-incompatible)
* [Stack Overflow：PHP 和 MySQL 处理国际化字符串的最佳实践](http://stackoverflow.com/questions/140728/best-practices-in-php-and-mysql-with-international-strings)
* [如何在 MySQL 中支持完整的 Unicode](http://mathiasbynens.be/notes/mysql-utf8mb4)
* [使用便于移植的 UTF-8 将 Unicode 引入 PHP](http://www.sitepoint.com/bringing-unicode-to-php-with-portable-utf8/)
* [Stack Overflow: DOMDocument loadHTML 不能正确编码 UTF-8](http://stackoverflow.com/questions/8218230/php-domdocument-loadhtml-not-encoding-utf-8-correctly)
