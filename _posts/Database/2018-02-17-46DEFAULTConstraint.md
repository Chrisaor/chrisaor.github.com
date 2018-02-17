---
layout: post
title:  "SQL 강좌 46. SQL DEFAULT Constraints"
date:   2018-02-17 12:00:00 +0900
categories: database
---

## SQL DEFAULT Constraint

디폴드 값을 준다. 새롭게 추가되는 레코드에 아무런 값이 주어지지 않았을 때 기본적으로 들어가는 값을 말한다.

## SQL DEFAULT on CREATE TABLE

Persons 테이블이 생성될 때 City의 디폴트 값으로 'Sandnes'를 준다

**My SQL / SQL Server / Oracle / MS Access:**

```sql
CREATE TABLE Persons (
	ID int NOT NULL,
	LastName varchar(255) NOT NULL,
	FirstName varchar(255),
	Age int,
	City varchar(255) DEFAULT 'Sandnes'
);
```

System 값을 삽입하는데 사용되기도 한다.

```sql
CREATE TABLE Orders(
	ID int NOT NULL,
	OrderNumber int NOT NULL,
	OrderDate date DEFAULT GETDATE()
);
```

## SQL DEFAULT on ALTER TABLE

이미 생성된 테이블에 디폴트 적용하기

**MySQL:**

```sql
ALTER TABLE Persons
ALTER City SET DEFAULT 'Sandnes';
```

**SQL Server / MS Access:**

```sql
ALTER TABLE Persons
ALTER COLUMN City SET DEFAULT 'Sandnes';
```

**Oracle:**

```sql
ALTER TABLE Persons
MODIFY City DEFAULT 'Sandnes';
```

## DROP a DEFAULT Constraint

디폴트 제거하기

**MySQL:**

```sql
ALTER TABLE Persons
ALTER City DROP DEFAULT;
```

**SQL Server / Oracle / MS Access:**

```sql
ALTER TABLE Persons
ALTER COLUMN City DROP DEFAULT;
```

