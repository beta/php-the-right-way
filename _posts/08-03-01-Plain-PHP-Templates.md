---
title:   纯 PHP 模板
isChild: true
anchor:  plain_php_templates
---

## 纯 PHP 模板 {#plain_php_templates_title}

纯 PHP 模板是用原生 PHP 代码编写的非常简单的模板。因为 PHP 本身就是个模板语言，所以这是一个非常自然的选择，你可以把 PHP 代码和其他的代码（比如 HTML）结合在一起。对 PHP 开发者来说这样做很有好处，因为他们不需要去学习新的语法，而且对各种可用的函数也都很熟悉，手头的代码编辑器也已经都内置了 PHP 语法高亮和自动完成。更重要的是，纯 PHP 模板因为不需要额外的编译阶段，所以用起来也会非常快。

每个现代的 PHP 框架都使用了某种类型的模板系统，大多数都是纯 PHP 模板。除了框架之外，类似于 [Plates][plates] 和 [Aura.View][aura] 的一些库也通过提供了一些现代化的模板功能，例如继承、布局和扩展，来使用纯 PHP 模板。

### 纯 PHP 模板的简单例子

使用 [Plates][plates] 库。

{% highlight php %}
<?php // user_profile.php ?>

<?php $this->insert('header', ['title' => '用户资料']) ?>

<h1>用户资料</h1>
<p>你好，<?=$this->escape($name)?></p>

<?php $this->insert('footer') ?>
{% endhighlight %}

### 使用继承的纯 PHP 模板的例子

使用 [Plates][plates] 库。

{% highlight php %}
<?php // template.php ?>

<html>
<head>
    <title><?=$title?></title>
</head>
<body>

<main>
    <?=$this->section('content')?>
</main>

</body>
</html>
{% endhighlight %}

{% highlight php %}
<?php // user_profile.php ?>

<?php $this->layout('template', ['title' => '用户资料']) ?>

<h1>用户资料</h1>
<p>你好，<?=$this->escape($name)?></p>
{% endhighlight %}


[plates]: http://platesphp.com/
[aura]: https://github.com/auraphp/Aura.View
