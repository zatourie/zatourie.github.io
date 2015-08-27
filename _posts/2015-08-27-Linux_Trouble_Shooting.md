---
layout: post
title: Linux Trouble Shooting
date: 2015-08-27 09:00:00
categories: DB
share: false
comments: true
---

# Shell 

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

#### Process State Codes
{% highlight bash%}
man ps 
{% endhighlight %}

### 좀비 프로세스 확인 및 처리

ps -el 

#### 좀비프로세스 확인

{% highlight bash%}
ps-el | awk '$2=="Z" { print $5 }'
{% endhighlight %>

#### zombie.sh

{% highlight bash%}
#!/bin/bash
# check the zombie process
ppid=`ps -el | awk '$2=="Z" {print $5}'`
for pid in $ppid
do
        ps -e | grep $pid
done
{% endhighlight %>

### thread 조회

{% highlight bash%}
gcc countThead.c -o countThread -lpthread
ps -eL | grep countThread
ps -eLo "pcpu,rss,vsz,command" | grep countThread
ps -eo "pid,ppid,rss,vsz,command" | grep firefox
{% endhighlight %>

### Process monitoring

{% highlight bash%}
#!/bin/bash
#Monitoring processes
if [ $# -ne 1 ]
then
        echo "Usage : $0 process_name"
        exit 1
fi
while true
do
        ps -eo "pid,ppid,rss,vsz,command" | grep $1 | grep -v grep
        sleep 3
done
{% endhighlight %>

## pgrep, pstree, top
