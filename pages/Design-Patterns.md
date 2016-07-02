---
layout: page
title: 设计模式
sitemap: true
---

# 设计模式

在 web 应用程序的代码和项目结构上有很多种可用的方法，在考虑架构上花费的工夫也完全由你的喜好决定。不过遵循一些常见的设计模式通常都是个好主意，可以让你的代码更加易于管理，也让别人更容易理解。

* [维基百科上的「架构模式」](https://en.wikipedia.org/wiki/Architectural_pattern)
* [维基百科上的「软件设计模式」](https://en.wikipedia.org/wiki/Software_design_pattern)
* [实现设计模式的案例集合](https://github.com/domnikl/DesignPatternsPHP)

## 工厂模式

工厂模式是最常用的设计模式之一。在这个模式中，由一个类来创建你所想要使用的对象。考虑下面的工厂模式的例子：

{% highlight php %}
<?php
class Automobile
{
    private $vehicleMake;
    private $vehicleModel;

    public function __construct($make, $model)
    {
        $this->vehicleMake = $make;
        $this->vehicleModel = $model;
    }

    public function getMakeAndModel()
    {
        return $this->vehicleMake . ' ' . $this->vehicleModel;
    }
}

class AutomobileFactory
{
    public static function create($make, $model)
    {
        return new Automobile($make, $model);
    }
}

// 让工厂创建 Automobile 对象
$veyron = AutomobileFactory::create('Bugatti', 'Veyron');

print_r($veyron->getMakeAndModel()); // 输出「Bugatti Veyron」
{% endhighlight %}

这段代码使用了一个工厂来创建 `Automobile` 对象。像这样编写代码有两种可能的好处：第一是如果你之后需要改变、重命名或者替换掉 `Automobile` 类，你可以直接去做，然后你只需要修改工厂中的代码，而不是去修改项目中所有用到了 `Automobile` 类的代码；第二个可能的好处是如果创建对象是一个非常复杂的工作，那么你可以在工厂中完成所有的这些工作，而不是在每一次创建一个实例时重复这些工作。

使用工厂模式并不总是必须的（或者明智的）。这里的例子非常简单，使用工厂反而会增加不必要的复杂度。不过，如果你正在编写一个规模相当大或者很复杂的项目，那么使用工厂模式可能会在之后的编码过程中为你省去大量的麻烦。

* [维基百科上的「工厂模式」](https://en.wikipedia.org/wiki/Factory_pattern)

## 单例模式

在设计 web 应用程序时，有时候对一个类只允许访问其一个且唯一一个实例，不论在概念上还是在架构上都很重要。单例模式让我们可以实现这一点：

{% highlight php %}
<?php
class Singleton
{
    /**
     * @var Singleton The reference to *Singleton* instance of this class
     */
    private static $instance;
    
    /**
     * Returns the *Singleton* instance of this class.
     *
     * @return Singleton The *Singleton* instance.
     */
    public static function getInstance()
    {
        if (null === static::$instance) {
            static::$instance = new static();
        }
        
        return static::$instance;
    }

    /**
     * Protected constructor to prevent creating a new instance of the
     * *Singleton* via the `new` operator from outside of this class.
     */
    protected function __construct()
    {
    }

    /**
     * Private clone method to prevent cloning of the instance of the
     * *Singleton* instance.
     *
     * @return void
     */
    private function __clone()
    {
    }

    /**
     * Private unserialize method to prevent unserializing of the *Singleton*
     * instance.
     *
     * @return void
     */
    private function __wakeup()
    {
    }
}

class SingletonChild extends Singleton
{
}

$obj = Singleton::getInstance();
var_dump($obj === Singleton::getInstance());             // bool(true)

$anotherObj = SingletonChild::getInstance();
var_dump($anotherObj === Singleton::getInstance());      // bool(false)

var_dump($anotherObj === SingletonChild::getInstance()); // bool(true)
{% endhighlight %}

上面的代码通过一个[**静态**变量](http://php.net/language.variables.scope#language.variables.scope.static)和一个静态的创建方法 `getInstance()` 实现了单例模式。要注意以下几点：

* 构造方法 [`__construct()`](http://php.net/language.oop5.decon#object.construct) 被声明为受保护的（protected），从而防止在类的外面使用 `new` 运算符建立一个新的实例；
* 魔术方法 [`__clone()`](http://php.net/language.oop5.cloning#object.clone) 被声明为私有的从而防止使用 [`clone`](http://php.net/language.oop5.cloning) 运算符克隆这个类的实例；
* 魔术方法 [`__wakeup()`](http://php.net/language.oop5.magic#object.wakeup) 被声明为私有的，从而防止使用全局函数 [`unserialize()`](http://php.net/function.unserialize) 来反序列化这个类的实例；
* 新的实例是通过在静态方法 `getInstance()` 中使用关键字 `static` 通过[后期静态绑定](http://php.net/language.oop5.late-static-bindings)来实现的。这样可以允许建立 `Singleton` 类的子类。

单例模式在需要确保一个类在 web 应用的整个请求的生命周期内只有唯一一个实例时非常有用，这种情况通常会在我们使用全局对象（例如一个配置类）或者使用共享的资源（例如事件队列）时发生。

你应当在使用单例模式时保持谨慎，因为它会为你的程序带来全局的状态，从而减弱了程序的可测试性。在大多数情况下，可以（并且应当）使用依赖注入来代替一个单例类。使用依赖注入意味着我们不会为程序的设计增加不必要的耦合度，因为使用共享或者全局资源的对象不需要对一个类的具体定义有所了解。

* [维基百科上的「单例模式」](https://en.wikipedia.org/wiki/Singleton_pattern)

## 策略模式

使用策略模式，你可以封装特定的算法族群，让客户类来负责实现算法，从而不需要对算法的具体实现有所了解。策略模式有很多变种，下面介绍了最简单的一种：

第一段代码展示了一个算法族群。你也许想得到一个连续的数组、一些 JSON 数据或者仅仅是一个数组而已：

{% highlight php %}
<?php

interface OutputInterface
{
    public function load();
}

class SerializedArrayOutput implements OutputInterface
{
    public function load()
    {
        return serialize($arrayOfData);
    }
}

class JsonStringOutput implements OutputInterface
{
    public function load()
    {
        return json_encode($arrayOfData);
    }
}

class ArrayOutput implements OutputInterface
{
    public function load()
    {
        return $arrayOfData;
    }
}
{% endhighlight %}

通过封装上面的算法，你让你的代码变得更加美观干净，让其他的开发者可以轻松地添加新的输出类型而不需要影响客户代码。

你可以看到每一个具体的「输出」类都实现了 `OutputInterface`，这样做有两个目的，首先这样做向所有新的具体实现提供了一个必须遵守的合约，其次，通过实现一个公有的接口，你可以在下面一节看到你可以使用 [类型约束](http://php.net/language.oop5.typehinting)来确保使用这些行为的客户代码符合正确的类型（在这里是 `OutputInterface`）。

下面一段代码展示了一个调用的客户代码可以使用这些算法的其中一个，并且可以在运行时设置需要的行为：

{% highlight php %}
<?php
class SomeClient
{
    private $output;

    public function setOutput(OutputInterface $outputType)
    {
        $this->output = $outputType;
    }

    public function loadOutput()
    {
        return $this->output->load();
    }
}
{% endhighlight %}

调用的客户类具有一个私有的属性，必须在运行时设置而且必须是 `OutputInterface` 类型。设置了这个属性之后，调用 `loadOutput()` 就会调用所设置的具体实现输出类型的类中的 `load()` 方法。

{% highlight php %}
<?php
$client = new SomeClient();

// 想得到一个数组？
$client->setOutput(new ArrayOutput());
$data = $client->loadOutput();

// 想得到 JSON？
$client->setOutput(new JsonStringOutput());
$data = $client->loadOutput();

{% endhighlight %}

* [维基百科上的「策略模式」](http://en.wikipedia.org/wiki/Strategy_pattern)

## 前端控制器模式

前端控制器模式就是让你的 web 应用只拥有一个入口点（例如 index.php）并处理所有的请求，这个入口点的代码要加载所有的依赖项、处理请求然后向浏览器发送响应。因为前端控制器模式鼓励代码的模块化，并且给了你一个用来编写将会嵌入所有请求的代码的地方（例如处理非法输入），因此这一模式通常是很有帮助的。

* [维基百科上的「前端控制器模式」](https://en.wikipedia.org/wiki/Front_Controller_pattern)

## 模型-视图-控制器模式

模型-视图-控制器（model-view-controller，MVC）模式以及与之相关的 HMVC 和 MVVM 模式可以让你将代码分割到不同的逻辑对象中，各司其职。模型会作为一个数据访问层，用来获取数据并以可用的形式返回从而在程序中使用；控制器处理请求和模型返回的数据，并且加载视图从而返回响应；视图则是用于显示的模板（标记、XML 等等），作为响应发送给浏览器。

MVC 模式是在流行的 [PHP 框架](https://github.com/codeguy/php-the-right-way/wiki/Frameworks) 中使用最广泛的架构模式。

了解更多有关 MVC 模式以及其他相关的模式的内容：

* [MVC](https://en.wikipedia.org/wiki/Model%E2%80%93View%E2%80%93Controller)
* [HMVC](https://en.wikipedia.org/wiki/Hierarchical_model%E2%80%93view%E2%80%93controller)
* [MVVM](https://en.wikipedia.org/wiki/Model_View_ViewModel)
