---
title:   编译模板
isChild: true
anchor:  compiled_templates
---

## 编译模板 {#compiled_templates_title}

PHP 已经发展成一个成熟的面向对象语言，但是作为一个模板语言，它却[没有很大的进步][article_templating_engines]。编译模板，例如 [Twig]、[Brainy] 和 [Smarty]*，通过提供一套专门用于模板的全新语法填补了这方面的空缺。从自动转义，到继承和简化的控制结构，编译模板非常容易编写和阅读，并且使用起来也很安全。编译模板可以在不同的语言中使用，例如 [Mustache] 就是一个很好的例子。由于这些模板必须被编译，它们在性能上会有一点点问题，不过如果使用了恰当的缓存机制的话就没有什么大碍。

* Smarty 虽然提供了自动转义的功能，这个功能模式是**没有**启用的。

### 编译模板的简单例子

使用 [Twig] 库。

{% highlight html+jinja %}
{% raw %}
{% include 'header.html' with {'title': '用户资料'} %}

<h1>用户资料</h1>
<p>你好，{{ name }}</p>

{% include 'footer.html' %}
{% endraw %}
{% endhighlight %}

### 使用继承的编译模板的例子

使用 [Twig] 库。

{% highlight html+jinja %}
{% raw %}
// template.html

<html>
<head>
    <title>{% block title %}{% endblock %}</title>
</head>
<body>

<main>
    {% block content %}{% endblock %}
</main>

</body>
</html>
{% endraw %}
{% endhighlight %}

{% highlight html+jinja %}
{% raw %}
// user_profile.html

{% extends "template.html" %}

{% block title %}用户资料{% endblock %}
{% block content %}
    <h1>用户资料</h1>
    <p>你好，{{ name }}</p>
{% endblock %}
{% endraw %}
{% endhighlight %}


[article_templating_engines]: http://fabien.potencier.org/article/34/templating-engines-in-php
[Twig]: http://twig.sensiolabs.org/
[Brainy]: https://github.com/box/brainy
[Smarty]: http://www.smarty.net/
[Mustache]: http://mustache.github.io/
