---
layout: post
title:  "MS SQL Server SET Command"
date:   2013-06-05 17:23:32
categories: DB
---
# MS SQL Server SET Command
발제 : 유재섭
일시 : 2015.7.22 (수) 11:00~12:00

## SET 명령리스트

| 문 |                                     |
|:----|:------------------------------------|
|날짜 및 시간 문 | SET DATEFIRST                       |
|     |SET DATEFORMAT                          |
| 잠금 문 | SET DEADLOCK\_PRIORITY              |
|     | SET LOCK\_TIMEOUT                       |
| 기타 문 | SET CONCAT\_NULL\_YIELDS\_NULL      |
|     | SET CURSOR\_CLOSE\_ON\_COMMIT             |
|     | SET FIPS\_FLAGGER                       |
|     | SET IDENTITY\_INSERT                    |
|     | SET LANGUAGE                           |
|     | SET OFFSETS                            |
|     | SET QUOTED\_IDENTIFIER                  |
| 쿼리 실행 문 | SET ARITHABORT                      |
|     | SET ARITHIGNORE                        |
|     | SET FMTONLY                            |
|     | SET NOCOUNT                            |
|     | SET NOEXEC                             |
|     | SET NUMERIC\_ROUNDABORT                 |
|     | SET PARSEONLY                          |
|     | SET QUERY\_GOVERNOR\_COST\_LIMIT          |
|     | SET ROWCOUNT                           |
|     | SET TEXTSIZE                           |
| ISO 설정 문 | SET ANSI\_DEFAULTS                  |
|     | SET ANSI\_NULL\_DFLT\_OFF                 |
|     | SET ANSI\_NULL\_DFLT\_ON                  |
|     | SET ANSI\_NULLS                         |
|     | SET ANSI\_PADDING                       |
|     | SET ANSI\_WARNINGS                      |
| 통계 문 | SET FORCEPLAN                       |
|     | SET SHOWPLAN\_ALL                       |
|     | SET SHOWPLAN\_TEXT                      |
|     | SET SHOWPLAN\_XML                       |
|     | SET STATISTICS IO                      |
|     | SET STATISTICS XML                     |
|     | SET STATISTICS PROFILE                 |
|     | SET STATISTICS TIME                    |
| 트랜잭션 문 | SET IMPLICIT\_TRANSACTIONS          |
|     | SET REMOTE\_PROC\_TRANSACTIONS           |
|     | SET TRANSACTION ISOLATION LEVEL        |
|     | SET XACT\_ABORT                         |


## SET 문 사용 시 고려 사항

  * SET FIPS\_FLAGGER, SET OFFSETS, SET PARSEONLY 및 SET QUOTED\_IDENTIFIER를 제외한 모든 SET 문은 실행 시간 또는 런타임에 구현됩니다. 이러한 문은 구문 분석 시 구현됩니다.
  * 저장 프로시저에 또는 트리거에서 SET 문을 실행하면 저장 프로시저 또는 트리거에서 컨트롤이 반환된 후 SET 옵션 값이 복원됩니다. 또한 sp\_executesql 또는 EXECUTE를 사용하여 실행되는 동적 SQL 문자열에서 SET 문을 지정하면 동적 SQL 문자열에 지정된 일괄 처리에서 컨트롤이 반환된 후 SET 옵션 값이 복원됩니다.
  * 저장 프로시저는 SET ANSI\_NULLS와 SET QUOTED\_IDENTIFIER를 제외한 실행 시간에 지정된 SET 설정으로 실행됩니다. SET ANSI\_NULLS나 SET QUOTED\_IDENTIFIER를 지정하는 저장 프로시저는 저장 프로시저를 만든 시간에 지정된 설정을 사용합니다. SET 설정을 저장 프로시저 내에서 사용하면 모두 무시됩니다.
  * sp\_configure의 user options 설정은 서버 측 설정을 허용하고 여러 데이터베이스에 걸쳐 작동합니다. 또한 이 설정은 로그인 시간에 발생할 경우를 제외하고 명시적 SET 문과 같이 작동합니다.
  * ALTER DATABASE를 사용하여 지정한 데이터베이스 설정은 데이터베이스 수준에서만 유효하며 명시적으로 설정된 경우에만 적용됩니다. 데이터베이스 설정은 sp\_configure를 사용하여 지정한 인스턴스 옵션 설정보다 우선 적용됩니다.
  * SET 문 중 하나라도 ON이나 OFF로 설정하면 여러 SET 옵션을 ON 또는 OFF로 설정할 수 있습니다.

