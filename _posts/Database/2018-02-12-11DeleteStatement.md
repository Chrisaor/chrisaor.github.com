---
layout: post
title:  "SQL 강좌 11.SQL DELETE Statement"
date:   2018-02-13 10:20:00 +0900
categories: database
---

## The SQL DELETE Statement

테이블에 이미 존재하는 레코드를 삭제할 때 사용한다.

**DELETE Syntax**

```sql
DELETE FROM table_name
WHERE condition;
```

**주의!** Where절을 빼먹고 삭제하면 모든 테이블이 삭제됨


## SQL DELETE Example

Customers테이블에서 Alfreds Futtekiste에 해당하는 레코드들을 지워보자

```sql
DELETE FROM Customers
WHERE CustomerName='Alfreds Futterkiste';
```

## Delete All Records

테이블을 제외한 모든 레코드들을 지운다. 테이블 구조, 속성, 색인들은 그대로 남아있는다

```sql
DELETE FROM table_name;
```
또는

```sql
DELETE * FROM table_name;
```

