---
title: Composer 和 Packagist
isChild: true
anchor:  composer_and_packagist
---

## Composer 和 Packagist {#composer_and_packagist_title}

Composer 是一个**卓越的** PHP 依赖管理工具。在一个名叫 `composer.json` 的文件里列出项目的依赖，再用上几个简单的命令，Composer 就可以自动下载你的项目所需的依赖并且配置好自动加载。Composer 可以与 node.js 世界的 NPM 或者 Ruby 世界的 Bundler 进行类比。

已经有非常多的 PHP 库兼容 Composer，做好准备在你的项目中了。这些「包」都列在了 [Packagist] 里，一个用于兼容 Composer 的 PHP 库的官方仓库。

### 如何安装 Composer

你可以在局部（在你当前工作的目录中）安装 Composer，也可以在全局范围内安装（例如 /usr/local/bin，推荐安装到这个目录）。假设你想全局安装 Composer：

{% highlight console %}
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
{% endhighlight %}

**注意：**如果上面的命令因为权限问题失败，在 `mv` 那行前面加上 `sudo` 重新运行。

这样将会下载 `composer.phar`（一个 PHP 二进制归档文件），你可以用 `php` 运行这个文件来管理你的项目。

**请注意：**如果你把下载的代码直接传输到解释器中，请先在网上阅读这些代码确保它是安全的。

#### 在 Windows 上安装

对 Windows 用户来说，安装和运行 Composer 最简单的方式是使用 `ComposerSetup` 安装工具，它会进行全局安装并且设置好 `$PATH`，从而让你可以用命令行在任何目录内执行 `composer`。

### 如何（手动）安装 Composer

手动安装 Composer 是一个技术活。不过，对一个开发者来说有很多种理由来选择手动安装而不是使用交互式安装工具。交互式安装会检查你安装的 PHP，从而确保：

- PHP 的版本足以使用
- 可以正确地运行 `.phar` 文件
- 拥有某些目录的权限
- 没有启用某些有问题的扩展
- 已经配置了某些 `php.ini` 选项

既然手动安装不会自动执行这些检查，你需要确定这样的取舍对你来说是否值得。下面是手动安装 Composer 的方法：

{% highlight console %}
curl -s https://getcomposer.org/composer.phar -o $HOME/local/bin/composer
chmod +x $HOME/local/bin/composer
{% endhighlight %}

路径 `$HOME/local/bin`（或者你所选择的目录）应当加入到你的 `$PATH` 环境变量中，使你可以执行 `composer` 命令。

当你看到文档里说使用 `php composer.phar install` 来运行 Composer，你可以替换成：

{% highlight console %}
composer install
{% endhighlight %}

这一节将会假设你已经全局安装了 Composer。

### 如何定义和安装依赖

Composer 在一个叫做 `composer.json` 的文件中记录项目的依赖。如果你喜欢的话，你可以手动管理这个文件，或者也可以使用 Composer 来进行管理。`composer require` 命令会添加一条项目依赖项，如果你没有 `composer.json` 这个文件的话，它会自动建立一个。下面是一个把 [Twig] 添加为项目的依赖项的例子：

{% highlight console %}
composer require twig/twig:~1.8
{% endhighlight %}

也可以用 `composer init` 命令来引导你为项目创建一个完整的 `composer.json` 文件。不论使用哪种方法，只要你已经建立了 `composer.json` 这个文件，你就可以让 Composer 下载项目的依赖项并且安装到 `vendor/` 目录。这同样也适用于你下载的、已经有一个 `composer.json` 文件的项目：

{% highlight console %}
composer install
{% endhighlight %}

下一步，将下面这一行添加到你的项目的主要 PHP 文件中。这行代码会告诉 PHP 使用 Composer 的自动加载器来自动加载项目的依赖项。

{% highlight php %}
<?php
require 'vendor/autoload.php';
{% endhighlight %}

现在你就可以使用项目的依赖了，它们也会被按需自动加载。

### 更新你的依赖

Composer 会建立一个名叫 `composer.lock` 的文件，在第一次执行 `composer.install` 时会存储所下载的每一个包的具体版本。如果你与其他的开发者共享你的项目，而且 `composer.lock` 文件随项目一同分发，当他们执行 `composer install` 时他们也会得到与你相同的版本。要更新你的依赖项，执行 `composer update`。

当你要灵活控制对版本的要求时，这会很有帮助。比方说，`~1.8` 这样的版本要求意味着「高于 `1.8.0` 但是低于 `2.0.x-dev` 的任何版本」。你也可以用通配符 `*`，例如 `1.8.*`。Composer 的 `composer update` 命令会将所有依赖项更新到满足你的规定的最新版本。

### 更新提醒

要想收到有关新版本发布的提醒，你可以在 [VersionEye] 上面注册，那是一个检测你的 GitHub 和 BitBucket 账户中的 `composer.json` 文件并且当你使用的包有更新时自动向你发送邮件的 web 服务。

### 检查你的依赖中的安全问题

[Security Advisories Checker] 提供了一个 web 服务和一个命令行工具，两者都可以检查你的 `composer.lock` 文件，然后告诉你是否需要更新某些依赖项。

### 用 Composer 处理全局依赖

Composer 还可以用来处理全局的依赖项和二进制文件。用法非常简单，你只需要在命令前加 `global`，例如如果你想安装 PHPUnit 并且让它在全局范围内都可用，你可以执行以下的命令：

{% highlight console %}
composer global require phpunit/phpunit
{% endhighlight %}

这样会建立一个 `~/.composer` 文件夹，用来存放你的全局依赖项。要让已经安装的包的二进制文件随处可用，你需要将 `~/.composer/vendor/bin` 文件夹添加到你的 `$PATH` 变量中。

* [了解 Composer]

[Packagist]: http://packagist.org/
[Twig]: http://twig.sensiolabs.org
[VersionEye]: https://www.versioneye.com/
[Security Advisories Checker]: https://security.sensiolabs.org/
[Learn about Composer]: http://getcomposer.org/doc/00-intro.md
[ComposerSetup]: https://getcomposer.org/Composer-Setup.exe
