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

{% highlight bash%}
ps -el
{% endhighlight %}

#### 좀비프로세스 확인

{% highlight bash%}
ps-el | awk '$2=="Z" { print $5 }'
{% endhighlight %}

#### zombie.sh

{% highlight bash%}
#!/bin/bash
# check the zombie process
ppid=`ps -el | awk '$2=="Z" {print $5}'`
for pid in $ppid
do
        ps -e | grep $pid
done
{% endhighlight %}

### thread 조회

{% highlight bash%}
gcc countThead.c -o countThread -lpthread
ps -eL | grep countThread
ps -eLo "pcpu,rss,vsz,command" | grep countThread
ps -eo "pid,ppid,rss,vsz,command" | grep firefox
{% endhighlight %}

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
{% endhighlight %}

## pgrep, pstree, top


## strace

파일로 결과 저장

{% highlight bash %}
strace -o /tmp/id.strace id
{% endhighlight %}

파일에 관련된 것만 출력

{% highlight bash %}
strace -e trace=file id
{% endhighlight %}

프로세스에 관련된 것만 출력

{% highlight bash %}
strace -e trace=process id
{% endhighlight %}

### telnet 설치

{% highlight bash %}
yum install -y telnetd

service xinetd restart

yum install -y telnet

strace -p `pgrep xinetd`

strace -p `pgrep xinetd` -f -e trace=network
{% endhighlight %}

좋은데 결과를 읽기가 너무 힘들다 ㅠㅠ

참고

<http://chadfowler.com/blog/2014/01/26/the-magic-of-strace/>

<http://www.hokstad.com/5-simple-ways-to-troubleshoot-using-strace>

조금더 간단한 명령 ltrace

### 실제 예제

fileopen 이라는 프로그램이 실행시 Permission denied가 뜬다

#### strace 실행

{% highlight bash %}
strace ./fileopen
{% endhighlight %}

#### 결과

{% highlight bash%}
open("/etc/shadow", O_RDONLY)           = -1 EACCES (Permission denied)
{% endhighlight %}

#### ltrace 실행

{% highlight bash%}
ltrace ./fileopen
{% endhighlight %}

#### 결과

{% highlight bash%}
fopen("/etc/shadow", "r")               = 0
perror("fopen"fopen: Permission denied
)
{% endhighlight %}

## lsof - list open files

### 특정파일을 오픈하고 있는 프로세스 추적

{% highlight bash %}
lsof /etc/passwd
{% endhighlight %}

###  ftp포트가 열고있는 파일리스트

{% highlight bash %}
lsof -i:ftp
{% endhighlight %}

###  명령 반복

{% highlight bash %}
lsof 0r 3 -i:telnet
{% endhighlight %}

###  특정 프로세스가 오픈하고 있는 파일

{% highlight bash %}
lsof -p `pgrep sshd`
{% endhighlight %}

* lsof -p 명령은 PID를 하나만 받으므로 sshd 프로세스가 여러개일경우 오류가 날 수 있다.

## netstat

{% highlight bash%}
netstat -atp
netstat -atp | grep telnet
{% endhighlight %}

 p 옵션 강추. 프로세스 정보도 함께 보여줌


## ifconfig

UP BROADCAST RUNNING MULTICAST
올라와있고 broadcast 가능하며 실행되고 있고 multicast가능

* eth+숫자 : 외부와 통신
* lo : loopback
* ::1/128 -> ipv6 앞부분이 모두 00인 경우 표기법
* /etc/sysconfig/network-scripts/ifcfg-eth? 네트웍설정

## Gateway 확인

{% highlight bash %}
route
{% endhighlight %}

or

{% highlight bash %}
netstat -r
{% endhighlight %}

## ARP

OSI 1, 2계층에서는 mac addr을 사용하여 통신함

arp 테이블에서 IP와 mac addr을 매핑해둔다

## tcpdump

{% highlight bash%}
tcpdump -i lo -w /tmp/tcpdump.log

tcpdump -r /tmp/tcpdump.log port telnet
{% endhighlight %}


## wireshark

{% highlight bash%}
yum install wireshark-gnome
{% endhighlight %}

## lo 살리기

{% highlight bash%}
ifup lo
{% endhighlight %}

# Linux Filesystem

![](http://www.learnlinux.org.za/courses/build/images/diagram07.png)

![](http://docstore.mik.ua/orelly/networking/puis/figs/puis_0501.gif)

## history

ext2 -> ext3 (journaling) -> ext4 (journal checksum, delayed file space allocation)

<http://www.linuxdevelopernews.com/a-brief-history-of-ext2-ext3-and-ext4-2011-12>


xfs, brfs(Btree Filesystem)

----
## 참고
http://egaoneko.github.io/os/2015/05/24/linux-starter-guide-4.html

## e2fsck

{% highlight bash%}
[root@localhost VMware Tools]# umount /data
[root@localhost VMware Tools]# df
Filesystem     1K-blocks     Used Available Use% Mounted on
/dev/sda2       18143556  5160340  12054912  30% /
tmpfs            1958420      224   1958196   1% /dev/shm
/dev/sda1         289293    74645    199288  28% /boot
.host:/        104318044 71775404  32542640  69% /mnt/hgfs
/dev/sr0           71618    71618         0 100% /media/VMware Tools
[root@localhost VMware Tools]#

[root@localhost VMware Tools]# e2fsck /dev/sdb1
e2fsck 1.41.12 (17-May-2010)
/dev/sdb1: clean, 11/327680 files, 55935/1309289 blocks
[root@localhost VMware Tools]# e2fsck -f /dev/sdb1
e2fsck 1.41.12 (17-May-2010)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/sdb1: 11/327680 files (0.0% non-contiguous), 55935/1309289 blocks
[root@localhost VMware Tools]#
{% endhighlight %}

## mke2fs 를 이용한 superblock backup확인

`mke2fs /dev/sdb1 --날라감 포맷명령`

{% highlight bash%}
[root@localhost VMware Tools]# mke2fs -n /dev/sdb1
mke2fs 1.41.12 (17-May-2010)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1309289 blocks
65464 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736

[root@localhost VMware Tools]#
{% endhighlight %}
