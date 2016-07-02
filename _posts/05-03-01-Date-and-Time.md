---
title: 日期和时间
isChild: true
anchor:  date_and_time
---

## 日期和时间 {#date_and_time_title}

PHP 有一个名叫 `DateTime` 的类，帮助你读、写、比较并计算日期和时间。除了 `DateTime` 以外，PHP 还有很多与日期和时间相关的函数，但是 `DateTime` 为通常的使用提供了美观的面向对象接口。它也可以用来处理时区，但是这超出了这个简介的范围。

要开始使用 `DateTime`，通过 `createFromFormat()` 工厂方法把原生的日期和时间字符串转换成一个对象，或者用 `new DateTime` 来获取当前的日期和时间。`format()` 方法可以把 `DateTime` 转换回字符串从而可以输出。

{% highlight php %}
<?php
$raw = '22. 11. 1968';
$start = DateTime::createFromFormat('d. m. Y', $raw);

echo '开始日期：' . $start->format('Y-m-d') . "\n";
{% endhighlight %}

用 `DateTime` 进行计算可以通过 `DateInterval` 类来完成，`DateTime` 有一些例如 `add()` 和 `sub()` 的方法可以接受一个 `DateInterval` 的参数。不要在代码中假设每一天都一样长，不论是夏令时还是时区交替都会打破这个假设。你应该使用日期间的间隔。要计算日期的差异可以使用 `diff()` 方法，它会返回一个新的 `DateInterval`，非常容易显示出来。

{% highlight php %}
<?php
// 复制 $start 并且加上一个月零六天
$end = clone $start;
$end->add(new DateInterval('P1M6D'));

$diff = $end->diff($start);
echo '差距：' . $diff->format('%m 月 %d 天（一共 %a 天）') . "\n";
// 差距：1 月 6 天（一共 37 天）
{% endhighlight %}

你可以对 `DateTime` 对象进行标准的比较：

{% highlight php %}
<?php
if ($start < $end) {
    echo "start 在 end 之前！\n";
}
{% endhighlight %}

最后一个例子演示了 `DatePeriod` 类的用法。这个类可以用来遍历重复发生的时间，它需要两个 `DateTime` 对象作为开始和结束，以及一个日期间隔，从而返回其间所有的事件。

{% highlight php %}
<?php
// 输出 $start 和 $end 之间的所有星期四
$periodInterval = DateInterval::createFromDateString('first thursday');
$periodIterator = new DatePeriod($start, $periodInterval, $end, DatePeriod::EXCLUDE_START_DATE);
foreach ($periodIterator as $date) {
    // 输出这段时间内的所有日期
    echo $date->format('Y-m-d') . ' ';
}
{% endhighlight %}

* [了解 DateTime][datetime]
* [了解日期的格式化][dateformat]（可被接受的日期格式字符串选项）

[datetime]: http://php.net/book.datetime
[dateformat]: http://php.net/function.date
