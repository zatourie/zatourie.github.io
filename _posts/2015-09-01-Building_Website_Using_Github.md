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
<section id="table-of-contents" class="toc">
  <header>
    <h3>Overview</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

## 1.Github란? <http://www.github.com>

* Web으로 Git repository를 서비스하는 싸이트
* 단순히 repo를 제공하는데 그치지 않고 해당 repo를 이용해서 웹싸이트를 만들과 관리할 수 있는 Github Pages 서비스도 제공하고 있습니다.<https://pages.github.com/>


## 2.Github Page Basic

![](http://comoyo.github.io/assets/img/posts/comoyo_jekyll_blog.png)

간단하게 설명하면 Github repo를 특정 형태로 세팅하고 파일을 생성 혹은 commit 하면 웹싸이트에 게재(publish)됩니다.

## 3.만들어보자

백문이 불여일타. 만들어봅시다.

Github Pages는 User or organization site와 Project site의 두가지 서비스를 제공합니다.

* User or organization은 계정당 하나씩 만들 수 있고 url이 user_name.github.io 의 형태가 됩니다.
* Project site는 계정에 관계없이 여러개 생성이 가능하고 url은 다음과 같은 형태가 됩니다. user_name.github.io/repo_name
* User 싸이트가 더 쉬우므로 이 글에서는 User site로 진행하도록 하겠습니다.

1) Github 가입합니다.

2) user_name.github.io 라는 이름으로 repository 생성합니다. user_name 부분은 실제 계정이름으로 치환해서 생성해야합니다.

![계정.github.io의 이름으로 repo 생성](https://guides.github.com/features/pages/create-new-repo-screen.png)

3) index.html 파일을 다음과 같이 생성합니다.

{% highlight html linenos %}
<html>
<head><title>Hello Github</title></head>
<body>
<h1>Hello Github pages!</h1>
</body>
</html>
{% endhighlight %}

4) http://user_name.github.io로 접속하면

짜잔

화면에 덩그러니 Hello Github pages!가 보이실겁니다. 뭔가 한것 같은데, 허전하죠. 이걸로 뭘할 수 있을까.

여기서 제길 아니 Jekyll이 등장합니다.


## 4.Jekyll?

<figure>
	<img src="http://images.moviepostershop.com/jekyll-movie-poster-2007-1020439848.jpg"></a>
	<figcaption>요런 지킬박사?가 아니고</figcaption>
</figure>

<figure>
	<img src="http://jekyllrb.com/img/logo-2x.png"></a>
	<figcaption>요겁니다.</figcaption>
</figure>

지킬 [공식홈페이지]<http://jekyllrb.com/> 설명에 따르면

>Transform your plain text into static websites and blogs.

랍니다. 다시 말해서, plain text파일을 html로 변환시켜주는 툴입니다. 루비로 만들어져있구요. 로컬에 루비를 설치하고 jekyll을 세팅하면 markdown으로 코딩된 파일을 html로 변환해줍니다.
로컬에서 실행하려면 ruby가 필요하지만, github pages에서 제공하는 서비스가 바로 이 지킬입니다.

우리가 해야할 일은 한가지. Markdown 형태의 포스팅을 작성하고 commit 하기만 하면 됩니다. Github가 jekyll을 대신 돌려서 static HTML을 생성해서 올리고 서비스해줍니다.

## 5.Github를 사용하는 두가지 방법 : CLI or Github for Desktop

### 5.1.Command Line Interface (CLI)

<div markdown="0"><a href="https://git-scm.com/download/win" class="btn btn-info">Git for Windows</a></div>

1) `Don't invent the wheels` 이라고, 이미 존재하는 보일러플레이트 클로닝(cloning)해서 진행합니다.

{% highlight bash %}
git clone https://github.com/necolas/jekyll-boilerplate.git heartonbit.github.io
{% endhighlight %}


2) repository 연결을 확인해봅니다.

{% highlight bash %}
git remote -v
{% endhighlight %}

3) 보일러플레이트 repo에다가 작업할 것이 아니니 삭제

{% highlight bash %}
git remote rm origin
{% endhighlight %}

4) 작업할 repo로 연결합니다.

{% highlight bash %}
git remote add origin https://github.com/heartonbit/heartonbit.github.io.git
{% endhighlight %}

5) 제대로 연결되었는지 확인해봅니다.

{% highlight bash %}
git remote -v
{% endhighlight %}

6) 내 Github repo로 push합니다. 계정과 비번을 물어보는데 그대로 입력해줍니다.

{% highlight bash %}
git push origin master
{% endhighlight %}

여기까지만 작업해도 http://heartonbit.github.io 내용은 보실 수 있습니다. 테스트로 포스팅을 추가해보겠습니다.
{: .notice}

7) \_post 폴더에 2015-09-02-Test_Page.md 화일을 다음과 같은 코드로 만듭니다.

{% highlight html linenos %}
{% raw %}
---
layout: post
title: Test Page
---

**{{ page.title }}** written in markdown.
{% endraw %}
{% endhighlight %}

8) repo 파일 상태를 확인하고

{% highlight bash %}
git status
{% endhighlight %}

9) 추가된 변경사항을 add 해줍니다.

{% highlight bash %}
git add --all
{% endhighlight %}

10) 변경된 사항을 commit 합니다.

{% highlight bash %}
git commit -m"Commit message"
{% endhighlight %}

11) 변경된 사항 push, github 계정과 비번을 입력해주면 push해줍니다.

{% highlight bash %}
git push origin master
{% endhighlight %}

12) http://heartonbit.github.io 접속해보면 포스팅이 늘어있는 것을 확인할 수 있습니다.

<http://heartonbit.github.io/>

CLI로 작업하는데도 무리는 없지만 Github에서는 Github Desktop이라는 설치용 소프트웨어도 제공하고 있습니다. 다음에는 Github Desktop을 사용하여 진행해보겠습니다.

### 5.2.Github Desktop

<div markdown="0"><a href="https://desktop.github.com/" class="btn btn-info">Github Desktop</a></div>

내용 작성예정



----
# Reference

* Webcast: Getting Started with GitHub Pages <https://www.youtube.com/watch?v=WDyRhiG_BHM>
* Jekyll Boilerplate<https://github.com/necolas/jekyll-boilerplate>
* Github Flavoured Markdown <https://help.github.com/articles/github-flavored-markdown/>
* Github Pages, Dependency versions <https://pages.github.com/versions/>
