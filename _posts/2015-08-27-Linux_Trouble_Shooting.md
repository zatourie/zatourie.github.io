---
layout: post
title: Linux Trouble Shooting
date: 2015-08-27 09:00:00
categories: DB
share: false
comments: true
---

{% highlight %}
#!/bin/bash
echo "Hello world!"
{% endhighlight %}

{% highlight bash%}
pid=`ps -e | grep crond | awk '{print $1}'`
kill -9 $pid
{% endhighlight %}

awk / sed
