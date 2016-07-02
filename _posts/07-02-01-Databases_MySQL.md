---
isChild: true
title:   MySQL 扩展
anchor:  mysql_extension
---

## MySQL 扩展 {#mysql_extension_title}

PHP 的 [mysql] 扩展太古老了，已经被另外两个扩展所取代： 

- [mysqli]
- [PDO]

[mysql] 不仅在很久之前就停止了开发，[在 PHP 5.5.0 版本中这个插件还被废除掉了][mysql_deprecated]，而且**[在 PHP 7.0 中被正式移除][mysql_removed]**。

为了不需要到 `php.ini` 中查看启用了哪些模块，你可以在选择的编辑器中搜索 `mysql_*`，如果出现了类似 `mysql_connect()` 或者 `mysql_query()` 之类的函数，那就说明正在使用 `mysql`。

即使你用的不是 PHP 7.0，不尽快升级到这个版本只会为之后真正需要更新的时候带来更大的困难。最佳的方案是在你的开发计划中使用 [mysqli] 或者 [PDO] 来代替 mysql，这样之后在升级时就没那么慌张了。

**如果你正在从 [mysql] 升级到 [mysqli]，要小心那些建议你直接把 `mysql_*` 查找替换成 `mysqli_*` 的教程。这样做不仅仅是过度的简单化，而且还忽略掉了 mysqli 带来的种种好处，例如参数绑定，[PDO] 也提供了这个特性。**

* [PHP：为 MySQL 选择一组 API][mysql_api]
* [MySQL 开发者的 PDO 教程][pdo4mysql_devs]

[mysql]: http://php.net/mysql
[mysql_deprecated]: http://php.net/migration55.deprecated
[mysql_removed]: http://php.net/manual/en/migration70.removed-exts-sapis.php
[mysqli]: http://php.net/mysqli
[PDO]: http://php.net/pdo
[mysql_api]: http://php.net/mysqlinfo.api.choosing
[pdo4mysql_devs]: http://wiki.hashphp.org/PDO_Tutorial_for_MySQL_Developers
