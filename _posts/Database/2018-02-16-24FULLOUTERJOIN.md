---
layout: post
title:  "SQL 강좌 24.SQL FULL OUTER JOIN Keyword"
date:   2018-02-16 10:15:00 +0900
categories: database
---

## SQL FULL OUTER JOIN Keyword

FULL OUTER JOIN 키워드는 왼쪽과 오른쪽 테이블의 모든 데이터를 반환한다.

**FULL OUTER JOIN Syntax**

```sql
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2 ON table1.column_name=table2.column_name;
```

## SQL FULL OUTER JOIN Exapmle

Customer 테이블에서 CustomerName을 가져오고, Orders 테이블에서 OrderID를 가져옴. FULL OUTER JOIN으로 양쪽의 테이블 정보를 모두 가져옴.

```sql
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;
```

결과는 다음과 같다

<img width="606" alt="2018-02-16 4 32 48" src="https://user-images.githubusercontent.com/33015649/36297608-0d162d62-1337-11e8-83ee-9ac16c62dbe5.png">

왼쪽 테이블에 있지만 오른쪽 테이블에 존재하지 않거나 그 반대의 경우라도 리스트에는 모두 표시가 된다.
