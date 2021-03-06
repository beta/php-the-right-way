---
title:   好处
isChild: true
anchor:  templating_benefits
---

## 好处 {#templating_benefits_title}

使用模板带来的最主要的好处是，表示层逻辑与程序的其他部分可以清晰地分离开来。模板的责任很专一，就是显示格式化之后的内容。它们不会负责数据的查询、持久化或者其他更加复杂的工作，这样可以让代码变得更加简单、可读，对于程序员开发服务器后端代码（控制器和模型）、设计师编写前端的代码（样式）的团队工作环境来说是很有帮助的。

模板还可以改善表示层代码的结构。通常模板都会放在一个名叫「views」（视图）的文件夹内，每一个模板都用一个单独的文件来编写。这种组织方式鼓励代码的重用，可以让大段的代码分散成更小的、更加可以重用的代码块（通常称作部分代码）。例如，网站的头尾部分可以分别定义成一个模板，就可以在其他每一个页面模板的开头和结尾处导入。

最后，根据你所使用的库，模板可以通过自动转义用户生成的内容从而带来更好的安全性。一些库甚至还提供了沙盒机制，模板设计者只能获得白名单内的变量和函数的访问权限。