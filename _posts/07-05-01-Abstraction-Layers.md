---
isChild: true
title:   抽象层
anchor:  databases_abstraction_layers
---

## 抽象层 {#databases_abstraction_layers_title}

大多数框架都提供了一个抽象层，可能会也可能不会建立在 [PDO][1] 的基础上。通常这些抽象层可以用 PHP 方法封装你的查询，来模拟一些在一个数据库系统里存在、而在另一个里不存在的功能，从而带给你真正的数据库抽象而不是 PDO 提供的连接上的抽象。当然，这样做会带来一定的开销，不过如果你需要搭建一个可移植的、可以与 MySQL、PostgreSQL 和 SQLite 等等多种数据库工作的程序，那么这点开销相比较于代码的整洁性而言还是非常值得的。

一些抽象层是遵循 [PSR-0][psr0] 或者 [PSR-4][psr4] 命名空间标准来编写的，因此可以用在任何程序中，其中包括：

* [Aura SQL][6]
* [Doctrine2 DBAL][2]
* [Propel][7]
* [ZF2 Db][4]


[1]: http://php.net/book.pdo
[2]: http://www.doctrine-project.org/projects/dbal.html
[4]: http://packages.zendframework.com/docs/latest/manual/en/index.html#zend-db
[6]: https://github.com/auraphp/Aura.Sql
[7]: http://propelorm.org/
[psr0]: http://www.php-fig.org/psr/psr-0/
[psr4]: http://www.php-fig.org/psr/psr-4/
