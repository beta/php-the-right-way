---
title: 在 Windows 上配置
isChild: true
anchor:  windows_setup
---

## 在 Windows 上配置 {#windows_setup_title}

你可以从 [windows.php.net/download][php-downloads] 下载到 PHP 的二进制文件。解压之后，推荐你将 PHP 的根目录（php.exe 所在的地方）添加到 [PATH][windows-path] 中从而可以在任何地方执行 PHP。

如果是用于学习和本地开发，你可以使用从 PHP 5.4 起内置的 web 服务器，不需要额外的配置。如果你想使用一个集成了全功能 web 服务器和 MySQL 的「多合一」安装包的话，可以使用例如 [Web Platform Installer][wpi]、[XAMPP][xampp]、[EasyPHP][easyphp]、[OpenServer][openserver] 以及 [WAMP][wamp] 之类的工具来快速搭建一个 Windows 下的开发环境。顺便提一句，这些工具与生产环境相比会有一些小小的不同，因此如果你在 Windows 上开发而在 Linux 上面部署，你需要格外注意这些环境上的差别。

如果你需要在 Windows 上面运行你的生产系统，那么 IIS7 会是最稳定的、最高性能的选择。你可以使用 [phpmanager][phpmanager]（一个 IIS7 的 GUI 插件）来简化 PHP 的配置和管理。IIS7 内置了 FastCGI，你只需要将 PHP 配置为一个处理程序即可。在 [iis.net 的一个专用板块][php-iis] 里面可以找到更多的支持信息和额外的资源。

通常来说，在不同的开发和生产环境上运行程序通常会在上线时带来一些奇怪的错误。如果你在 Windows 上面开发同时又要部署到 Linux（或者其他的非 Windows 系统），那么你应该考虑使用一个[虚拟机](/#virtualization_title)。

Chris Tankersley 的一篇博客文章介绍了他[在 Windows 上开发 PHP 所使用的工具][windows-tools]。

[easyphp]: http://www.easyphp.org/
[phpmanager]: http://phpmanager.codeplex.com/
[openserver]: http://open-server.ru/
[wamp]: http://www.wampserver.com/en/
[php-downloads]: http://windows.php.net/download/
[php-iis]: http://php.iis.net/
[windows-path]: http://www.windows-commandline.com/set-path-command-line/
[windows-tools]: http://ctankersley.com/2015/07/01/developing-on-windows/
[wpi]: http://www.microsoft.com/web/downloads/platform.aspx
[xampp]: http://www.apachefriends.org/en/xampp.html
