---
layout: post
title:  "SQL 강좌 9.SQL NULL Values"
date:   2018-02-12 10:15:00 +0900
categories: database
---

## What is a NULL Value?

값이 없는 필드는 Null값이 있는 필드이다.

테이블의 한 필드가 선택적이라면, 새로운 레코드를 삽입하거나 아무 값을 넣지않고 업데이트를 할 수 있다. 아무 값을 넣지 않는다면  NULL 값이 들어갈 수 있다.

Null은 0값과 전혀 다름을 이해하는 것은 중요하다. 

## How to Test for NULL Values?

NULL값은 =, <, != 같은 비교 연산자를 사용하여 테스트할 수 없다. 

IS NULL과 IS NOT NULL 구문을 이용해야한다.

## IS NULL Syntax

```sql
SELECT column_names
FROM table_name
WHERE column_name IS NULL;
```

## IS NOT NULL Syntax

```sql
SELECT column_names
FROM table_name
WHERE column_name IS NOT NULL;
```

## The IS NULL Operator

다음 SQL문은 IS NULL연산자를 사용하여 주소가 없는 사람들의 리스트를 보여준다.

```sql
SELECT LastName, FirstName, Address FROM Persons
WHERE Address IS NULL;
```

**Tip**: NULL 값을 찾을 땐 항상 IS NULL을 사용해라

## The IS NOT NULL Operator

NULL값이 아닌(주소를 갖고 있는) 사람들의 리스트를 준다.

```sql
SELECT LastName, FirstName, Address FROM Persons
WHERE Address IS NOT NULL;
```

