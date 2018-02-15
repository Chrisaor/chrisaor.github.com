---
layout: post
title:  "SQL 강좌 14.SQL COUNT(), AVG() and SUM() Functions"
date:   2018-02-15 09:25:00 +0900
categories: database
---

COUNT() : 특정 기준과 일치하는 행의 수를 센다.

AVG() : 숫자로 된 column의 평균 값을 반환한다.

SUM() : 숫자로 된 column의 합을 반환한다.

**COUNT() Syntax**

```sql
SELECT COUNT(column_name)
FROM table_name
WHERE condition;
```

**AVG() Syntax**

```sql
SELECT AVG(column_name)
FROM table_name
WHERE condition;
```

**SUM() Syntax**

```sql
SELECT SUM(column_name)
FROM table_name
WHERE condition;
```

## COUNT() Example

```sql
SELECT COUNT(ProductID)
FROM Products;
```

## AVG() Example

```sql
SELECT AVG(Price)
FROM Products;
```

## SUM() Example

```sql
SELECT SUM(Quantity)
FROM OrderDetatils;
```

