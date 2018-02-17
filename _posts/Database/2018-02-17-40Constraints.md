---
layout: post
title:  "SQL 강좌 40. SQL Constraints"
date:   2018-02-17 11:30:00 +0900
categories: database
---

## SQL Constraints

테이블 내의 데이터에 대한 규칙을 명시하는데 사용된다.

테이블 안에 들어갈 데이터 타입을 제한할 때 사용되고 이는 데이터의 정확성과 신뢰성을 보장한다. 만약에 constraint와 데이터 액션 사이에 충돌이 일어날 경우 데이터 액션은 실행되지 않는다.

Constraint는 column레벨 또는 테이블 레벨에서 행해진다. Column레벨의 constraint 는 column에 적용이 되고 table레벨의 constraint는 전체 테이블에 적용된다.

- NOT NULL : column에는 NULL이 아닌 값이 들어가야한다
- UNIQUE : 중복된 값은 들어갈 수 없다
- PRIMARY KEY : NOT NULL과  UNIQUE의 결합. 고유한 값이며 반드시 값이 존재해야함
- FOREIGN KEY : 다른 테이블에 행 또는 레코드가 유일하게 존재한다.
- CHECK : 모든 값이 특정한 조건에 만족하는지 확인한다
- DEFAULT : 기본 값을 설정한다.
- INDEX : 데이터베이스로부터 빠르게 데이터를 생성하고 반환할 때 사용된다.



## SQL Create Constraints

Constratint는 CREATE TABLE구문으로 테이블이 생성될 때 명시될 수 있다.

**Syntax**

```sql
CREATE TABLE table_name (
	column1 datatype constratint,
	column2 datatype constratint,
	column3 datatype constratint,
	...
);
	
