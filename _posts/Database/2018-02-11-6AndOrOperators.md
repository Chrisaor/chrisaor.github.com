---
layout: post
title:  "SQL 강좌 6.SQL AND & OR Operators"
date:   2018-02-11 12:00:00 +0900
categories: database
---

## The SQL AND & OR Operators

AND 연산자는 두 개 이상의 조건이 모두 만족할 때 True  
OR 연산자는 앞 조건이나 뒤 조건이 하나라도 만족하면 True


## AND Operator Example

나라가 독일 **그리고** 도시가 베를린인 데이터 가져오기

```sql
SELECT * FROM Customers
WHERE Country='Germany'
AND City='Berlin';
```

## OR Operator Example

도시가 베를린 **또는** 런던인 데이터 가져오기

```sql
SELECT * FROM Customers 
WHERE City='Berlin' 
OR City='London'
```

## Combining AND & OR

나라가 독일이면서 도시가 베를린 또는 뮌휀인 데이터 가져오기

```sql
SELECT * FROM Customers
WHERE Country='Germany'
AND (City='Berlin' OR City='München')
```



