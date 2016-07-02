---
title: 数据库
anchor: databases
---

# 数据库 {#databases_title}

在很多时候你的 PHP 代码都会使用数据库来持久化信息。要连接并使用数据库，你有不少可选的方式，自从 **PHP 5.1.0** 开始推荐的方式是使用原生的驱动，比如 [mysqli]、[pgsql]、[mssql] 等等。

如果你的程序只需要使用**一个**数据库，那么原生的驱动会非常好用。不过，假如说你在用 MySQL 的同时还要用到一点 MSSQL，或者说你还要连接一个 Oracle 数据库，那么你就不能使用相同的驱动了，你需要为每一种数据库学习一组全新的 API——这么做太蠢了。


[mysqli]: http://php.net/mysqli
[pgsql]: http://php.net/pgsql
[mssql]: http://php.net/mssql