```
참고
통계 관련 SET 옵션에는 적용되지 않습니다.
```

  * 예를 들어 SET QUOTED\_IDENTIFIER, ANSI\_NULLS ON 은 QUOTED\_IDENTIFIER 및 ANSI\_NULLS를 ON으로 설정합니다.
  * SET 문 설정은 ALTER DATABASE를 사용하여 설정한 해당 데이터베이스 옵션 설정보다 우선 적용됩니다. 예를 들어 SET ANSI\_NULLS 문에서 지정한 값은 ANSI\_NULL에 대한 데이터베이스 설정보다 우선 적용됩니다. 뿐만 아니라 데이터베이스에 연결할 때 사용자가 이전에 sp\_configure user options 설정을 사용할 때 적용된 값 또는 모든 ODBC 및 OLE/DB 연결에 적용되는 값에 따라 일부 연결 설정은 자동으로 ON으로 설정됩니다.
  * ALTER, CREATE 및 DROP DATABASE 문은 SET LOCK\_TIMEOUT 설정을 인식하지 못합니다.
  * 전역 또는 바로 가기 SET 문(예: SET ANSI\_DEFAULTS)은 여러 개의 설정을 설정해 바로 가기 SET 문이 이 문에 의해 영향을 받은 모든 옵션에 대해 이전 설정을 되돌리도록 합니다. 바로 가기 SET 문이 실행된 후 바로 가기 SET 문에 의해 영향을 받은 개별 SET 옵션을 명시적으로 설정하면 이 SET 문은 해당 바로 가기 설정보다 우선 적용됩니다.
  * 일괄 처리를 사용하면 USE 문을 사용해 만든 일괄 처리에 의해 데이터베이스 컨텍스트가 결정됩니다. 임의 쿼리 및 저장 프로시저 외부에서 실행되고 일괄 처리되는 다른 모든 문은 USE 문을 사용해 만든 연결 및 데이터베이스 옵션 설정을 따릅니다.
  * MARS(Multiple Active Result Set) 요청은 최신 세션 SET 옵션 설정이 포함된 전역 상태를 공유합니다. 각 요청을 실행하면 SET 옵션을 수정할 수 있습니다. 변경 내용은 이 옵션이 설정된 요청 컨텍스트에만 적용되며 다른 동시 MARS 요청에는 영향을 주지 않습니다. 그러나 요청 실행을 완료한 후 새 SET 옵션이 전역 세션 상태로 복사됩니다. 이 변경 작업 후 같은 세션에서 실행되는 새 요청은 이러한 새 SET 옵션 설정을 사용합니다.
  * 일괄 처리 또는 다른 저장 프로시저의 저장 프로시저를 실행하면 저장 프로시저가 포함된 데이터베이스에 현재 설정된 옵션 값에 따라 실행됩니다. 예를 들어 저장 프로시저 db1.dbo.sp1이 저장 프로시저 db2.dbo.sp2를 호출할 경우 저장 프로시저sp1은 데이터베이스 db1의 현재 호환성 수준 설정에서 실행되고 저장 프로시저 sp2는 데이터베이스 db2의 현재 호환성 수준 설정에서 실행됩니다.
  * Transact-SQL 문이 여러 데이터베이스에 있는 개체를 참조할 경우 현재 데이터베이스 컨텍스트 및 현재 연결 컨텍스트가 이 문에 적용됩니다. 이러한 경우 Transact-SQL 문이 일괄 처리에 있을 경우 현재 연결 컨텍스트는 USE 문으로 정의된 데이터베이스입니다. Transact-SQL 문이 저장 프로시저에 있을 경우 연결 컨텍스트는 이 저장 프로시저가 포함된 데이터베이스입니다.
  * 계산 열이나 인덱싱된 뷰에서 인덱스를 만들고 조작할 때는 ARITHABORT, CONCAT\_NULL\_YIELDS\_NULL, QUOTED\_IDENTIFIER, ANSI\_NULLS, ANSI\_PADDING 및 ANSI\_WARNINGS SET 옵션을 ON으로 설정해야 합니다. NUMERIC\_ROUNDABORT 옵션은 OFF로 설정해야 합니다. 이 옵션 중 하나라도 필요한 값으로 설정하지 않으면 인덱싱된 뷰나 계산 열에 인덱스가 있는 테이블에서의 INSERT, UPDATE, DELETE, DBCC CHECKDB 및 DBCC CHECKTABLE 동작이 실패합니다. SQL Server는 잘못 설정된 모든 옵션을 나열하는 오류 메시지를 표시합니다. 또한 SQL Server에서는 옵션이 잘못 설정된 테이블이나 인덱싱된 뷰에 대해 계산 열이나 뷰에 이러한 인덱스가 존재하지 않는 것처럼 SELECT 문을 처리합니다.