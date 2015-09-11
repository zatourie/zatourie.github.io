---
layout: post
title: SQL Server Cached Plan
excerpt: SQL Server의 Execution Plan Cache 관련 쿼리
modified:
categories: db
tags: [sqlserver]
author: mango
share: true
comments: true  
---

{% highlight SQL linenos=table %}

/* 전체 Cached Plan 용량 */
select sum(convert(bigint, size_in_bytes))/ 1048576 [size_in_MB]  from sys.dm_exec_cached_plans

/* TYPE별 용량 Cached Plan */
SELECT objtype, count(*) num_of_plans, sum(convert(bigint, size_in_bytes)) / 1048576 [size_in_MB]
FROM (
     SELECT objtype, query_hash, query_plan_hash, text,size_in_bytes FROM sys.dm_exec_cached_plans AS cp
     JOIN sys.dm_exec_query_stats AS qs ON cp.plan_handle = qs.plan_handle
     CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS t
     --WHERE objtype = 'Prepared'

) T
GROUP BY objtype


/* TYPE별 쿼리별 Cached Plan 개수 및 용량*/
SELECT objtype,query_hash, query_plan_hash, COUNT(*), MAX(text), sum(convert(bigint, size_in_bytes)) / 1048576 [size_in_MB]
FROM (
     SELECT objtype,query_hash, query_plan_hash, text,size_in_bytes FROM sys.dm_exec_cached_plans AS cp
     JOIN sys.dm_exec_query_stats AS qs ON cp.plan_handle = qs.plan_handle
     CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS t
     --WHERE objtype = 'Prepared'

) T
GROUP BY objtype,query_hash, query_plan_hash
ORDER BY COUNT(*) DESC
{% endhighlight %}

----

## Reference

*
