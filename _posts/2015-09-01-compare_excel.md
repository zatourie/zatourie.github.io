---
layout: post
title: Diff excel
date: 2015-09-01 12:00:00 
categories:
excerpt: 엑셀파일을 비교할 수 있는 툴
tags: [excel]
image:
  feature:
author: mango
share: true
comments: true  
---
자바로 만들어져있으며 CLI 실행 가능

<https://github.com/na-ka-na/ExcelCompare>

{% highlight bash %}
excel_cmp xxx.xlsx yyy.xlsx --ignore1 Sheet1 --ignore2 Sheet1
EXTRA Cell in WB1 Sheet2!A1 => 'abc'
EXTRA Cell in WB2 Sheet3!A1 => 'haha'
----------------- DIFF -------------------
Sheets: []
Rows: []
Cols: []
----------------- EXTRA WB1 -------------------
Sheets: [Sheet2]
Rows: [1]
Cols: [A]
----------------- EXTRA WB2 -------------------
Sheets: [Sheet3]
Rows: [1]
Cols: [A]
-----------------------------------------
Excel files xxx.xlsx and yyy.xlsx differ
{% endhighlight %}
