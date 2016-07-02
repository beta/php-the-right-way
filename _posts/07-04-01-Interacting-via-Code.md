---
isChild: true
title:   操作数据库
anchor:  databases_interacting
---

## 操作数据库 {#databases_interacting_title}

当开发者初次学习 PHP 时，他们经常会将数据库操作与表示层的逻辑混在一起，写出就像这样自的代码：

{% highlight php %}
<ul>
<?php
foreach ($db->query('SELECT * FROM table') as $row) {
    echo "<li>".$row['field1']." - ".$row['field1']."</li>";
}
?>
</ul>
{% endhighlight %}

这是非常糟糕的代码，糟糕提现在很多地方，比如说很难调试、很难测试、很难阅读，而且如果你不设置一个上限的话，它将会输出大量的字段。

根据你更喜欢 [OOP](#object-oriented-programming) 还是[函数式编程](#functional-programming)，写出来的代码可能有所不同，不过都必须在数据库操作和表示层逻辑之间进行一定的分离。

考虑最基本的步骤：

{% highlight php %}
<?php
function getAllFoos($db) {
    return $db->query('SELECT * FROM table');
}

foreach (getAllFoos($db) as $row) {
    echo "<li>".$row['field1']." - ".$row['field1']."</li>"; // 糟糕的！
}
{% endhighlight %}

这是一个不错的开始，将这两部分放到两个不同的文件里，你就实现了比较干净的分离。

建立一个类并将那个方法放进去，你就得到了一个「模型」；把表示层逻辑放到一个简单的 `.php` 文件里，你就得到了一个「视图」。这样做就与 [MVC]——一个被大多数[框架](#frameworks)所采用的 OOP 架构——非常接近了。

**foo.php**

{% highlight php %}
<?php
$db = new PDO('mysql:host=localhost;dbname=testdb;charset=utf8', 'username', 'password');

// 引入模型
include 'models/FooModel.php';

// 建立一个实例
$fooModel = new FooModel($db);
// 获得 Foos
$fooList = $fooModel->getAllFoos();

// 展示视图
include 'views/foo-list.php';
{% endhighlight %}


**models/FooModel.php**

{% highlight php %}
<?php
class FooModel
{
    protected $db;

    public function __construct(PDO $db)
    {
        $this->db = $db;
    }

    public function getAllFoos() {
        return $this->db->query('SELECT * FROM table');
    }
}
{% endhighlight %}

**views/foo-list.php**

{% highlight php %}
<?php foreach ($fooList as $row): ?>
    <?= $row['field1'] ?> - <?= $row['field1'] ?>
<?php endforeach ?>
{% endhighlight %}

这与大多数现代框架所做的工作完全一致，尽管得亲手多做一点事情。你不一定每次都需要这样做，但是如果你想对程序进行 [单元测试](#unit-testing)的话，把大量的表示层逻辑和数据库操作混合在一起却会带来大问题。

[PHPBridge] 有一篇不错的文章，叫做[建立一个数据类]，其中包含了非常相似的内容，对于刚开始学习操作数据库的开发者来说是很不错的参考。


[MVC]: http://code.tutsplus.com/tutorials/mvc-for-noobs--net-10488
[PHPBridge]: http://phpbridge.org/
[建立一个数据类]: http://phpbridge.org/intro-to-php/creating_a_data_class
