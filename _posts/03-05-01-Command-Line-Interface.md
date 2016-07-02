---
title: 命令行接口
isChild: true
anchor:  command_line_interface
---

## 命令行接口 {#command_line_interface_title}

PHP 创建之初是用来编写 web 应用程序的，但是用它来编写命令行接口（command line interface，CLI）的程序也同样实用。命令行的 PHP 程序可以用来自动化常见的任务，例如测试、部署和程序管理等等。

命令行 PHP 程序非常强大，你可以直接使用你的应用的代码，而无需创建和维护一个 web 图形界面。只要确保别把你的命令行 PHP 脚本放到你的公共 web 目录就行！

试着在你的命令行里运行 PHP：

{% highlight console %}
> php -i
{% endhighlight %}

`-i` 选项会像 [`phpinfo()`][phpinfo] 函数那样输出你的 PHP 配置信息。

`-a` 选项可以启动一个交互式 shell，就像 Ruby 的 IRB 以及 Python 的交互式 shell 那样。除此之外还有其他很多有用的 [命令行选项][cli-options]。

我们来写一个简单的「Hello, $name」命令行程序吧。建立一个名叫 `hello.php` 的文件，内容如下：

{% highlight php %}
<?php
if ($argc !== 2) {
    echo "Usage: php hello.php [name].\n";
    exit(1);
}
$name = $argv[1];
echo "Hello, $name\n";
{% endhighlight %}

PHP 会基于你运行脚本时提供的参数建立两个特殊的变量。[`$argc`][argc] 是一个整数，表示参数的**个数**，而 [`$argv`][argv] 则是一个数组，其中包含每一个参数的**值**。第一个参数永远都是你的 PHP 脚本文件的名字，在这里就是 `hello.php`。

`exit()` 与一个非 0 数字搭配使用，可以告诉 shell 这个命令执行失败了。常用的退出代码可以在[这里][exit-codes]找到。

要在运行上面的脚本，在命令行中输入：

{% highlight console %}
> php hello.php
Usage: php hello.php [name]
> php hello.php world
Hello, world
{% endhighlight %}


 * [了解在命令行中运行 PHP][php-cli]
 * [了解如何配置 Windows 从而在命令行中运行 PHP][php-cli-windows]


[phpinfo]: http://php.net/function.phpinfo
[cli-options]: http://php.net/features.commandline.options
[argc]: http://php.net/reserved.variables.argc
[argv]: http://php.net/reserved.variables.argv
[exit-codes]: http://www.gsp.com/cgi-bin/man.cgi?section=3&amp;topic=sysexits
[php-cli]: http://php.net/features.commandline
[php-cli-windows]: http://php.net/install.windows.commandline
