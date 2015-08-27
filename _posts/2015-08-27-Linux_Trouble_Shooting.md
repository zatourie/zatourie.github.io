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


## strace

파일로 결과 저장
strace -o /tmp/id.strace id

파일에 관련된 것만 출력
strace -e trace=file id

프로세스에 관련된 것만 출력
strace -e trace=process id

### telnet 설치

yum install -y telnetd
service xinetd restart

yum install -y telnet

strace -p `pgrep xinetd`

strace -p `pgrep xinetd` -f -e trace=network

좋은데 결과를 읽기가 너무 힘들다 ㅠㅠ

http://chadfowler.com/blog/2014/01/26/the-magic-of-strace/
http://www.hokstad.com/5-simple-ways-to-troubleshoot-using-strace

조금더 간단한 명령 ltrace

### 실제 예제

fileopen 이라는 프로그램이 실행시 Permission denied가 뜬다

#### strace 실행

{% highlight bash%}
strace ./fileopen
{% endhighlight %>

#### 결과

{% highlight bash%}
open("/etc/shadow", O_RDONLY)           = -1 EACCES (Permission denied)
{% endhighlight %>

#### ltrace 실행

{% highlight bash%}
ltrace ./fileopen
{% endhighlight %>

#### 결과

{% highlight bash%}
fopen("/etc/shadow", "r")               = 0
perror("fopen"fopen: Permission denied
) 
{% endhighlight %>

## lsof - list open files

* 특정파일을 오픈하고 있는 프로세스 추적

{% highlight bash%}
lsof /etc/passwd
{% endhighlight %>

* ftp포트가 열고있는 파일리스트 

{% highlight bash%}
lsof -i:ftp 
{% endhighlight %>

* 명령 반복

{% highlight bash%}
lsof 0r 3 -i:telnet
{% endhighlight %>

* 특정 프로세스가 오픈하고 있는 파일 

{% highlight bash%}
lsof -p `pgrep sshd`
{% endhighlight %>

lsof -p 명령은 PID를 하나만 받으므로 sshd 프로세스가 여러개일경우 오류가 날 수 있다.

## netstat

{% highlight bash%}
netstat -atp
netstat -atp | grep telnet
{% endhighlight %>

 p 옵션 강추. 프로세스 정보도 함께 보여줌
