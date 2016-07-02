---
isChild: true
title:   PDO 扩展
anchor:  pdo_extension
---

## PDO 扩展 {#pdo_extension_title}

自 5.1.0 版本开始 PHP 引入了 [PDO]，这是一个数据库连接抽象库，提供了访问多种数据库的一组共同的接口。例如，你基本可以使用相同的代码来访问 MySQL 和 SQLite：

{% highlight php %}
<?php
// PDO + MySQL
$pdo = new PDO('mysql:host=example.com;dbname=database', 'user', 'password');
$statement = $pdo->query("SELECT some_field FROM some_table");
$row = $statement->fetch(PDO::FETCH_ASSOC);
echo htmlentities($row['some_field']);

// PDO + SQLite
$pdo = new PDO('sqlite:/path/db/foo.sqlite');
$statement = $pdo->query("SELECT some_field FROM some_table");
$row = $statement->fetch(PDO::FETCH_ASSOC);
echo htmlentities($row['some_field']);
{% endhighlight %}

PDO 不会翻译你的 SQL 查询或者模拟缺失的功能，它只是用于借助同一组 API 来连接多种类型的数据库。

更重要的是，PDO 可以让你将外部输入（例如 ID）安全地注入到 SQL 查询中，而不需要担心数据 SQL 注入攻击的发生。使用 PDO 语句和绑定的参数可以做到这一点。

我们假设一个 PHP 脚本会接受一个数字 ID 作为查询参数，这个 ID 将会用来从数据库获取一个用户记录。下面是一种错误的做法：

{% highlight php %}
<?php
$pdo = new PDO('sqlite:/path/db/users.db');
$pdo->query("SELECT name FROM users WHERE id = " . $_GET['id']); // <-- NO!
{% endhighlight %}

这段代码写得非常烂。你向 SQL 查询中插入了一个未经处理的参数，如果使用一种叫做 [SQL 注入]的技术，你会马上遭到攻击。想象一个攻击者通过类似 `http://domain.com/?id=1%3BDELETE+FROM+users` 这样的 URL 传入一个不一般的 `id` 参数，变量 `$_GET['id']` 的值就会变成 `1;DELETE FROM users`，从而删掉你所有的用户！正确的做法是使用 PDO 绑定参数来给输入的 ID 消消毒。

{% highlight php %}
<?php
$pdo = new PDO('sqlite:/path/db/users.db');
$stmt = $pdo->prepare('SELECT name FROM users WHERE id = :id');
$id = filter_input(INPUT_GET, 'id', FILTER_SANITIZE_NUMBER_INT); // <-- filter your data first (see [Data Filtering](#data_filtering)), especially important for INSERT, UPDATE, etc.
$stmt->bindParam(':id', $id, PDO::PARAM_INT); // <-- Automatically sanitized for SQL by PDO
$stmt->execute();
{% endhighlight %}

这才是正确的代码。它在 PDO 语句中使用了绑定参数，在将输入的 ID 传递到数据库之前，先将其进行转义，从而预防潜在的 SQL 注入攻击。

对于写入操作，例如 `INSERT` 和 `UPDATE`，必须先[过滤数据](#data_filtering)并处理掉其他的一些内容（例如删除掉 HTML 标签、JavaScript 代码等等）。PDO 只会为 SQL 而不会为你的程序清洁输入。

* [了解 PDO]

你还应该注意数据库连接会占用资源，而且如果连接没有被隐式地关闭的话，耗尽资源并不是没发生过的事情。不过，这种情况在其他的语言中会更普遍一些。使用 PDO 的话你可以通过将所有的引用删除掉（例如设置成 `NULL`）来销毁一个对象，从而关闭一个连接。如果你不显式地这样做，PHP 也会在脚本运行结束时自动关闭连接，当然，除非你用的是永久性连接。

* [了解 PDO 连接]


[pdo]: http://php.net/pdo
[SQL 注入]: http://wiki.hashphp.org/Validation
[了解 PDO]: http://php.net/book.pdo
[了解 PDO 连接]: http://php.net/pdo.connections
