---
layout: post
title:  "SQL 강좌 9.SQL UPDATE Statement"
date:   2018-02-12 10:15:00 +0900
categories: database
---

## The SQL UPDATE Statement

이미 존재하는 데이터에 정보가 변경되었을 때 수정하는 방법

**SQL UPDATE Syntax**

```sql
UPDATE table_name
SET column1=value1, column2=value2, ...
WHERE some_column=some_value;
```

WHERE를 써서 어떠한 특정 레코드의 값이 바뀔지 명시해야함. 그렇지 않으면 전체의 테이블이 바뀔 수 있다.

## UPDATE table

```sql
UPDATE Customers
SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
WHERE CustomerID = 1;
```

## UPDATE Multiple Records

WHERE 절은 얼마나 많은 레코드를 바꿀지 결정한다. 다음 SQL문은 멕시코 국적의 모든 contact name을 Juan으로 업데이트(수정)하는 구문이다.

```sql
UPDATE Customers
SET ContactName='Juan'
WHERE Country='Mexico';
```

## Update Warning!

WHERE 절을 빼먹으면 ㅈ된다. 모든 레코드의 값이 바뀌니까

```sql
UPDATE Customers
SET ContactName='Juan';
```

<img width="825" alt="2018-02-12 12 21 21" src="https://user-images.githubusercontent.com/33015649/36074961-b4cbdefe-0f8a-11e8-8e59-0de7c00627fb.png">