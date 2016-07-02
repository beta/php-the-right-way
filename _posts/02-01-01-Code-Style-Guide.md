---
title: 代码风格指南
anchor: code_style_guide
---

# 代码风格指南 {#code_style_guide_title}

PHP 社区是非常庞大而且多样化的，包括难以计数的库、框架和组件。对于 PHP 开发者来说，从其中选择几件组合到一个单独的项目中是很常见的事情。让 PHP 代码都（尽可能地）遵守一个共同的代码风格是很重要的，可以让开发者很轻松地组合和搭配各种不同的库并用于自己的项目。

[框架互操作组织（Framework Interop Group）][fig] 提出并通过了一系列风格建议。它们并不都是有关代码风格，真正与风格相关的有 [PSR-0][psr0]、[PSR-1][psr1]、[PSR-2][psr2] 和 [PSR-4][psr4]。这些建议只是一系列规则，并且正在被 Drupal、Zend、Symfony、CakePHP、phpBB、AWS SDK、FuelPHP、Lithium 等等这些项目所采用。你可以在自己的项目中应用这些建议，或者也可以继续用你自己的风格。

理想情况下，你应该按照一个已有的标准来编写 PHP 代码，这个标准可以是几个 PSR 的任意组合，或者由 PEAR 或者 Zend 提出的一些编码规范，从而让其他的开发者可以轻松的阅读和使用你的代码，并且让实现了许多组件的程序可以与大量第三方代码共存。

* [了解 PSR-0][psr0]
* [了解 PSR-1][psr1]
* [了解 PSR-2][psr2]
* [了解 PSR-4][psr4]
* [了解 PEAR 编码规范][pear-cs]
* [了解 Symfony 编码规范][symfony-cs]

你可以用 [PHP_CodeSniffer][phpcs] 来根据以上任意一个代码风格建议检查自己的代码，还可以使用诸如 [Sublime Text 2][st-cs] 的文本编辑器的一些插件来提供实时的反馈。

你可以使用以下工具之一来自动调整代码的布局。首先是 [PHP Coding Standards Fixer][phpcsfixer]，它有一个测试非常完备的代码库；然后是 [php.tools][phptools]，由 [sublime-phpfmt][sublime-phpfmt] 插件发扬光大，虽然相比前者更新一些，但是它在性能上表现优异，可以带来更加流畅的编辑器实时调整。

你还可以在 shell 中手动运行 phpcs：

    phpcs -sw --standard=PSR2 file.php

它会列出错误并且告诉你如何修复。可以把这个工具集成到 git hook 中，这样包含与选定的规范不符的代码的分支在修复之前就无法进入到仓库中。

推荐使用英文来编写所有符号名和代码架构。注释可以用任何语言书写，从而让当前以及之后将会使用这个代码库的人更加容易阅读。


[fig]: http://www.php-fig.org/
[psr0]: http://www.php-fig.org/psr/psr-0/
[psr1]: http://www.php-fig.org/psr/psr-1/
[psr2]: http://www.php-fig.org/psr/psr-2/
[psr4]: http://www.php-fig.org/psr/psr-4/
[pear-cs]: http://pear.php.net/manual/en/standards.php
[symfony-cs]: http://symfony.com/doc/current/contributing/code/standards.html
[phpcs]: http://pear.php.net/package/PHP_CodeSniffer/
[st-cs]: https://github.com/benmatselby/sublime-phpcs
[phpcsfixer]: http://cs.sensiolabs.org/
[phptools]: https://github.com/phpfmt/php.tools
[sublime-phpfmt]: https://github.com/phpfmt/sublime-phpfmt
