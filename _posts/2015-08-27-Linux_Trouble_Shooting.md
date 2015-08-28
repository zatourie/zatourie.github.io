---
layout: post
title: Linux Trouble Shooting
date: 2015-08-27 09:00:00
categories: DB
share: false
comments: true
---

# Shell Programming

{% highlight bash%}
#!/bin/bash
echo "Hello world!"
{% endhighlight %}

{% highlight bash%}
pid=`ps -e | grep crond | awk '{print $1}'`
kill -9 $pid
{% endhighlight %}

## 유용한 툴

* vi
* gedit
* awk
* sed

# Troble Shooting

## 도구

* 프로세스 디버깅 도구 : ps/pgrep/pstree/top
* 시스템콜(system call) 추적 도구 : strace
* 프로세스의 역추적 : 마법키? 사용
* 오픈파일 확인 도구 : lsof

## 프로세스 상태

![](http://www.makelinux.net/books/lkd2/graphics/03fig03.gif)

<http://www.makelinux.net/books/lkd2/ch03lev1sec1>

#### pid_max

{% highlight bash%}
man proc
{% endhighlight %}
