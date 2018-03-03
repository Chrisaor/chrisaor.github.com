---
layout: post
title:  "[Project] 1-2. Model field reference"
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

---

### db_tablespace

**Field.db_tablespace**

이 필드의 index가 작성될 경우, 이 필드의 index에 사용할 데이터베이스 tablespace의 이름을 지정한다. 기본값은 프로젝트의 DEFAULT\_INDEX\_TABLESPACE 또는 모델의 db_tablespace이다. 백엔드가 인덱스의 tablespace를 지원하지 않으면 이 옵션은 무시됨

---

### default

**Field.default**

필드의 기본값을 지정함. 값 또는 호출 가능한 객체일 수 있다. 호출 가능한 객체의 경우 객체가 생성될 때마다 호출될 것이다.

기본 값은 변경 가능한 객체(모델 인스턴스, 리스트, 셋 등)일 수 없다. 이는 해당 객체의 동일한 인스턴스에 대한 참조가 모든 새 모델 인스턴스에서 기본 값으로 사용되기 때문이다. 

대신, 원하는 기본 값을 호출 가능한 코드로 감싸면 된다. 예를 들어 JSONField의 기본 dict를 지정하려면 다음 함수를 이용한다.

```python
def contact_default():
	return {"email": "to1@example.com"}
	
contact_info = JSONField("ContactInfo", default = contact_default)
```

**lambda**는 [마이그레이션에 의해 직렬화](https://docs.djangoproject.com/en/2.0/topics/migrations/#migration-serializing) 될 수 없기 때문에 default와 같은 필드 옵션에 사용될 수 없다. 

모델 인스턴스에 매핑되는 ForeignKey와 같은 필드의 경우 기본값은 모델 인스턴스 대신 참조하는 필드 값(to_field가 설정되지 않은 경우 pk)이어야한다.

기본 값은 새 모델 인스턴스가 만들어지고 값이 필드에 제공되지 않을 때 사용된다. 필드가 primary key인 경우, 필드가 없음으로 설정된 경우 기본값이 사용된다.

---

### error_messages

**Field.error_messages**

error_messages 인수를 사용하면 필드에서 발생시키는 기본 메세지를 대체할 수 있다. 덮어쓰려는 오류 메시지와 일치하는 key가 있는 dict를 전달해야한다.

오류 메시지 키에는 null, blank, invalid, invalid_choice, unique 및 unique_for_date가 포함된다. 추가 오류 메시지 키는 아래 필드 유형 섹션의 각 필드에 지정된다.

이러한 오류 메시지는 form에 전달되지 않는 경우가 많다. [Conisderations regarding model's error_messages](https://docs.djangoproject.com/en/2.0/topics/forms/modelforms/#considerations-regarding-model-errormessages)를 참조

---

### help_text

**Field.help_text**

폼 위젯과 함께 표시되는 '도움말'텍스트. 필드가 폼에 사용되지 않아도 문서화할 때 유용하다.

이 값은 자동 생성된 폼에서 HTML 이스케이프 처리되지 않는다. 원하는 경우 help_text에 HTML을 포함시킬 수 있다. 

```
help_text="Please use the following format: <em>YYYY-MM-DD</em>."
```

또는 일반 텍스트와 django.utils.html.escape()를 사용하여 HTML 특수 문자를 이스케이프 처리할 수 있다. cross-site scripting attack을 피하기 위해 신뢰할 수 없는 사용자의 도움말 텍스트를 이스케이프 해야한다.

---

### primary_key

**Field.primary_key**

True로 설정하면 해당 필드는 모델의 primary key가 된다.

모델의 모든 필드에 대해 primary_key = True를 지정하지 않으면 장고는 primary key를 보유할 자동 필드를 자동으로 추가한다. 따라서 primary key 동작을 덮어쓰려고 하지 않는 한 굳이 설정할 필요 없다. [Automatic primary key fields](https://docs.djangoproject.com/en/2.0/topics/db/models/#automatic-primary-key-fields) 참조

primary_key = True로 설정하는 것은 null = False 및 unique = True를 의미한다. 한 객체에는 오직 하나의 primary key만이 허용된다.

primary key 필드는 읽기 전용이다. 기존 객체의 primary key 값을 변경한 다음에 저장하면 새로운 키 값이 생성된다.



