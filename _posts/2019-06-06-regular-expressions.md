---
title: 正则表达式
layout: post
---

### 实例：ip地址匹配
{% highlight php %}
'/^(2(5[0-5]{1}|[0-4]\d{1})|[0-1]?\d{1,2})(\.(2(5[0-5]{1}|[0-4]\d{1})|[0-1]?\d{1,2})){3}$/'
{% endhighlight %}

1. ```^(2(5[0-5]{1}|[0-4]\d{1})|[0-1]?\d{1,2})``` 表示第一块，范围是
2. ```(\.(2(5[0-5]{1}|[0-4]\d{1})|[0-1]?\d{1,2})){3}$/``` 表示后面3块， 范围是