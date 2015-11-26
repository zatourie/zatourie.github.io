---
layout: post
title: Secure Coding 교육
excerpt: Secure Coding 교육 정리노트
date:   2015-11-24 09:00:00
categories:
tags: [securecoding]
author: mango
share: true
comments: true  
---

# What is vulnerability

> In computer security, a vulnerability is a weakness which allows an attacker to reduce a system's information assurance.

from https://en.wikipedia.org/wiki/Vulnerability_(computing)

In short, a weakness is in theory, a vulnerability is in practice.


# Refs

* 행자부 (Secure Coding Guide)
* CWE (Mitre's Common Weakness Enumeration) cf)CVE(Common Vulnerability Exposure)
* OWASP
* CERT (Secure Coding Standard)

# Minimum Requirement

* CWE Top 25 http://cwe.mitre.org/top25/
* OWASP Top 10 https://www.google.co.kr/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwix_KHtga3JAhXBF5QKHedtB08QFgghMAA&url=https%3A%2F%2Fwww.owasp.org%2Fimages%2F2%2F2c%2FOWASP_Top_10_-_2013_Final_-_Korean.pdf&usg=AFQjCNE1EkTUTkxfsVwHCLfnozE7J21Kpg&bvm=bv.108194040,d.dGo

# Mitigate Vulnerability

* Network Layer : Traffic, Packet monitoring
* App Op Layer : Request, pattern monitoring
* App Dev Layer : Secure Coding

# Cases

* Authentication flaw + non-authorized request exploitation
* Webshell[1] attack using Paros[2]
* Library vulneribility exploitation


----
# etc

Secure Coding 진단원 

OAuth 2.0 https://developers.daum.net/services/apis/docs/oauth2_0/reference

# Mitre corporation

* http://www.mitre.org/
* https://en.wikipedia.org/wiki/Mitre_Corporation
* http://www.oxforddictionaries.com/definition/english/mitre 

# Qualys

https://www.ssllabs.com/ssltest/analyze.html?d=skbiok.com

https://www.ssllabs.com/projects/documentation/index.html

# MS SDL (Security Development Lifecycle)

* https://www.microsoft.com/en-us/sdl/

![](http://i.msdn.microsoft.com/cc448177.SDL-Lifecycle-gradient_0609(en-us,MSDN.10).jpg)

![MS-SDL openeg](http://cfile22.uf.tistory.com/R750x0/227AA74355630C1A13AFA8)

* DREAD : http://www.openeg.co.kr/541

![](http://image.slidesharecdn.com/praetorianthreatmodelingpresentation-110117095334-phpapp02/95/threat-modeling-improve-security-drive-testing-reduce-costs-25-728.jpg?cb=1332160190) 

* STRIDE 

![](http://image.slidesharecdn.com/praetorianthreatmodelingpresentation-110117095334-phpapp02/95/threat-modeling-improve-security-drive-testing-reduce-costs-20-728.jpg?cb=1332160190)

![](https://i-technet.sec.s-msft.com/en-us/security/hh855044.STRIDE-definitions(en-us,MSDN.10).jpg)

![](https://i-technet.sec.s-msft.com/en-us/security/hh855044.STRIDE-mitigation-categories(en-us,MSDN.10).jpg)

# OWASP Webgoat Project

* WebGoat is a deliberately insecure web application maintained by OWASP designed to teach web application security lessons. You can install and practice with WebGoat in either J2EE (this page) or WebGoat for .Net in ASP.NET. In each lesson, users must demonstrate their understanding of a security issue by exploiting a real vulnerability in the WebGoat applications. For example, in one of the lessons the user must use SQL injection to steal fake credit card numbers. The application is a realistic teaching environment, providing users with hints and code to further explain the lesson.
* https://www.owasp.org/index.php/Category:OWASP_WebGoat_Project

# HSQLDB: http://hsqldb.org/

* HSQLDB (HyperSQL DataBase) is the leading SQL relational database software written in Java. It offers a small, fast multithreaded and transactional database engine with in-memory and disk-based tables and supports embedded and server modes. It includes a powerful command line SQL tool and simple GUI query tools.
* Benchmark: http://hsqldb.org/PolePosition.pdf

# web.xml 웹서버 설정 폴더 리스팅 / 파일쓰기 권한 등 

# 보안관련 Encoding

* URL Encoding : URL에 사용할 수 있는 형태로 encoding
* HTML Encoding : 브라우저 렌더링 escape

# 1. 입력데이터 검증 및 표현

# MySQL SQL Injection

## SQL Injection Sample

    ' union select 1,2,3,4,5,6 #
    ' union select schema_name,2,3,4,5,6 from information_schema.schemata#
    ' union select table_name,2,3,4,5,6 from information_schema.tables where table_schema = database()#
    ' union select group_concat(column_name),2,3,4,5,6 from information_schema.columns where table_name = '테이블명'#

## Mitigate SQL Injection

* Use static query 
* Use PreparedStatement properly when using JDBC API
* Use # in My/IBatis query
* In case of dynamic query, use a proper filter to remove/replace special characters which enable attacks

# Access Control 경로조작 및 자원 삽입

## 주어진 권한을 넘어서는 리소스 탈취/수정

## Mitigate Access Control Attack

* Application에게 과도한 권한을 주지 말 것: root, administrator 사용금지
* Application내에서 whitelist, input string validation 등 수행


# Cross Site Script (XSS)

클라이언트 대상 해킹기법으로 악성코드를 클라이언트에서 실행시켜 원하는 데이터를 탈취함.

## Type (regarding where malicious script originated)

* Stored XSS
![](http://excess-xss.com/persistent-xss.png)

* Reflected XSS
![](http://excess-xss.com/reflected-xss.png)

* DOM XSS
![](http://excess-xss.com/dom-based-xss.png)

## Mitigate

* Using JSTL

{% highlight java%}
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
....
<c:out value="${board.content}" />
....
{% endhighlight %}

or

{% highlight java%}
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
....
<${fn:escapeXml(board.content)}
....
{% endhighlight %}

* Using Lucy XssPreventer/XssFilter : https://github.com/naver/lucy-xss-filter

{% highlight java%}
String dirty = "\"><script>alert('xss');</script>";
String clean = XssPreventer.escape(dirty); //&quot;&gt;&lt;script&gt;alert(&#39xss&#39);&lt;/script&gt;

String dirty = "<img src=\"<img src=1\\ onerror=alert(1234)>\" onerror=\"alert('XSS')\">";
String clean = filter.doFilter(dirty); //<img src=\"\"><!-- Not Allowed Attribute Filtered ( onerror=alert(1234)) --><img src=1\\>\" onerror=\"alert('XSS')\"&gt;
{% endhighlight %}

* Filter 사용 : http://www.openeg.co.kr/383

Example above is too basic. Consult link below for smarter attacks 

OWASP Cheat Sheet : https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet

And here's a first hand experience of "Mass SQL Injection" http://using.tistory.com/11, Korean

# Malicious File Upload

* 웹에서 직접 접근이 불가능한 위치에 파일 업로드 할 것
* 사용자가 올린 파일명과 저장된 물리적 파일명을 다르게 할 것 (둘 사이의 연결 정보 관리필요)
* 실행가능한 파일 업로드 불가
* 파일을 업로드하면서 weakness를 모두 없애는 것은 쉽지 않다

# 신뢰되지 않는 URL주소로 Redirect

* URL주소를 whitelist로 관리
* 적절하게 encode

# XPath Injection

* XQuery를 사용하라 (PreparedStatement와 유사)

# LDAP Injection

* 검색어);추가정보; : 검색어 정보와 추가정보가 함께 조회됨
* 검증되지 않은 입력값 검증 필요 

# Cross Site Request Forgery (CSRF)

![](http://www.opensourceforu.efytimes.com/wp-content/uploads/2010/11/Figure-1-CSRF-attack-on-GET.png)

## Mitigate CSRF

* CSRF TOKEN으로 실제 시나리오상의 요청인지 검증
* 사용자에게 실제 요청의 의도를 확인한다 : 홍길동에게 100만원을 이체하시겠습니까?
* mitigate csrf : https://www.google.com/recaptcha
* 이런 의견도 있음. : https://gist.github.com/homakov/5607607
* 정적 소스코드 진단으로는 검출되지 않으며, 동적진단을 통해 검출해야한다
* 

# HTTP 응답 분할 (HTTP Response Splitting)

* OWASP : https://www.owasp.org/index.php/HTTP_Response_Splitting
* JDK 1.6 이상에서는 해결되었다? 확인필요
* MITRE 참조 : https://cwe.mitre.org/data/definitions/113.html

# 정수형 오버플로우 Integer overflow

* OWASP : https://www.owasp.org/index.php/Integer_overflow
* Java는 Integer overflow가 발생하지 않는다. 연산오류가 있을 뿐. http://www.mkyong.com/java/javas-silent-killer-buffer-overflow-careful/

## Android Stagefright Vulnerability

* Blog : https://blog.zimperium.com/the-latest-on-stagefright-cve-2015-1538-exploit-is-now-available-for-testing-purposes/
* Demo : https://www.youtube.com/watch?v=PxQc5gOHnKs
* CVE-2015-1538

# 메모리 버퍼 오버플로우 Memory buffer overflow

* http://www.openeg.co.kr/331
* Java와 C#처럼 managed code를 사용하는 언어는 관계없음

# Format String 

* 포맷스트링 함수를 이용한 공격
* 이것도 Java와 C#처럼 managed code를 사용하는 언어는 관계없음

# 2. 보안기능

# Secure Coding Conclusion : 

> *DO NOT BELIEVE ANYONE*

> *CONSIDER USER AS ATTACKER*

> Secure Coding을 다섯자로 줄이면, 입력값검증

# Java Cripytography

## [Java Cryptography Architecture(JCA)](https://en.wikipedia.org/wiki/Java_Cryptography_Architecture)

> The Java Cryptography Architecture (JCA) is a framework for working with cryptography using the Java programming language.

## [Java Cryptography Extension(JCE)](https://en.wikipedia.org/wiki/Java_Cryptography_Extension)

> The Java Cryptography Extension (JCE) is an officially released Standard Extension to the Java Platform. JCE provides a framework and implementation for encryption, key generation and key agreement, and Message Authentication Code (MAC) algorithms.


----

# Footnotes

[1] Wehshell : 웹상에서 서버 CLI를 사용하도록 해주는 툴

http://darksoulstory.tistory.com/324 
https://www.trustwave.com/Resources/SpiderLabs-Blog/Hiding-Webshell-Backdoor-Code-in-Image-Files/

[2] Paros : http://www.parosproxy.org/

[3] Burp Suite : https://portswigger.net/burp/

[4] Pangolin : http://www.toolswatch.org/2011/04/pangolin-automated-sql-injection-test-tool-free-edition-v3-2-3-released/

[5] Cooxie Toolbar : http://download.cnet.com/Cooxie-Toolbar-for-Microsoft-Internet-Explorer/3000-2144_4-10268044.html

[6] Zap : https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project

[7] Apache Commons Collection Vulnerability http://www.kb.cert.org/vuls/id/576313

InvokeTransformer

[8] OWASP Enterprise Security API : https://www.owasp.org/index.php/Category:OWASP_Enterprise_Security_API 닷넷도 있음

[9] exploit-db.com : 공격코드

[10] SpringFramework Request Flow 

![](http://i.stack.imgur.com/IjOIC.jpg)

[11] BackTrack https://en.wikipedia.org/wiki/BackTrack

