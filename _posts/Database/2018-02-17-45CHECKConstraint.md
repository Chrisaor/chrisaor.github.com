---
layout: post
title:  "SQL 강좌 45. SQL CHECK Constraints"
date:   2018-02-17 11:55:00 +0900
categories: database
---

## SQL CHECK Constraint

column에 들어갈 값의 범위를 제한할 떄 사용된다.

CHECK constraint를 한 column에 걸 때, 오직 특정한 값만 허용한다.
만약 테이블에 걸 때는 그 행의 다른 column에 있는 값을 기준으로 특정 column의 값을 제한 할 수 있다.

## SQL CHECK on CREATE TABLE

Person 테이블을 생성할 때 Age column에 CHECK constraint를 건다. 다음과 같이 조건을 걸면 18세 이하의 사람은 등록되지 않는다.

**MySQL:**

```sql
CREATE TABLE Persons (
	ID int NOT NULL,
	LastName varchar(255) NOT NULL,
	FirstName varchar(255),
	Age int,
	CHECK (Age>=18)
);
```

**SQL Server / Oracle / MS Access:**

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int CHECK (Age>=18)
);
```

CHECK constraint에 이름을 주고 나이가 18세 이상이고 City가 Sandnes인 조건을 준다.

**MySQL / SQL Server / Oracle / MS Access:**

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    City varchar(255),
    CONSTRAINT CHK_Person CHECK (Age>=18 AND City='Sandnes')
);
```

## SQL CHECK on ALTER TABLE

**MySQL / SQL Server / Oracle / MS Access:**

```sql
ALTER TABLE Persons
ADD CHECK (Age>=18);
```

**MySQL / SQL Server / Oracle / MS Access:**

```sql
ALTER TABLE Persons
ADD CONSTRAINT CHK_PersonAge CHECK (Age>=18 AND City='Sandnes');
```

## DROP a CHECK Constraint

CHECK Constraint 제거

**SQL Server / Oracle / MS Access:**

```sql
ALTER TABLE Persons
DROP CONSTRAINT CHK_PersonAge;
```

**MySQL:**

```sql
ALTER TABLE Persons
DROP CHECK CHK_PersonAge;
```






