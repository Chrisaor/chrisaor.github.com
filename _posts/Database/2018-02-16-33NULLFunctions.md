---
layout: post
title:  "SQL 강좌 33.SQL NULL Functions"
date:   2018-02-16 11:00:00 +0900
categories: database
---

## SQL IFNULL(), ISNULL(), COALESCE()  and NVL() Functions

다음 Products 테이블을 보자

<img width="601" alt="2018-02-16 6 18 32" src="https://user-images.githubusercontent.com/33015649/36300776-d593a50e-1345-11e8-9063-aa1b51357be9.png">

UnitsOnOrder의 필드는 필수로 적어야하는 항목이 아니며 NULL을 포함할 수 있다.

다음 구문은 어떠한 UnitsOnOrder의 값이 NULL이라면 결과는 NULL이 될 것이다.

```sql
SELECT ProductName, UnitPrice * (UnitsInStock + UnitsOnOrder)
FROM Products;
```

NULL값으로 연산을 하면 무조건 NULL이 된다는 소리인듯?

따라서 해결책은 다음과 같다

## Solutions

**MySQL**

```sql
SELECT ProductName, UnitPrice * (UnitsInStock + IFNULL(UnitsOnOrder, 0))
FROM Products
```
또는

```sql
SELECT ProductName, UnitPrice * (UnitsInStock + COALESCE(UnitsOnOrder, 0))
FROM Products
```

**SQL Server**

```sql
SELECT ProductName, UnitPrice * (UnitsInStock + ISNULL(UnitsOnOrder, 0))
FROM Products
```

**MS Access**

```sql
SELECT ProductName, UnitPrice * (UnitsInStock + IIF(IsNull(UnitsOnOrder), 0, UnitsOnOrder))
FROM Products
```

**Oracle**

```sql
SELECT ProductName, UnitPrice * (UnitsInStock + NVL(UnitsOnOrder, 0))
FROM Products
```



