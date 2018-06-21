---
layout: post
title:  "[The model layer] Model field reference"
date:   2018-02-18 11:30:00 +0900
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

### primary\_key

**Field.primary\_key**

**True**로 설정하면 해당 필드는 모델의 primary key가 된다.

모델의 모든 필드에 대해 **primary_key = True**를 지정하지 않으면 장고는 primary key를 보유할 **AutoField**를 자동으로 추가한다. 따라서 primary key 동작을 덮어쓰려고 하지 않는 한 굳이 설정할 필요 없다. [Automatic primary key fields](https://docs.djangoproject.com/en/2.0/topics/db/models/#automatic-primary-key-fields) 참조

**primary_key = True**로 설정하는 것은 **null = False** 및 **unique = True**를 의미한다. 한 객체에는 오직 하나의 primary key만이 허용된다.

primary key 필드는 읽기 전용이다. 기존 객체의 primary key 값을 변경한 다음에 저장하면 새로운 키 값이 생성된다.



---

### unique

**Field.unique**

만약에 True로 설정되어 있다면 필드는 테이블 전체에서 유일한 값을 가져야 한다.

이는 데이터 베이스 레벨에서 모델 유효성 검사에 의해 시행된다. **unique** 설정이 되어있는 필드에 중복값이 있는 모델을 저장하려고 하면 모델의 **save()** 메소드에 의해 **django.db.IntegrityError**가 발생한다.

이 옵션은 ManyToManyField 및 OneToOneField를 제외한 모든 필드 유형에 유효하다.

unique를 설정하면 인덱스 생성을 의미하므로 db_index를 따로 지정할 필요가 없다.

---

### unique\_for\_date

**Field.unique\_for\_date**

이것을 DateField 또는 DateTimeField의 이름으로 설정하여 이 필드가 날짜 값에 대해 고유한 값을 가지게 한다.

예를 들어, **title** 필드에 **unique\_for\_date='pub_date'** 속성을 주면 장고는 같은 **title**, **pub_date**를 허용하지 않는다.

**DateTimeField**로 설정한다면 필드의 날짜부분만 고려된다. 게다가 **USE_TZ**이 **True**일 때, 객체가 저장될 때 현지 시간대에서 검사가 수행된다.

모델 유효성 검사 중에는 Model.validate_unique()에 의해 적용되지만 데이터베이스 수준에서는 적용되지 않는다. unique\_for\_date 제약 조건에 ModelForm의 일부가 아닌 필드가 포함되는 경우(예를 들어, 필드 중에 하나가 exclude목록에 있거나 나 editable=False 설정이 된 경우), Model.validate\_unique()는 그 특정 제약에 의해 유효성 검사를 건너뛴다.

---
### unique\_for\_month

**Field.unique\_for\_month**

**unique\_for\_date**와 같지만 월을 기준으로 필드에 unique옵션이 적용된다.

---
### unique\_for\_year

**Field.unique\_for\_year**

**unique_for_date**와 **unique_for_month**처럼 사용하면 된다.

---
### verbose_name

Field.verbose_name

필드에 인간친화적인 이름을 붙인다. 만약에 verbose name이 주어지지 않으면 장고는 자동으로 필드의 속성이름을 자동으로 변환한다(언더스코어는 스페이스로). [Verbose field](https://docs.djangoproject.com/en/2.0/topics/db/models/#verbose-field-names)를 참고

---
### validators

**Field.validators**

필드에 실행할 validator(유효성 검사) 목록이다. 자세한 내용은 [유효성 검사 문서](https://docs.djangoproject.com/en/2.0/ref/validators/)를 참조.


#### 검색 등록 및 가져오기

필드는 조회 등록 API를 구현한다. API를 사용하여 필드 클래스에 사용할 수 있는 조회와 필드에서 조회를 가져오는 방법을 사용자 정의할 수 있다.

## Field types

### AutoField

**class AutoField(\*\*options)**

사용 가능한 ID에 따라 자동으로 증가하는 IntegerField이다. 보통 이것을 직접 사용할 필요는 없다. 별도로 지정하지 않으면 primary key필드가 자동으로 모델에 추가된다. [Automatic primary key fields](https://docs.djangoproject.com/en/2.0/topics/db/models/#automatic-primary-key-fields)를 참고

### BigAutoField

**class BigAutoField(\*\*options)**

1에서 9223372036854775807까지 숫자를 보장하는 AutoField와 같다.

### BinaryField

**class BinaryField(\*\*options)**

raw binary data를 저장하는 필드이다. bytes 할당만 지원한다. 기능이 제한되어 있다. 예를 들어, BinaryField 값에 대한 쿼리 집합을 필터링 할 수 없다. 또한 BinaryField를 ModelForm에 포함시키는 것도 불가능하다.

> **BinaryField의 남용**
> 
> DB에 파일을 저장하는 것에 대해 생각할 수 있지만 99%의 경우 잘못된 디자인이다. 이 필드는 적절한 정적파일 처리를 대체하지 않는다.

### BooleanField

**class BooleanField(\*\*options)**

True/False 필드

이 필드의 기본 양식 위젯은 CheckboxInput이다. null값을 받아 들일 필요가 있다면 NullBooleanField를 사용해라. Field.default가 정의되어 있지 않으면 BooleanField의 기본값은 None이다.

### CharField

**class CharField(max_length=None, \*\*options)**

작거나 큰 크기의 문자열을 나타내는 필드이다. 텍스트가 매우 클 경우 TextField를 사용해라. 이 필드의 기본 양식 위젯은 TextInput이다.  

CharField에는 필수 인수가 있다.

**CharField.max_length**

필드의 최대 길이를 나타낸다. max_length는 데이터베이스 레벨과 장고 유효성 검사에 적용된다.

> **Note**
> 
> 여러 데이터베이스 백엔드로 이식할 수 있어야하는 응용 프로그램을 작성하는 경우 일부 백엔드의 경우 max_length에 대한 제한 사항이 있음을 알아야한다. 자세한 내용은 [database backend notes](https://docs.djangoproject.com/en/2.0/ref/databases/)를 참고


### DateField

**class DateField(auto_now=False, auto_now_add=False, \*\*options)**

Python에서 datetime.date 인스턴스로 표현되는 날짜이다. 추가로 몇가지 선택적 인수가 있다.

**DateField.auto\_now**

**객체가 저장될 때마다** 필드를 현재로 자동 설정한다. "마지막으로 수정 된 시간"이 필요할 때 유용하다. 현재 날짜는 재정의 할 수 있는 기본 값이 아니다.

이 필드는 Model.save()를 호출 할 때 자동으로 업데이트된다. 이 필드는 QuerySet.update()와 같은 다른 방법으로 다른 필드를 업데이트 할 때 업데이트 되지 않지만 이와 같은 업데이트에서 필드의 사용자 지정 값을 지정할 수 있다.

**DateField.auto\_now\_add**

**객체가 처음 생성 될 때** 자동으로 필드를 현재로 설정한다. 타임 스탬프 생성에 유용하다. 

이 필드를 수정하려면 auto\_now\_add=True 대신 다음을 설정해라.

- DateField : default=date.today  <= datetime.date.today()

- DateTimeField : default=timezone.now <= django.utils.timezone.now()

이 필드의 기본 양식 위젯은 TextInput이다. 관리자 페이지는 JavaScript 캘린더와 '오늘'에 대한 바로가기를 추가한다. invalid_date 에러 메시지 키를 추가로 포함한다.

auto_now_add, auto_now와 default 옵션은 서로 배타적이므로 함께 사용하면 오류가 발생한다.

> **Note**
> 
> 현재 구현 된 것처럼 auto_now 또는 auto_now_add를 True로 설정하면 해당 필드는 editable = False 및 blank = True로 설정된다.

---

> **Note**
> 
> auto_now 및 auto_now_add 옵션은 생성 또는 업데이트 할 때 항상 기본 시간대의 날짜를 사용한다. 다른 것이 필요한 경우 auto_now 또는 auto_now_add를 사용하는 대신 자신의 호출 가능한 기본 값을 사용하거나 save()를 재정의하는 것이 좋다. 또는 DateField 대신 DateTimeField를 사용하고 표시 시간에 datetime에서 date로의 변환을 처리하는 방법을 결정할 수 있다.

### DateTiemField

**class DateTimeField(auto_now=False, auto_now_add=False, \*\*options)**

Python에서 datetime.datetime 인스턴스로 표현되는 날짜와 시간. DateField와 동일한 추가 인수를 사용한다. 이 필드의 기본 양식 위젯은 하나의 TextInput이다. 관리자는 JavaScript 바로가기가 있는 두 개의 별도 TextInput 위젯을 사용한다.

### DecimalField

### DurationField

### EmailField

**class EmailField(max_length=254, \*\*options)**


유효한 이메일 주소인지 확인하는 CharField이다. EmailValidator를 사용하여 입력의 유효성을 검사함.

### FileField

### FilePathField

### FloatField

**class FloatField(\*\*options)**

float 인스턴스로 파이썬에서 표현되는 부동소수점 숫자다. 이 필드의 기본 양식 위젯은 localize가 False일 때 NumberInput이고 그렇지 않으면 TextInput이다.

> **FloatField vs. DecimalField**
>
> FloatField 클래스는 때때로 DecimalField 클래스와 혼합된다. 둘 다 실수를 나타내지만 그 수를 다르게 나타낸다. FloatField는 내부적으로 Python float 타입을 사용하고 DecimalField는 Python의 Decimal 타입을 사용한다.


### ImageField

**class ImageField(upload_to=None, height_field=None, width_field=None, max_length=100, \*\*options)**

FileField의 모든 속성과 메서드를 상속하지만 업로드가 된 객체가 유효한 이미지인지 확인한다. FileField에서 사용할 수 있는 특수 특성 외에도 ImageField에는 height와 width 특성이 있다. 이러한 속성에 대한 쿼리를 용이하게 하기 위해 ImageField에는 두 가지의 추가 옵션 인수가 있다.

**ImageField.height_field**

모델 인스턴스가 저장될 떄마다 이미지의 높이로 자동 채워지는 모델 필드의 이름이다.

**ImageField.width_field**

모델 인스턴스가 저장될 떄마다 이미지의 너비가 자동으로 채워지는 모델 필드의 이름이다.

**Pillow라이브러리가 필요하다**

ImageField 인스턴스는 기본 최대 길이가 100자인 varchar열로 데이터베이스에 만들어진다. 다른 필드와 마찬가지로 max_length인수를 사용하여 최대 길이를 변경할 수 있다. 이 필드의 기본 양식 위젯은 ClearableFileInput이다.

### IntegerField

**class IntegerField(\*\*options)**

정수. -2147483648에서 2147483647까지의 값은 장고가 지원하는 모든 데이터베이스에서 안전하다. 이 필드의 기본 양식 위젯은 localize가 False 일 때 NumberInput이고 그렇지 않으면 TextInput.

### GenericIPAddressField

### NullBooleanField

**class NullBooleanField(\*\*options)**

BooleanField와 NULL을 옵션 중 하나로 허용합니다. BooleanField 대신 null=True를 사용해라. 이 필드의 기본 양식 위젯은 NullBooleanSelect.

### PoisitiveIntegerField

### PositiveSmallIntegerField

### SlugField

### SmallIntegerField

### TextField

**class TextField(\*\*options)**

긴 텍스트 필드. 이 필드에 대한 기본 양식 위젯은 Textarea.

만약 max_length 속성을 명시하면 Textarea 위젯에 반영된다. 그래서 모델 또는 데이터베이스 레벨에서는 적용되지 않는다. 적용하려면 CharField를 사용해라.

### TimeField

**class TimeField(auto_now=False, auto_now_add=False, \*\*option)**

Python에서 datetime.time 인스턴스로 표현되는 시간. DateField와 동일한 자동 채우기 옵션을 적용한다. 이 필드의 기본 양식 위젯은 TextInput. 관리자 페이지에는 몇가지 JavaScript 바로가기가 추가된다.

### URLField

**class URLField(max_length=200, \*\*options)**

URL을 위한 CharField

TextInput이 기본 양식 위젯으로 사용된다.

모든 CharField의 서브클래스와 같이 URLField는 선택적 max_length 인수를 취한다. max_length를 지정하지 않으면 기본값으로 200이 들어간다.

### UUIDField




## Relationship fields(관계형 필드)






