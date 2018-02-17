---
layout: post
title:  "SQL 강좌 48. SQL AUTO INCREMENT Field"
date:   2018-02-17 12:10:00 +0900
categories: database
---

## AUTO INCREMENT Field

Auto-increment는 새로운 레코드가 테이블에 쓰여질 때 고객ID와 같은 유일한 숫자가 자동으로 생성되는 것을 허락한다.

보통 primary key필드라고 생각하면 된다.

## Syntax for MySQL

Persons테이블에 레코드가 쓰여질 때마다 자동적으로 생성되는 primary key만들기

```sql
CREATE TABLE Persons (
	ID int NOT NULL AUTO_INCREMENT,
	LastName varchar(255) NOT NULL,
	FirstName varchar(255),
	Age int,
	PRIMARY KEY (ID)
);
```

기본적으로 AUTO_INCREMENT는 1부터 시작하며 새로운 레코드마다 1씩 증가한 값을 갖는다

만약에 시작 값을 다르게 설정하고 싶다면

```sql
ALTER TABLE Persons AUTO_INCREMENT=100;
```

새로운 레코드를 테이블에 넣을 때, 굳이 ID column을 명시하지 않아도 된다

```sql
INSERT INTO Persons (FirstName, LastName)
VALUES ('Lars', 'Monsen');
```

위 구문으로 최근값보다 1이 증가한 ID값을 갖고 FirstName에 Lars, LastName에 Monsen의 값을 갖는 레코드가 생성된다

## Syntax for SQL Server

```sql
CREATE TABLE Persons (
	ID int IDENTITY(1,1) PRIMARY KEY,
	LastName varchar(255) NOT NULL,
	FirstName varchar(255),
	Age int
);
```

MS SQL Server는 IDENTITY 키워드를 사용하는데, 위 구문은 IDENTITY는 1에서 시작하여 1씩 증가하는 새로운 레코드의 ID값을 생성한다.

**Tip:** IDENTITY(10,5)는 10에서 시작하여 5씩 증가하는 ID값을 줄 것이다.

마찬가지로 일부러 명시하지 않아도 ID값이 생성된다

```sql
INSERT INTO Persons (FirstName, LastName)
VALUES ('Lars', 'Monsen');
```

## Syntax for Access

```sql
CREATE TABLE Persons (
    ID Integer PRIMARY KEY AUTOINCREMENT(1,1),
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);
```

(1,1)을 굳이 쓰지않아도 디폴트 값으로 적용된다.

## Syntax for Oracle

```sql
CREATE SEQUENCE seq_person
MINVALUE 1
START WITH 1
INCREMENT BY 1
CACHE 10;
```

seq_person이라는 이름의 시퀀스 오브젝트를 먼저 생성한다.
1부터 시작하며 1씩 증가한다. Cache는 얼마나 많은 시퀀스 값을 메모리에 저장해놓고 빠르게 접근할지 명시한다.

새로운 레코드를 Persons테이블에 삽입하려면 nextval 함수를 이용해야한다.

```sql
INSERT INTO Persons (ID, FirstName, LastName)
VALUES (seq_person.nextval, 'Lars', 'Monsen');
```
