---
layout: post
title:  "SQL 강좌 32.SQL INSERT INTO SELECT Statement"
date:   2018-02-16 10:55:00 +0900
categories: database
---

## The SQL INSERT INTO SELECT Statement
 
INSERT INTO SELECT 구문은 한 테이블로부터 데이터를 복사하고 다른 테이블로 삽입하는 명령어다.

- 데이터 타입이 같아야한다
- 타겟 테이블의 기존 레코드에 영향을 미치지않는다.

**INSERT INTO SELECT Syntax**

모든 column들을 그대로 복사할 때

```sql
INSERT INTO table2
SELECT * FROM table1
WHERE condition;
```

특정 column들만 복사할 때

```sql
INSERT INTO table2 (column1, column2, column3, ...)
SELECT column1, column2, column3, ...
FROM table1
WHERE condition;
```

## SQL INSERT INTO SELECT Examples

Suppliers를 복사하여 Customers 테이블에 삽입하기(데이터로 채워지지 않은 필드는 NULL이 들어감)

```sql
INSERT INTO Customers (CustomerName, City, Country)
SELECT SupplierName, City, Country FROM Suppliers;
```

독일 suppliers만 복사하여 Customers 테이블에 삽입하기

```sql
INSERT INTO Customers (CustomerName, City, Country)
SELECT SupplierName, City, Country FROM Suppliers
WHERE Country='Germany';
```




