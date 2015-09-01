---
layout: post
title: Github를 이용한 웹싸이트 만들기
modified: 
categories: 
excerpt: Github로 웹싸이트 만들기 및 관리하기 
tags: [github]
image: 
  feature: 
author: mango
share: true
comments: true  
---

# Github란? <http://www.github.com>

* web으로 git repository를 서비스하는 싸이트
* 단순히 repo를 제공하는데 그치지 않고 해당 repo를 이용해서 웹싸이트를 만들과 관리할 수 있는 Github Pages 서비스를 하고 있음.<https://pages.github.com/>


# Github Page Basic 

![](http://comoyo.github.io/assets/img/posts/comoyo_jekyll_blog.png)


간단하게 설명하면 Github repo를 특정 형태로 세팅하고 파일을 생성 혹은 commit 하면 웹싸이트에 게재됨.


# 만들어보자

백문이 불여일타. 만들어봅시다.
Github Pages는 User or organization site와 Project site의 두가지 서비스를 제공한다. 

* User or organization은 계정당 하나씩 만들 수 있고 url이 user_name.github.io 의 형태가 된다.
* Project site는 계정에 관계없이 여러개 생성이 가능하고 url은 다음과 같은 형태가 된다. user_name.github.io/repo_name
* User 싸이트가 더 쉬우므로 이 글에서는 User site로 진행함


1. Github에 계정생성

2. user_name.github.io 라는 이름으로 repository 생성. user_name 부분은 실제 계정이름으로 치환

![계정.github.io의 이름으로 repo 생성](https://guides.github.com/features/pages/create-new-repo-screen.png)

3. index.html을 만들어보자. 

{% highlight html %}
<html>
<head><title>Hello Github</title></head>
<body>
<h1>Hello Github pages!</h1>
</body>
</html>
{% endhighlight %}

4. http://user_name.github.io로 접속

짜잔~

화면에 덩그러니 Hello Github pages!가 보이실겁니다. 
뭔가 한것 같은데, 허전하죠. 이걸로 뭘할 수 있을까. 
여기서 제길 아니 Jekyll이 등장합니다. 

# Jekyll?

<figure>
	<img style="width:100px;" src="https://upload.wikimedia.org/wikipedia/commons/7/78/Dr_Jekyll_and_Mr_Hyde_poster_edit2.jpg"></a>
	<figcaption>지킬박사와 하이드?</figcaption>
</figure>

요게 아니고.

<figure>
	<img style="width:100px;" src="http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/02/1424055625jekyll.png"></a>
	<figcaption>요겁니다.</figcaption>
</figure>

지킬 [공식홈페이지]<http://jekyllrb.com/> 설명에 따르면 

>Transform your plain text into static websites and blogs.

랍니다. 다시 말해서, plain text파일을 html로 변환시켜주는 툴입니다. 루비로 만들어져있구요. 로컬에 루비를 설치하고 jekyll을 세팅하면 markdown으로 코딩된 파일을 html로 변환해줍니다.

로컬에서 실행하려면 ruby가 필요하지만, github pages에서 제공하는 서비스가 바로 이 지킬입니다. 

![](http://comoyo.github.io/assets/img/posts/comoyo_jekyll_blog.png)

----
Reference

https://www.youtube.com/watch?v=WDyRhiG_BHM


