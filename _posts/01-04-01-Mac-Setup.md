---
title: 在 Mac 上配置
isChild: true
anchor:  mac_setup
---

## 在 Mac 上配置 {#mac_setup_title}

OS X 预装了 PHP，但是通常都要比最新的稳定版本落后一些。Mavericks 预装的版本是 5.4.17，Yosemite 是 5.5.9，而 El Capitan 则是 5.5.29，但是在 PHP 7.0 发布之后这些预装的版本都不那么好了。

在 OS X 上安装 PHP 有很多种方法。

### 使用 Homebrew 安装 PHP

[Homebrew] 是 OS X 平台的一个功能强大的包管理器，可以帮助你方便地安装 PHP 以及各种扩展。[Homebrew PHP] 是一个包含着用于 Homebrew 的、与 PHP 相关的各种「配方」的仓库，可以让你用来安装 PHP。

到目前为止，你可以通过 `brew install` 命令来安装 `php53`、`php54`、`php55`、`php56` 或者 `php70` 这些版本，并且通过修改 `PATH` 变量来切换不同的版本。除此之外你还可以用 [brew-php-switcher][brew-php-switcher] 来自动切换。

### 使用 Macports 安装 PHP

[MacPorts] 是一个由社区发起的开源项目，设计了一个非常易于使用的系统用来编译、安装和更新 OS X 操作系统上的基于命令行、X11 和 Aqua 的开源软件。

MacPorts 提供了预先编译好的二进制文件，因此你不需要从源码包重新编译每一个依赖项。如果你的系统里没有安装任何包的话，MacPort 可以大大节约你的时间。

到目前为止，你可以通过 `port install` 命令来安装 `php54`、`php55`、`php56` 和 `php70` 这些版本。例如：

    sudo port install php56
    sudo port install php70

你可以运行 `select` 命令来切换当前使用的 PHP 版本：

    sudo port select --set php php70

### 使用 phpbrew 安装 PHP

[phpbrew] 是一个用来安装和管理多个 PHP 版本的工具。如果两个应用程序或者项目分别使用了不同版本的 PHP，那么这个工具会非常实用，你也不需要再使用虚拟机了。

### 使用 Liip 的二进制安装程序安装 PHP

还有一个非常热门的方法是使用 [php-osx.liip.ch] 提供的一键式安装工具来安装从 5.3 到 7.0 的各种版本。它不会覆盖掉苹果自带的 PHP 二进制文件，而是安装到一个独立的位置（例如 /usr/local/php5）。

### 从源代码编译

还有一个途径可以让你自由控制你所安装的 PHP 版本，那就是[自己编译][mac-compile]。如果你要自己编译的话，确保你已经安装了 [Xcode][xcode-gcc-substitution] 或者苹果的[「XCode 命令行工具」]，可以从苹果的 Mac 开发人员中心下载到。

### 多合一安装包

以上列出的方法大多只用来安装 PHP 本体，不会包括诸如 Apache、Nginx 或者一个 SQL 服务器之类的东西。一些「多合一」的方法可以把这些软件捆绑起来一起安装，例如 [MAMP][mamp-downloads] 和 [XAMPP][xampp]，但是伴随着简易的安装而来的则是在灵活性上的妥协。


[Homebrew]: http://brew.sh/
[Homebrew PHP]: https://github.com/Homebrew/homebrew-php#installation
[MacPorts]: https://www.macports.org/install.php
[phpbrew]: https://github.com/phpbrew/phpbrew
[php-osx.liip.ch]: http://php-osx.liip.ch/
[mac-compile]: http://php.net/install.macosx.compile
[xcode-gcc-substitution]: https://github.com/kennethreitz/osx-gcc-installer
[「XCode 命令行工具」]: https://developer.apple.com/downloads
[mamp-downloads]: http://www.mamp.info/en/downloads/
[xampp]: http://www.apachefriends.org/en/xampp.html
[brew-php-switcher]: https://github.com/philcook/brew-php-switcher
