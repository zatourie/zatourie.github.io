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

# Refs

* 행자부 (Secure Coding Guide)
* CWE (Mitre's Common Weakness Enumeration) cf)CVE(Common Vulnerability Exposure)
* OWASP
* CERT (Secure Coding Standard)

# Minimum Requirement

* CWE SANS Top 25
* OWASP Top 10

# What is vulneribility

> In computer security, a vulnerability is a weakness which allows an attacker to reduce a system's information assurance.

https://en.wikipedia.org/wiki/Vulnerability_(computing)

# Strange user behaviour monitoring, 이상징후 모니터링

* Network Layer : Traffic, Packet monitoring
* Application Layer : Request, pattern monitoring

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


![MS-SDL openeg](http://cfile22.uf.tistory.com/R750x0/227AA74355630C1A13AFA8)
----

# Footnotes

[1] Wehshell : http://darksoulstory.tistory.com/324 콩글리시 인듯 외국자료는 거의 나오지 않음

[2] Paros : http://www.parosproxy.org/
