---
layout: post
title:  "[The model layer] 1-2. Model field reference"
date:   2018-02-28 11:30:00 +0900
categories: django
---

구글 번역기의 도움을 (많이) 받아 장고 공식 문서를 번역하였습니다.

# Model field reference

이 문서는 장고가 제공하는 필드 옵션과 필드 유형을 포함하여 Field의 모든 API 레퍼런스를 포함한다.

> 기술적으로, 이들 모델은 django.db.models.fields에 정의되어 지만 편의상 django.db.models로 가져온다.  
> 표준 규칙은 `from django.db import models`를 사용하고 models.<Foo>Field를 참조하는 것이다. 

## Field options

다음 속성 인자들은 모든 필드 유형에서 사용할 수 있으며 선택 사항이다.

### null

**Field.null**

값이 True이면 장고는 빈 값을 NULL로 데이터베이스에 저장한다. 기본값은 False임.

CharField 및 TextField와 같이 문자열 기반 필드에서는 null을 사용하지 않는 것이 좋다. 문자열 기반 필드에 `null=True`가 있으면 '데이터 없음'에 대해 가능한 두 개의 값이 있는 것이다. 바로 NULL과 빈 문자열(blank). 대부분의 경우 '데이터 없음'에 대해 한 가지 값만 갖는 것이 바람직하다. 장고의 규칙은 NULL이 아닌 빈 문자열을 사용하는 것이다.

한 가지 예외는 CharField에 `unique=True`와 `blank=True`가 설정된 경우다. 이 경우, 빈 값으로 여러 오브젝트를 저장할 때 unique constraint 조건 위반을 피하기 위해 `null=True`가 필요하다.

문자열 기반 및 비문자열 기반 필드의 경우 null 매개변수가 데이터베이스 저장소에만 영향을 주기 때문에 약식에서 빈 값을 허용하려면 `blank=True`를 설정해야함.

> **Note:** Oracle 데이터베이스 백엔드를 사용할 때 이 값과 상관없이 NULL값이 저장되어 빈 문자열을 나타냄.

BooleanField로 null값을 허용하려면 대신 NullBooleanField를 사용해라.

---

### blank

**Field.blank**

True로 설정해놓으면 필드에 빈 값이 허용된다. 기본값은 마찬가지로 False.

null과는 다르다는 점을 알아야한다. null은 순전히 데이터베스와 관련된 설정이지만 blank는 유효성 검사와 관련이 있다. 필드에 `blank=True`가 있으면 폼 유효성 검사에서 빈 값을 입력할 수 있다. 필드에 `blank=False`가 있으면 필드가 필요하다.

---

### choices

**Field.choices**

이 필드의 선택 항목으로 두 개의 원소가 있는 iterable한 리스트 또는 튜플을 사용한다.

iterable한 객체가 속성에 주어지면 기본 form widget은 표준 텍스트 필드 대신 선택사항을 가진 select box가 생성된다.

각 튜플의 첫 번째 요소는 모델에 설정할 실제 값이고 두 번째 요소는 사람이 읽을 수 있는 이름이다. 

```python
YEAR_IN_SCHOOL_CHOICES = (
	('FR', 'Freshman'),
	('SO', 'Sophomore'),
	('JR', 'Junior'),
	('SR', 'Senior'),
)
```
 
일반적으로 모델 클래스 내에서 선택 사항을 정의하고 각 값에 대해 적절히 명명된 상수를 정의하는 것이 좋다. 

```python
from django.db import models

class Student(models.Model):
    FRESHMAN = 'FR'
    SOPHOMORE = 'SO'
    JUNIOR = 'JR'
    SENIOR = 'SR'
    YEAR_IN_SCHOOL_CHOICES = (
        (FRESHMAN, 'Freshman'),
        (SOPHOMORE, 'Sophomore'),
        (JUNIOR, 'Junior'),
        (SENIOR, 'Senior'),
    )
    year_in_school = models.CharField(
        max_length=2,
        choices=YEAR_IN_SCHOOL_CHOICES,
        default=FRESHMAN,
    )

    def is_upperclass(self):
        return self.year_in_school in (self.JUNIOR, self.SENIOR)
```

모델 클래스 바깥쪽에서 선택 목록을 정의한 다음 참조할 수 있지만, 모델 클래스 내의 각 선택 항목에 대해 선택 사항과 이름을 정의하면 해당 정보를 모두 사용하는 클래스와 함께 유지되므로,선택 사항을 쉽게 참조할 수 있게 한다. (예: Student.SOPHOMORE는 Student 모델을 가져온 곳이면 어디에서나 작동함)

---

### db_column

**Field.db_column**

이 필드에 사용할 데이터베이스 열의 이름. 따로 설정하지 않으면 그냥 필드의 이름을 사용한다.

데이터베이스 열 이름이 SQL 예약어거나 Python변수 이름에 허용되지 않는 문자여도 상관없다. 장고는 이면에서 열과 테이블의 이름을 인용한다.

---

### db_index

**Field.db_index**

True로 설정하면 필드에 대해 데이터베이스 인덱스가 생성된다.





 
