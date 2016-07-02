---
layout: page
title: 基础
sitemap: true
---

# 基础

## 比较运算符

比较运算符是 PHP 中经常被忽视的一部分，导致产生许多意想不到的后果。其中一种问题源于严格比较（将布尔值与整型值进行比较）。

{% highlight php %}
<?php
$a = 5;   // 5 是个整型值

var_dump($a == 5);       // 比较值；返回 true
var_dump($a == '5');     // 比较值（忽略类型）；返回 true
var_dump($a === 5);      // 比较值和类型（整型值与整型值相比较）；返回 true
var_dump($a === '5');    // 比较值和类型（整型值和字符串相比较）；返回 false

/**
 * 严格比较
 */
if (strpos('testing', 'test')) {    // 在位置 0 上发现了 'test'，又被解释为布尔值 false
    // 代码...
}

// vs.

if (strpos('testing', 'test') !== false) {    // true，因为使用了严格比较（0 !== false）
    // 代码...
}
{% endhighlight %}

* [比较运算符](http://php.net/language.operators.comparison)
* [比较表](http://php.net/types.comparisons)
* [比较备忘条](http://phpcheatsheets.com/index.php?page=compare)

## 条件语句

### if 语句

在函数或者类中使用 `if`/`else`语句时，一个误解是必须联合使用 `else` 来声明可能出现的结果。然而，如果这个结果是用来确定返回值的，那么 `else` 就是不必要的，`return` 会结束函数，让 `else` 变得毫无意义。

{% highlight php %}
<?php
function test($a)
{
    if ($a) {
        return true;
    } else {
        return false;
    }
}

// vs.

function test($a)
{
    if ($a) {
        return true;
    }
    return false;    // else 是不必要的
}

// 还可以更短：

function test($a)
{
    return (bool) $a;
}

{% endhighlight %}

* [if 语句](http://php.net/control-structures.if)

### switch 语句

`switch` 语句是用来避免大量输入 `if` 和 `else if` 的好方法。不过有一些需要注意的事情：

- `switch` 语句只会比较值，而不会考虑类型（和 `==` 一样）
- 各个 `case` 会依次遍历，直到某一个匹配为止。如果没有匹配的 `case`，那么会使用 `default`（如果定义了的话）
- 没有 `break` 的话将会继续向下执行每一个 `case`，直到遇到了一个 `break` 或者 `return`
- 在一个函数内，使用 `return` 直接结束函数可以减少对 `break` 的需要

{% highlight php %}
<?php
$answer = test(2);    // case 2 和 case 3 的代码都会被执行

function test($a)
{
    switch ($a) {
        case 1:
            // 代码...
            break;             // break 用来结束 switch 语句
        case 2:
            // 代码...         // 没有 break，会继续执行 case 3 的内容
        case 3:
            // 代码...
            return $result;    // 在一个函数内，return 会直接结束函数
        default:
            // 代码...
            return $error;
    }
}
{% endhighlight %}

* [switch 语句](http://php.net/control-structures.switch)
* [PHP switch](http://phpswitch.com/)

## 全局命名空间

在使用命名空间时，你会发现内部函数会被你所写的函数覆盖。要解决这个问题，通过在函数名前面加一个反斜线就可以访问全局的函数：

{% highlight php %}
<?php
namespace phptherightway;

function fopen()
{
    $file = \fopen();    // 我们的函数与内部函数同名
                         // 通过加上「\」来执行内部函数
}

function array()
{
    $iterator = new \ArrayIterator();    // ArrayIterator 是一个内部类，不使用反斜线的话
                                         // 将会在你自己的命名空间内寻找这个类
}
{% endhighlight %}

* [全局空间](http://php.net/language.namespaces.global)
* [规则](http://php.net/userlandnaming.rules)

## 字符串

### 拼接

- 如果你的一行代码超过了推荐的行长度（120 个字符），考虑拼接多个行
- 为了可读性，最好使用拼接运算符，而不是拼接赋值运算符
- 在变量所处的范围内，如果拼接遇到新的一行时加上一个缩进


{% highlight php %}
<?php
$a  = 'Multi-line example';    // 拼接赋值运算符（.=）
$a .= "\n";
$a .= 'of what not to do';

// vs

$a = 'Multi-line example'      // 拼接运算符（.）
    . "\n"                     // 缩进新行
    . 'of what to do';
{% endhighlight %}

* [字符串运算符](http://php.net/language.operators.string)

### 字符串类型

字符串是一系列字符，听起来非常简单。不过，有几种不同类型的字符串，他们在语法和行为上都有些许不同。

#### 单引号

单引号用来表示一个「字面上的字符串」。这种字符串不会解析特殊字符和变量。

如果使用单引号时你将一个变量名放入字符串中，就像 `some $thing`，那么你会得到完全相同的输出，也就是 `some $thing`。如果使用双引号的话，`$thing` 会被求值，如果找不到这个变量就会出现错误。


{% highlight php %}
<?php
echo 'This is my string, look at how pretty it is.';    // 不需要解析一个简单的字符串

/**
 * 输出：
 *
 * This is my string, look at how pretty it is.
 */
{% endhighlight %}

* [单引号](http://php.net/language.types.string#language.types.string.syntax.single)

#### 双引号

双引号是字符串的瑞士军刀。它们不仅会像上面提到的那样解析变量，还会解析各种特殊字符，例如 `\n` 用于换行，`\t` 用于制表符等等。

{% highlight php %}
<?php
echo 'phptherightway is ' . $adjective . '.'     // 一个单引号的例子，与变量和转移字符串多次拼接
    . "\n"
    . 'I love learning' . $code . '!';

// vs

echo "phptherightway is $adjective.\n I love learning $code!"  // 双引号可以让我们使用解析的字符串，而不需要多次拼接
{% endhighlight %}

双引号可以包含变量，这样也被称为「插值」。

{% highlight php %}
<?php
$juice = 'plum';
echo "I like $juice juice";    // 输出：I like plum juice
{% endhighlight %}

在使用插值时，通常变量会与其他的字符相连，导致在变量名和字符之间产生一些误解。

要解决这个问题，可以用一对花括号括起变量。

{% highlight php %}
<?php
$juice = 'plum';
echo "I drank some juice made of $juices";    // $juice 无法解析

// vs

$juice = 'plum';
echo "I drank some juice made of {$juice}s";    // $juice 可以被解析

/**
 * 花括号内的复杂变量也可以被解析
 */

$juice = array('apple', 'orange', 'plum');
echo "I drank some juice made of {$juice[1]}s";   // $juice[1] 可以被解析
{% endhighlight %}

* [双引号](http://php.net/language.types.string#language.types.string.syntax.double)

#### Nowdoc 结构

PHP 5.3 引入了 Nowdoc 结构，从内部来说它的工作方式与单引号字符串相同，除了一点：它更适用于多行的字符串而不需要拼接。

{% highlight php %}
<?php
$str = <<<'EOD'             // 由 <<< 开始
Example of string
spanning multiple lines
using nowdoc syntax.
$a does not parse.
EOD;                        // 结尾的「EOD」必须单独在一行上的开始位置

/**
 * 输出：
 *
 * Example of string
 * spanning multiple lines
 * using nowdoc syntax.
 * $a does not parse.
 */
{% endhighlight %}

* [Nowdoc 结构](http://php.net/language.types.string#language.types.string.syntax.nowdoc)

#### Heredoc 结构

Heredoc 结构从内部来讲与双引号一样，但是更适合用于多行的的字符串而不需要拼接。

{% highlight php %}
<?php
$a = 'Variables';

$str = <<<EOD               // 由 <<< 开始
Example of string
spanning multiple lines
using heredoc syntax.
$a are parsed.
EOD;                        // 结尾的「EOD」必须单独在一行上的开始位置

/**
 * 输出：
 *
 * Example of string
 * spanning multiple lines
 * using heredoc syntax.
 * Variables are parsed.
 */
{% endhighlight %}

* [Heredoc 结构](http://php.net/language.types.string#language.types.string.syntax.heredoc)

### 哪一种更快？

有一种传说是单引号字符串会比双引号字符串稍稍快一些，这个说法从根本上就是错误的。

如果你只定义一个字符串，不去拼接值或者其他什么复杂的东西，那么单引号和双引号字符串是完全相同的，没有哪一个会更快一点。

如果你拼接多个各种类型的字符串，或者在双引号字符串里插值，那么结果可能会有所变化。如果你处理的值数量很少，那么拼接在性能上会有一丁点的优势；如果是很多值的话，插值反而又会有一点点优势。

不论你对字符串进行什么样的处理，没有哪一种类型会对你的程序带来可以察觉到的影响，改用另一种类型重新写一遍代码纯粹是徒劳无功，因此除非你真正了解了其中的差异所带来的意义和影响，不要去做这一点小小的优化。

* [反驳单引号性能传说](http://nikic.github.io/2012/01/09/Disproving-the-Single-Quotes-Performance-Myth.html)


## 三目运算符

三目运算符是一个让代码变得紧凑的好方法，但是往往会被过度使用。虽然三目运算符可以堆叠或者嵌套，不过还是建议你每个单独占一行从而更加可读。

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? 'yay' : 'nay';
{% endhighlight %}

为了比较，下面是一个为了减少代码行数而完全牺牲了可读性的写法：

{% highlight php %}
<?php
echo ($a) ? ($a == 5) ? 'yay' : 'nay' : ($b == 10) ? 'excessive' : ':(';    // excess nesting, sacrificing readability
{% endhighlight %}

要使用三目运算符 `return` 一个值，一定要使用正确的语法。

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? return true : return false;    // 这个例子会报错

// vs

$a = 5;
return ($a == 5) ? 'yay' : 'nope';    // 这个例子会返回 yay

{% endhighlight %}

要注意的是，你不需要使用三目运算符来返回布尔值，比如这个例子：

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? true : false; // 如果 $a == 3 的话会返回 true

// vs

$a = 3;
return $a == 3; // 如果 $a == 3 的话会返回 true

{% endhighlight %}

这一点对于所有的运算符都是一样的（`===`、`!==`、`!=`、`==`，等等）。

#### 搭配使用括号和三目运算符Utilising brackets with ternary operators for form and function

在使用三目运算符时，括号可以提升代码的可读性，并且可以在代码中建立分组。一个没有使用括号的需求的例子是：

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? "yay" : "nope"; // 如果 $a == 3 返回 yay，否则返回 nope

// vs

$a = 3;
return $a == 3 ? "yay" : "nope"; // 如果 $a == 3 返回 yay，否则返回 nope
{% endhighlight %}

括号还可以在语句中创建一个分组，从而作为整体进行判断。例如下面的这个例子会在 `($a == 3 && $b ==4)` 和 `$c == 5` 都为 `true` 时才返回 `true`：

{% highlight php %}
<?php
return ($a == 3 && $b == 4) && $c == 5;
{% endhighlight %}

还有一个例子，如果 `($a != 3 && $b != 4)` 或者 `($c == 5)` 时就会返回 `true`：

{% highlight php %}
<?php
return ($a != 3 && $b != 4) || $c == 5;
{% endhighlight %}

从 PHP 5.3 开始，我们可以忽略掉三目运算符的中间部分。表达式 `expr1 ?: expr3` 会在 `expr1` 求值为 `true` 时返回 `expr1` 自身，否则返回 `expr3`。

* [三目运算符](http://php.net/language.operators.comparison)

## 变量声名

有时候，开发者们会用一个不同的名字来声明一些预先声明过的变量，从而让代码看起来「更干净」，然而这样做带来的是双倍的内存消耗。例如下面的例子，我们假设字符串包含 1MB 的数据，为了复制变量，你将这个脚本的内存消耗增加到了 2MB。

{% highlight php %}
<?php
$about = 'A very long string of text';    // 占用 2MB 内存
echo $about;

// vs

echo 'A very long string of text';        // 占用 1MB 内存
{% endhighlight %}

* [性能建议](http://web.archive.org/web/20140625191431/https://developers.google.com/speed/articles/optimizing-php)
