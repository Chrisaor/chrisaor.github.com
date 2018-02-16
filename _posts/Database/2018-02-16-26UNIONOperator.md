---
layout: post
title:  "SQL 강좌 26.SQL UNION Operator"
date:   2018-02-16 10:30:00 +0900
categories: database
---

## The SQL UNION Operator

UNION 연산자는 두 개이상의 SELECT 구문의 결과셋을 결합할 때 사용한다.

- UNION안의 각 SELECT구문은 같은 숫자의 column들을 가져야한다.
- 각 column은 비슷한 데이터 타입을 가져야한다.
- 각 SELECT 구문의 column들은 같은 순서여야한다

**UNION Syntax**

```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```

**UNION ALL Syntax**

```sql
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```

**Note:** 결과 셋의 column이름은 보통 첫번째 SELECT구문의 column의 이름과 같다.

## SQL UNION Example

- Customers와 Suppliers의 모든 도시 정보 가져오기(중복X)

```sql
SELECT City FROM Customers
UNION
SELECT City FROM Suppliers
ORDER BY City;
```

**Note:** 같은 이름의 도시가 양쪽 테이블에 있다면 하나만 표시된다. UNION은 오직 유일한 값만 가져온다. 만약에 중복된 값도 허용하려면 UNION ALL을 사용

## SQL UNION ALL Example

- Customers와 Suppliers의 모든 도시 정보 가져오기(중복O)


```sql
SELECT City FROM Customers
UNION ALL
SELECT City FROM Suppliers
ORDER BY City;
```

## SQL UNION With WHERE

- Customer와 Supplier에서 독일의 모든 도시 정보 가져오기(중복X)

```sql
SELECT City, Country FROM Customers
WHERE Country='Germany'
UNION
SELECT City, Country FROM Suppliers
WHERE Country='Germany'
ORDER BY City;
```

## SQL UNION ALL With WHERE

- Customer와 Supplier에서 독일의 모든 도시 정보 가져오기(중복O)

```sql
SELECT City, Country FROM Customers
WHERE Country='Germany'
UNION ALL
SELECT City, Country FROM Suppliers
WHERE Country='Germany'
ORDER BY City;
```

## Another UNION Example


- 'Customer'과 'Supplier'는 Type으로 설정하고 ContactName, City, Country 정보를 가져옴

```sql
SELECT 'Customer' As Type, ContactName, City, Country
FROM Customers
UNION
SELECT 'Supplier', ContactName, City, Country
FROM Suppliers;
```

