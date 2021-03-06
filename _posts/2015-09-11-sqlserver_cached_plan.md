---
layout: post
title: SQL Server Cached Plan
excerpt: SQL Server의 Execution Plan Cache를 분석해보는 글. 전체 용량과 타입별 용량 1회성 Plan의 용량을 확인해볼 수 있다.
modified:
categories: db
tags: [sqlserver]
author: mango
share: true
comments: true  
---

## SQL Server Execution Plan Cache란?

쿼리, 프로시저, Prepared Statement의 플랜을 저장해놓은 저장소

## 전체 Cached Plan 용량

{% highlight SQL %}

select sum(convert(bigint, size_in_bytes))/ 1048576 [size_in_MB]  from sys.dm_exec_cached_plans

{% endhighlight %}

## TYPE별 Cached Plan 용량

{% highlight SQL %}

SELECT objtype, count(*) num_of_plans, sum(convert(numeric, size_in_bytes)) / 1048576 [size_in_MB]
FROM (
     SELECT objtype, query_hash, query_plan_hash, text,size_in_bytes FROM sys.dm_exec_cached_plans AS cp
     JOIN sys.dm_exec_query_stats AS qs ON cp.plan_handle = qs.plan_handle
     CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS t
) T
GROUP BY objtype

{% endhighlight %}

## TYPE별 쿼리별 Cached Plan 개수 및 용량

{% highlight SQL %}

SELECT objtype,query_hash, query_plan_hash, COUNT(*), MAX(text), sum(convert(numeric, size_in_bytes)) / 1048576 [size_in_MB]
FROM (
     SELECT objtype,query_hash, query_plan_hash, text,size_in_bytes FROM sys.dm_exec_cached_plans AS cp
     JOIN sys.dm_exec_query_stats AS qs ON cp.plan_handle = qs.plan_handle
     CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS t
) T
GROUP BY objtype,query_hash, query_plan_hash
ORDER BY COUNT(*) DESC

{% endhighlight %}

## Reference

* [Plan cache and optimizing for adhoc workloads](http://www.sqlskills.com/blogs/kimberly/plan-cache-and-optimizing-for-adhoc-workloads/)
* [Improve SQL Server Performance by Looking at Plan Cache (Part 1)](http://logicalread.solarwinds.com/sql-server-minimize-single-use-plans-tl01/#.VfIgcxGqpBc)
* [Improve SQL Server Performance by Looking at Plan Cache (Part 2)](http://logicalread.solarwinds.com/sql-server-identifying-plans-that-need-tuning-tl01/#.VfJMrRGqpBc)
* [Improve SQL Server Performance by Looking at Plan Cache (Part 3)](http://logicalread.solarwinds.com/sql-server-identify-similar-plans-tl01/#.VfJM5BGqpBc)
