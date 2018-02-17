---
layout: post
title:  "SQL 강좌 47. SQL CREATE INDEX Statement"
date:   2018-02-17 12:05:00 +0900
categories: database
---

## SQL CREATE INDEX Statement

테이블 내의 인덱스(색인)를 생성할 때 사용된다

인덱스는 데이터베이스로부터 매우 빠르게 데이터를 얻으려 할 때 사용된다. 유저들은 인덱스를 볼 수 없다. 그냥 빠른 검색/쿼리를 위해서 사용된다.

**Note:**  인덱스가 있는 테이블을 업데이트할 때는 시간이 더 오래 걸린다. 왜냐하면 레코드들 뿐만 아니라 인덱스 또한 업데이트가 필요하기 때문이다. 따라서 자주 검색할 column들에만 인덱스를 생성하는게 좋다.


**CREATE INDEX Syntax**

테이블에 인덱스를 생성한다. 중복된 값이 허용된다.

```sql
CREATE INDEX index_name
ON table_name (column1, column2, ...);
```

**CREATE UNIQUE INDEX Syntax**

테이블에 인덱스를 생성하되, 중복된 값이 허용되지 않는다

```sql
CREATE UNIQUE index_name
ON table_name (column1, column2, ...);
```

## CREATE INDEX Example

Persons테이블에 있는 LastName column에 idx_lastname이라는 인덱스를 생성한다

```sql
CREATE INDEX idx_lastname
ON Persons (LastName);
```

여러 column들을 묶어서 인덱스를 생성하고 싶을 땐 콤마로 구분된 괄호로 묶는다.

```sql
CREATE INDEX idx_pname
ON Persons (LastName, FirstName);
```

## DROP INDEX Statement

인덱스를 삭제할 때

**MS Access:**

```sql
DROP INDEX index_name ON table_name;
```

**SQL Server:**

```sql
DROP INDEX table_name.index_name;
```

**DB2/Oracle:**

```sql
DROP INDEX index_name;
```

**MySQL:**

```sql
ALTER TABLE table_name
DROP INDEX index_name;
```

