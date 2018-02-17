---
layout: post
title:  "SQL 강좌 41. SQL NOT NULL Constraints"
date:   2018-02-17 11:35:00 +0900
categories: database
---

## SQL NOT NULL Constraint

기본적으로 column은 NULL값을 가질 수 있지만 NOT NULL constraint를 사용하면 해당 column은 절대 NULL값을 받을 수 없다


다음 예제는 Persons테이블에서 ID값, 성과 이름에 절대 NULL을 받을 수 없게 constraint를 거는 것이다.

**Example**

```sql
CREATE TABLE Persons (
	ID int NOT NULL,
	LastName varchar(255) NOT NULL,
	FirstName varchar(255) NOT NULL,
	Age int
);
```

**Tip:** 만약에 이미 만들어진 테이블에 constraint를 걸고싶으면 ALTER TABLE구문을 이용해서 바꾸면 된다.