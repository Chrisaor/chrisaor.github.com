---
layout: post
title:  "SQL 강좌 42. SQL UNIQUE Constraints"
date:   2018-02-17 11:40:00 +0900
categories: database
---

## SQL UNIQUE Constraint

모든 값들이 서로 다른 값임을 보장한다.

UNIQUE와 PRIMARY KEY는 둘다 고유성을 보장하지만 UNIQUE는 테이블 마다 여러개를 설정할 수 있지만 PRIMARY KEY는 테이블마다 단 하나의 값만 가져야한다.

## SQL UNIQUE Constraint on CREATE TABLE

Persons테이블이 생성될 때 ID에 UNIQUE constraint를 건다

**SQL Server / Oracle / MS Access:**

```sql
CREATE TABLE Persons (
	ID int NOT NULL UNIQUE,
	LastName varchar(255) NOT NULL,
	FirstName varchar(255),
	Age int
```

**MySQL:**

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    UNIQUE (ID)
);
```

여러 column에 한번에 UNIQUE constraint를 걸려면

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT UC_Person UNIQUE (ID,LastName)
);
```

## SQL UNIQUE Constraint on ALTER TABLE

이미 만들어진 테이블에 ALTER TABLE을 통해 constraint추가하기

```sql
ALTER TABLE Persons
ADD UNIQUE (ID);
```

UNIQUE constraint에 이름을 붙이고 여러개 column에 걸때

```sql
ALTER TABLE Persons
ADD CONSTRAINT UC_Person UNIQUE (ID,LastName);
```

## DROP a UNIQUE Constraint

UNIQUE constraint를 제거할 때

**MySQL**

```sql
ALTER TABLE Persons
DROP INDEX UC_Person;
```

**SQL Server / Oracle / MS Access:**

```sql
ALTER TABLE Persons
DROP CONSTRAINT UC_Persons;
```
