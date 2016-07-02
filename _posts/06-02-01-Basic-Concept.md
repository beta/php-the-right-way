---
title: 基本概念
isChild: true
anchor:  basic_concept
---

## 基本概念 {#basic_concept_title}

我们可以通过一个简单（甚至可以说很幼稚）的例子来展示这一概念。

在这里我们有一个 `Database` 类，它需要用一个适配器来与数据库通信。我们在构造方法中实例化这个适配器，并且创建了一个硬编码的依赖。这样会让测试变得非常困难，而且 `Database` 类会与适配器紧密耦合在一起。

{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct()
    {
        $this->adapter = new MySqlAdapter;
    }
}

class MysqlAdapter {}
{% endhighlight %}

可以使用依赖注入来重构这段代码，从而减轻依赖。

{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(MySqlAdapter $adapter)
    {
        $this->adapter = $adapter;
    }
}

class MysqlAdapter {}
{% endhighlight %}

现在是由我们向 `Database` 类提供它的依赖，而不是由它自己来创建。我们甚至还可以建立一个方法，接受这个依赖的参数并且进行设置。如果 `$adapter` 属性是 `public` 的话，我们还可以直接进行设置。
