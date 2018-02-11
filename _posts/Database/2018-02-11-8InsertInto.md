---
layout: post
title:  "SQL 강좌 8.SQL INSERT INTO Statement"
date:   2018-02-11 12:10:00 +0900
categories: database
---

## The SQL INSERT INTO Statement

new records를 테이블에 삽입할 때 사용됨  
INSERT INTO구문은 두 가지 형식이 있다.

첫 번째는 column 이름을 적어주고 그에 매치되는 값들을 삽입하는 것. 값이 입력되지 않은 column에는 null값이 들어간다.

```sql
INSERT INTO table_name(column1, column2, ...)
VALUES (value1, value2, ...);
```

두 번째는 column의 이름을 명시하지 않고 삽입하는 것이다. 만약 column이 7개 있다면 value에도 7개의 값을 입력해야 오류없이 삽입가능하다. 그리고 순서에 주의하자. 

```sql
INSERT INTO table_name
VALUES (value1, value2, value3,...);
```

## INSERT INTO Example

```sql
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');
```

## Insert Data Only in Specified Columns

특정한 column에만 값을 넣을 경우 빈 필드에는 null이 들어감.

```sql
INSERT INTO Customers (CustomerName, City, Country)
VALUES ('Cardinal', 'Stavanger', 'Norway');
```

<img width="834" alt="2018-02-11 11 56 56" src="https://user-images.githubusercontent.com/33015649/36074712-4a740a48-0f87-11e8-9a4d-2a78f5895f68.png">

