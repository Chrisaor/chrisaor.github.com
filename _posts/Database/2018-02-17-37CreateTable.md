---
layout: post
title:  "SQL 강좌 37. SQL CREATE TABLE Statement"
date:   2018-02-17 11:15:00 +0900
categories: database
---

## The SQL CREATE TABLE Statement

데이터베이스에 새로운 테이블을 생성할 때 사용된다

**Syntax**

```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);
```

column1, column2 등의 파라미터들은 column들의 이름을 명시한다.
datatype 파라미터들은 column의 데이터 타입(varchar, integer, date등)을 명시한다.

## SQL CREATE TABLE Example

Persons에 해당하는 테이블을 생성한다. 이 테이블은 PersonID, LastName, FirstName, Address 그리고 City 5개의 필드를 갖는다


```sql
CREATE TABLE Persons(
	PersonID int,
	LastName varchar(255),
	FirstName varchar(255),
	Address varchar(255),
	City varchar(255)
);
```

## Create Table Using Another Table

다른 테이블을 이용해서 새로운 테이블을 만들 수 있다.

CREATE TABLE과 SELECT구문을 이용해서 이미 존재하는 테이블의 틀을 그대로 가져올 수 있다.

```sql
CREATE TABLE new_table_name AS
    SELECT column1, column2,...
    FROM existing_table_name
    WHERE 1=0;
```


