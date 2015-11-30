---
layout: post
title: Secure Coding 교육
excerpt: 2015.11.24 ~ 27 Secure Coding 교육 정리노트
date:   2015-11-24 09:00:00
categories:
tags: [securecoding]
author: mango
share: true
comments: true  
---

> NOTE: 너무 길어서 좀 나눌까 생각중입니다. 귀찮으면 그냥 둘 예정; -민규


# 0. OVERVIEW

## 0.1. What is vulnerability

> In computer security, a vulnerability is a weakness which allows an attacker to reduce a system's information assurance.
> from https://en.wikipedia.org/wiki/Vulnerability_(computing)

In short, a weakness is in theory, a vulnerability is in practice.


## 0.2. Guidelines

* [행정안전부 시큐어코딩 가이드 (Secure Coding Guide)](http://www.mogaha.go.kr/frt/bbs/type001/commonSelectBoardArticle.do;jsessionid=IcuGyiF19H0HFQ7aauptfaJZhfaMFdTeEKXebtlcmwyhxIgc6BbdXtT4bRmkykmc.mopwas52_servlet_engine1?bbsId=BBSMSTR_000000000045&nttId=34430)
* [CWE (Mitre's Common Weakness Enumeration) Top 25](http://cwe.mitre.org/top25/) cf)CVE(Common Vulnerability Exposure)
* [OWASP Top 10 vulnerabilities](https://www.owasp.org/index.php/Top_10_2013-Top_10)
* [CERT (Secure Coding Standard)](https://www.securecoding.cert.org/confluence/display/seccode/SEI+CERT+Coding+Standards)

## 0.4. Mitigate Vulnerability Strategy

* Network Layer : Traffic, Packet monitoring
* App Op Layer : Request, pattern monitoring
* App Dev Layer : Secure Coding (What we are dealing with)

## 0.5. [Microsoft Security Development Lifecycle:MSSDL](https://www.microsoft.com/en-us/sdl/)

![](http://i.msdn.microsoft.com/cc448177.SDL-Lifecycle-gradient_0609(en-us,MSDN.10).jpg)

![MS-SDL openeg](http://cfile22.uf.tistory.com/R750x0/227AA74355630C1A13AFA8)

### MS SDL: DREAD

![](http://image.slidesharecdn.com/praetorianthreatmodelingpresentation-110117095334-phpapp02/95/threat-modeling-improve-security-drive-testing-reduce-costs-25-728.jpg?cb=1332160190) 

### MS SDL: STRIDE 

![](http://image.slidesharecdn.com/praetorianthreatmodelingpresentation-110117095334-phpapp02/95/threat-modeling-improve-security-drive-testing-reduce-costs-20-728.jpg?cb=1332160190)

### Definition

![](https://i-technet.sec.s-msft.com/en-us/security/hh855044.STRIDE-definitions(en-us,MSDN.10).jpg)

### Mitigation

![](https://i-technet.sec.s-msft.com/en-us/security/hh855044.STRIDE-mitigation-categories(en-us,MSDN.10).jpg)

## 0.6. 보안관련 Encoding

* URL Encoding : URL에 사용할 수 있는 형태로 encoding
* HTML Encoding : 브라우저 렌더링 escape
* [SO Difference between Url Encode and HTML encode](http://stackoverflow.com/questions/1812473/difference-between-url-encode-and-html-encode)

## 0.7. KISA SW보안약점 카테고리 7개

1. 입력데이터 검증 및 표현 : SQL Injection, XSS
2. 보안기능 : Weak Cryptographic Algorithm
3. 시간 및 상태 : TOCTOU
4. 에러처리 
5. 코드오류
6. 캡슐화
7. API오용

하나의 Attack은 위 7개 카테고리를 복합적으로 사용할 수 있음.

## 0.8 보안관련툴

* Proxy : Paros [1] , Fiddler [2] , Charles [3] , Burp Suite [4]
* SQL Injection : Pangolin [5]
* Toolbar for IE : Cooxie Toolbar [6]
* Integrated penetration testing tool : ZAP [7]
* Backtrack [11]

# 1. 입력데이터 검증 및 표현

## 1.1. SQL Injection

### MySQL Injection Example

{% highlight sql %}
' union select 1,2,3,4,5,6 #
' union select schema_name,2,3,4,5,6 from information_schema.schemata#
' union select table_name,2,3,4,5,6 from information_schema.tables where table_schema = database()#
' union select group_concat(column_name),2,3,4,5,6 from information_schema.columns where table_name = '테이블명'#
{% endhighlight %}

pangoline [4] 같은 툴을 사용하면 일일이 손으로 공격값을 넣을 필요없음

### How to mitigate SQL Injection

* Use static query 
* Use PreparedStatement properly when using JDBC API
* Use # in My/IBatis query
* In case of dynamic query, use a proper filter to remove/replace special characters which enable attacks

## 1.2. Access Control 경로조작 및 자원 삽입

### 주어진 권한을 넘어서는 리소스 탈취/수정

### How to mitigate Access Control Attack

* Application에게 과도한 권한을 주지 말 것: root, administrator 사용금지
* Application내에서 whitelist, input string validation 등 수행

##  1.3. Cross Site Script (XSS)

클라이언트 대상 해킹기법으로 악성코드를 클라이언트에서 실행시켜 원하는 데이터를 탈취함.

### Type (regarding where malicious script originated)

#### Stored XSS
![](http://excess-xss.com/persistent-xss.png)

#### Reflected XSS
![](http://excess-xss.com/reflected-xss.png)

#### DOM XSS
![](http://excess-xss.com/dom-based-xss.png)

### How to mitigate XSS

* Use JSTL

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

* Use 3rt party library such as Lucy XssPreventer/XssFilter : https://github.com/naver/lucy-xss-filter

{% highlight java%}
String dirty = "\"><script>alert('xss');</script>";
String clean = XssPreventer.escape(dirty); //&quot;&gt;&lt;script&gt;alert(&#39xss&#39);&lt;/script&gt;

String dirty = "<img src=\"<img src=1\\ onerror=alert(1234)>\" onerror=\"alert('XSS')\">";
String clean = filter.doFilter(dirty); //<img src=\"\"><!-- Not Allowed Attribute Filtered ( onerror=alert(1234)) --><img src=1\\>\" onerror=\"alert('XSS')\"&gt;
{% endhighlight %}

* Spring Framework인 경우, Filter 사용 : http://www.openeg.co.kr/383

Examples above is too basic. Consult link below for smarter attacks 

OWASP Cheat Sheet : https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet

And here's a first hand experience of ["Mass SQL Injection"](http://using.tistory.com/11)

## 1.4. Malicious File Upload

* 웹에서 직접 접근이 불가능한 위치에 파일 업로드 할 것
* 사용자가 올린 파일명과 저장된 물리적 파일명을 다르게 할 것 (둘 사이의 연결 정보 관리필요)
* 실행가능한 파일 업로드 불가
* 파일을 업로드하면서 weakness를 모두 없애는 것은 쉽지 않다

## 1.5. 신뢰되지 않는 URL주소로 Redirect

* 피싱에 쓰이는 기법, 사용자가 인지하지 못하도록 공격싸이트로 이동시켜서 정보 탈취

### How to mitigate

* URL주소를 whitelist로 관리할 것
* 적절하게 encode 할 것

## 1.6. XPath Injection

* SQL Injection과 유사.
* XML문서를 검색하는 입력값을 이용하여 공격
* 방어방법은 XQuery 클래스를 사용 (PreparedStatement와 유사)

## 1.7. LDAP Injection

* 검색어);추가정보; : 검색어 정보와 추가정보가 함께 조회됨
* 검증되지 않은 입력값 검증 필요 

## 1.8. Cross Site Request Forgery (CSRF)

![](http://www.opensourceforu.efytimes.com/wp-content/uploads/2010/11/Figure-1-CSRF-attack-on-GET.png)

사용자로 하여금 공격자가 의도한 요청을 서버로 보내 처리하도록 하는 공격
누군가 보내준 링크를 클릭했더니 페이스북에 광고글(썬글래스)이 게시된다든지 하는 시나리오 (내가 안올렸는데)

일베의 적 일간워스트 개발자가 싸이트를 리뉴얼하는 동안 일워싸이트를 페북글쓰기로 리다이렉션을 걸어놓았는데, 일베사용자들이 페이스북에 로그인된 상태에서 악성댓글을 달다가 일베인증을 한 적이 있다. 바람직한 CSRF예

### How to mitigate CSRF

* 정적 소스코드 진단으로는 검출되지 않으며, 동적진단을 통해 검출해야한다
* Verify requests even from authorized users : CSRF TOKEN으로 실제 시나리오상의 요청인지 검증
* 사용자에게 실제 요청의 의도를 다시 확인한다 : 홍길동에게 100만원을 이체하시겠습니까?
* USE captcha : https://www.google.com/recaptcha
* discussion about captcha : https://gist.github.com/homakov/5607607


## 1.9. HTTP 응답 분할 (HTTP Response Splitting)

* OWASP : https://www.owasp.org/index.php/HTTP_Response_Splitting
* JDK 1.6 이상에서는 해결되었다? 확인필요
* MITRE 참조 : https://cwe.mitre.org/data/definitions/113.html

## 1.10. 정수형 오버플로우 Integer overflow

* OWASP : https://www.owasp.org/index.php/Integer_overflow
* Java는 Integer overflow가 발생하지 않는다. 연산오류가 있을 뿐. http://www.mkyong.com/java/javas-silent-killer-buffer-overflow-careful/

### Android Stagefright Vulnerability

* Blog : https://blog.zimperium.com/the-latest-on-stagefright-cve-2015-1538-exploit-is-now-available-for-testing-purposes/
* Demo : https://www.youtube.com/watch?v=PxQc5gOHnKs
* CVE-2015-1538
* http://fortune.com/2015/07/28/stagefright-google-android-security/

## 1.11. 메모리 버퍼 오버플로우 Memory buffer overflow

* http://www.openeg.co.kr/331
* Java와 C#처럼 managed code를 사용하는 언어는 관계없음

## 1.12. Format String 

* 포맷스트링 함수를 이용한 공격
* 이것도 Java와 C#처럼 managed code를 사용하는 언어는 관계없음

# 2. 보안기능

## 2.1. Cripytography

* http://seed.kisa.or.kr/iwt/ko/index.do
* 인터넷진흥원(KISA) [암호정책수립기준](http://seed.kisa.or.kr/iwt/ko/guide/EgovGuideDetail.do?bbsId=BBSMSTR_000000000011&nttId=30&pageIndex=1&searchCnd=&searchWrd=)

### [Java Cryptography Architecture(JCA)](https://en.wikipedia.org/wiki/Java_Cryptography_Architecture)

> The Java Cryptography Architecture (JCA) is a framework for working with cryptography using the Java programming language.

### [Java Cryptography Extension(JCE)](https://en.wikipedia.org/wiki/Java_Cryptography_Extension)

> The Java Cryptography Extension (JCE) is an officially released Standard Extension to the Java Platform. JCE provides a framework and implementation for encryption, key generation and key agreement, and Message Authentication Code (MAC) algorithms.

### [Bouncy Castle](http://bouncycastle.org/documentation.html)

* [재미나는 글](http://stackoverflow.com/questions/2927952/why-do-people-use-bouncycastle-instead-of-java-cryptography-extension-what-is-t)
* [Github](https://github.com/bcgit/bc-java)

## 2.2. 안전하지 않은 난수 Crpytographically insecure random number

### Javascript

* [Math.random() is not secure](http://stackoverflow.com/questions/5651789/is-math-random-cryptographically-secure)
* [Google crypto](https://code.google.com/p/crypto-js/)
* [MDN](https://developer.mozilla.org/en-US/docs/Web/API/RandomSource/getRandomValues)
* [Clipperz](https://github.com/clipperz/javascript-crypto-library)

### Java

* java.util.Random < java.security.SecureRandom
* [Difference between random and securerandom](http://stackoverflow.com/questions/11051205/difference-between-java-util-random-and-java-security-securerandom)

## 2.3. 쿠키관련 취약점 : [download](http://www.kisa.or.kr/uploadfile/201409/201409161535469432.pdf)

* [setHttpOnly](https://www.owasp.org/index.php/HttpOnly) 
* [setSecure](http://stackoverflow.com/questions/4578506/servlet-set-cookie-secure): SSL/TLS 통신을 강제함
* setPath 

## 2.4. Read from buffer 

[InputStream reading problem](http://stackoverflow.com/questions/6160432/java-inputstream-reading-problem)

아래와 같이 버퍼크기를 지정해서 가져오는 것을 권고

{% highlight java%}
byte[] buffer = new byte[BUFFER_SIZE];
    
int bytesRead = 0;
while ((bytesRead = in.read(buffer)) >= 0){
  for (int i = 0; i < bytesRead; i++){
     //Do whatever you need with the bytes here
  }
}
{% endhighlight %}

## 2.5. Brute Force Attack 막무가내 공격

* application 수준에서 BFA를 막기는 쉽지 않음. 
* 공격의 타겟이 될 수 있는 외부로 열려있는 기능은 throughput 제한을 두거나 세션별 최대 요청수 등을 관리할 수는 있음

### [Burp Suite](https://portswigger.net/burp/)

> Burp Suite is an integrated platform for performing security testing of web applications. Its various tools work seamlessly together to support the entire testing process, from initial mapping and analysis of an application's attack surface, through to finding and exploiting security vulnerabilities.

* [Demo](https://www.youtube.com/watch?v=vjb-MrUbvps)

## 2.6. Race Condition 

* [CWE-366](https://cwe.mitre.org/data/definitions/366.html)

### 경쟁조건 : 검사시점과 사용시점TOCOUT (Time of check, time of use)

* [TOCTOU](http://capec.mitre.org/data/definitions/29.html)

## 2.7. 에러처리

### Java Exception

    Throwable 
        Error -> system abort
        Exception 
            Checked Exception -> Compiler 
            *Runtime Exception* -> Problematic

### 오류메시지를 통한 정보 노출

    e.printStackTrace();

위 코드는 정적분석도구로 스캔하면 취약점으로 검색됨. 하지만, 웹시스템에서 위 코드는 콘솔창으로 오류스택을 내보내므로 오류메시지가 노출된다고 할 수 없음. 웹시스템인 경우 분석도구에서 예외처리 필요.

## 2.8. Null dereference, Null pointer 역참조...

> A null-pointer dereference takes place when a pointer with a value of NULL is used as though it pointed to a valid memory area.
> https://www.owasp.org/index.php/Null_Dereference

{% highlight c %}
int c1 = 5;
int* p1 = &c1; //p1 references c1 p1은 c1을 참조한다
int n1 = (*p1); //*p1 dereferences p1 to yield c1 *p1은 p1를 역참조한다 (뭔소리여 @_@)
{% endhighlight %}

The expression *p1 dereferences p1 to yield c1 as an lvalue

다시 말해서, *p1은 p1을 역참조한다는 말은 포인터인 p1이 참조하고 있는 c1의 값(여기서는 5)을 가지고 온다는 뜻. 
역참조라는 말은 개인적으로 안썼으면 좋겠다. 

그나저나, Java는 pointer가 없는데 왠 Null pointer 역참조? NullPointerException이랑 관련이 있는건가?
KISA에서 얘기하고 있는 Null pointer 역참조는 아래 내용. 
CERT에서는 역참조~~따위의~~와 같은 혼란스런 단어를 쓰지 않고 깔끔하게 "Do not use a null in a case where an object is required"로 정리했다. 

[Do not use a null in a case where an object is required](https://www.securecoding.cert.org/confluence/display/java/EXP01-J.+Do+not+use+a+null+in+a+case+where+an+object+is+required)

객체가 null이 아닐꺼라고 간주하지 말라. 

그렇다면, 모든 객체를 매번 null체크 하란 말? obj != null && 아 손꾸락 아퍼

## 2.9. Memory leak

### [Avoid memory leak and callback](http://stackoverflow.com/questions/8475314/avoid-memory-leaks-in-callbacks)

### tl;dr

* [Java Strong vs Weak Reference](http://stackoverflow.com/questions/9809074/java-difference-between-strong-soft-weak-phantom-reference)
* [Java Strong/Weak/Phantom](http://javaconceptoftheday.com/types-of-references-in-java-strong-soft-weak-and-phantom/)
* [Java Is fianlly expensive?](http://programmers.stackexchange.com/questions/210428/is-try-finally-expensive)

# 7. API 오용 (엉? 3,4,5,6 다 어디갔지?)

## 7.1. DNS lookup에 의존한 보안결정 

* DNS lookup/resolution : 도메인으로 IP를 찾는 것
* Reverse DNS lookup/resolution : IP로 도메인을 찾는 것. 공격자에 의해 hostname이 변조될 수 있음
* [CWE-350: Reliance on Reverse DNS Resolution for a Security-Critical Action](https://cwe.mitre.org/data/definitions/350.html)

> When the software performs a reverse DNS resolution for an IP address, if an attacker controls the server for that IP address, then the attacker can cause the server to return an arbitrary hostname. As a result, the attacker may be able to bypass authentication, cause the wrong hostname to be recorded in log files to hide activities, or perform other attacks.

> Attackers can spoof DNS names by either (1) compromising a DNS server and modifying its records (sometimes called DNS cache poisoning), or (2) having legitimate control over a DNS server associated with their IP address.

{% highlight java %}
String ip = request.getRemoteAddr();
InetAddress addr = InetAddress.getByName(ip);
if (addr.getCanonicalHostName().endsWith("trustme.com")) {
  trusted = true;
}
{% endhighlight %}

InetAddress의 getCannonicalHostName을 authentication용으로 사용하지 말것.

## 7.2. 취약한 API 사용

* OWASP [Using Components with Known Vulnerabilities](https://www.owasp.org/index.php/Top_10_2013-A9-Using_Components_with_Known_Vulnerabilities)

# 8. 정적분석툴 Static Source Code Analysis Tools

* OWASP [Source Code Analysis Tools](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)
* [Static code analysis tool](http://programmers.stackexchange.com/questions/209483/static-code-analysis-application-where-to-start)

## Yasca

![Yasca Screenshot](http://3.bp.blogspot.com/-u6egMoAGC50/UKNuTvWhRSI/AAAAAAAAAIo/kYt2Z-LJWmM/s1600/yasca1.png)

[Link](http://www.scovetta.com/yasca.html)

## Findbugs

![Findbugs Screenshot](http://beyondj2ee.pbworks.com/f/findbugs_perspective_1.png)

[LINK](http://findbugs.sourceforge.net/)

## SK Nextcore Code Inspector

## Fasoo sparrow

![Sparrow screenshot](http://image2.aving.net/2009/07/03/20090703172240690.jpg)

# 9. Conclusion (IMHO)

> *DO NOT BELIEVE ANYONE*

> *CONSIDER USER AS ATTACKER*

> *INPUT VALUES ARE EVIL*

* 개발자 관점에서 secure coding은 정도의 문제. 
* "보안"과 "가독성/성능" 의 trade-off 관계에서 적절한 선을 긋는 것이 중요.
* 적절한 선이 어디인지가 전쟁터.

----

# 9. miscellaneous

KISA Secure Coding 진단원 과정이 있음

## 인증관련 

* [OAuth 2.0](http://oauth.net/2/)
* [RFC6749](https://tools.ietf.org/html/rfc6749)
![flow](http://a5.files.readwrite.com/image/upload/MTIyNDQwNDkxNzM1MjIwODM4.gif)

## Mitre corporation

* [Homepage](http://www.mitre.org/)
* [Wikipedia]https://en.wikipedia.org/wiki/Mitre_Corporation
* [Meaning and proper Pronounciation]http://www.oxforddictionaries.com/definition/english/mitre 

## Qualys SSL Test

* [Link](https://www.ssllabs.com/ssltest/analyze.html)
* [Documentation](https://www.ssllabs.com/projects/documentation/index.html)

## Wehshell : 웹상에서 서버 CLI를 사용하도록 해주는 툴

* http://darksoulstory.tistory.com/324 
* https://www.trustwave.com/Resources/SpiderLabs-Blog/Hiding-Webshell-Backdoor-Code-in-Image-Files/

## SpringFramework Request Flow 

![](http://i.stack.imgur.com/IjOIC.jpg)

## HSQLDB: http://hsqldb.org/

* HSQLDB (HyperSQL DataBase) is the leading SQL relational database software written in Java. It offers a small, fast multithreaded and transactional database engine with in-memory and disk-based tables and supports embedded and server modes. It includes a powerful command line SQL tool and simple GUI query tools.
* Benchmark: http://hsqldb.org/PolePosition.pdf

## OWASP Webgoat Project

* WebGoat is a deliberately insecure web application maintained by OWASP designed to teach web application security lessons. You can install and practice with WebGoat in either J2EE (this page) or WebGoat for .Net in ASP.NET. In each lesson, users must demonstrate their understanding of a security issue by exploiting a real vulnerability in the WebGoat applications. For example, in one of the lessons the user must use SQL injection to steal fake credit card numbers. The application is a realistic teaching environment, providing users with hints and code to further explain the lesson.
* https://www.owasp.org/index.php/Category:OWASP_WebGoat_Project

## CC(Common Criteria)인증이란? 

[Link](http://www.kisa.or.kr/uploadfile/201204/201204061424023641.pdf)

## [정보보호시스템 평가인증 지침](http://www.미래창조과학부.com/www/brd/m_215/down.do?brd_id=w_g0402&seq=66&data_tp=A&file_seq=2)

## [KISA 공개용 소스코드 보안약점 분석도구 개발 동향 PDF](http://www.kisa.or.kr/uploadfile/201406/201406160957543174.pdf)


----

# 10. Footnotes

[1]: http://www.parosproxy.org/          "Paros"
[2]: http://www.telerik.com/fiddler       "Fiddler"
[3]: https://www.charlesproxy.com/      "Charles"
[4]: https://portswigger.net/burp/       "Burp Suite"
[5]: http://www.toolswatch.org/2011/04/pangolin-automated-sql-injection-test-tool-free-edition-v3-2-3-released/ "Pangolin"
[6]: http://download.cnet.com/Cooxie-Toolbar-for-Microsoft-Internet-Explorer/3000-2144_4-10268044.html "Cooxie Toolbar"
[7]: https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project  "Zap"
[8]: http://www.kb.cert.org/vuls/id/576313 "Apache Commons Collection Vulnerability"
[9]: https://www.owasp.org/index.php/Category:OWASP_Enterprise_Security_API  "OWASP Enterprise Security API"
[10]: http://exploit-db.com "공격코드를 공유하는 싸이트"
[11]: https://en.wikipedia.org/wiki/BackTrack "BackTrack"
[12]: http://regexper.com/ "REGEXPER"
[13]: https://blog.kaspersky.com/password-check/ "Kaspersky Password Strength Checker"

----

아래는 자바 8 관련인데 귀찮아서 여기 남김. 나중에 따로 포스팅 나누겠습니다.

* https://github.com/winterbe/java8-tutorial
* http://winterbe.com/java/
