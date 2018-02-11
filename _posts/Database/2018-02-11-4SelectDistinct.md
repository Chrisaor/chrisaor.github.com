---
layout: post
title:  "SQL 강좌 4.SELECT DISTINCT"
date:   2018-02-11 11:50:00 +0900
categories: database
---

## The SQL SELECT DISTINCT Statement

테이블에서 한 column은 반복된(같은) 값을 포함할 수 있다. 그 중복을 제거한 유일한 값만 가져오려면 SELECT DISTINCT를 사용하면 된다.


```sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```


예를들면  구매자들의 거주 도시를 파악하려면 중복을 제거한 유일한 도시의 값만 가져오면 된다. 

```sql
SELECT DISTINCT City FROM Customers;
```

<img width="567" alt="2018-02-11 10 51 54" src="https://user-images.githubusercontent.com/33015649/36074075-40456ea8-0f7e-11e8-9450-9844013f4d7d.png">

유일한 값들의 개수를 세려면
SELECT COUNT(DISTINCT column)을 사용한다.

```sql
SELECT COUNT(DISTINCT City) FROM Customers;
```

<img width="555" alt="2018-02-11 10 55 52" src="https://user-images.githubusercontent.com/33015649/36074109-bdb80314-0f7e-11e8-8bb7-e883e02741f0.png">

