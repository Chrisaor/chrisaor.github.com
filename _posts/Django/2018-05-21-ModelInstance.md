---
layout: post
title:  "[The model layer] Instance methods"
date:   2018-05-21 11:30:00 +0900
categories: django
---

구글 번역기의 도움을 (많이) 받아 장고 공식 문서를 번역하였습니다.

# Model instance reference

이 문서는 Model API의 세부 사항을 기술한다. 모델 및 데이터베이스 쿼리 가이드에 제시된 내용을 토대로 작성되었으므로 이 문서를 읽기 전에 해당 문서를 읽고 이해하는 것이 좋다.

이 레퍼런스를 통해 우리는 데이터 베이서 쿼리 가이드에 제시된 Weblog모델을 사용할 것이다.

## 객체 생성하기

모델의 새 인스턴스를 만들려면 다른 파이썬 클래스 처럼 인스턴스를 생성하면 된다.

**class Model(**kwargs)**

키워드 인수는 단순히 모델에 정의한 필드의 이름이다. 모델을 인스턴스화 하는 것은 **save()**를 하지 않는 이상 절대로 데이터베이스를 건드리지 않는다.

> **Note** 
> 
> \_\_init\_\_메소드를 오버라이드하여 모델을 커스터마이즈 할 수 있다.그러나 그렇게하면 모델 인스턴스의 변경사항이 저장되지 않을 수 있으므로 호출 서명(calling signature)을 변경하지 않도록 주의해야한다. \_\_init\_\_을 오버라이드하는 대신 다음 방법 중 하나를 사용해보자

> 1. 모델 클래스에 클래스메소드를 추가한다.
 

```python
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=100)

    @classmethod
    def create(cls, title):
        book = cls(title=title)
        # do something with the book
        return book
        
book = Book.create("Pride and Prejudice")>
```

> 2. 커스텀 메니저에 메소드를 추가한다.(일반적으로 선호되는 방법)

```python
class BookManager(models.Manager):
    def create_book(self, title):
        book = self.create(title=title)
        # do something with the book
        return book

class Book(models.Model):
    title = models.CharField(max_length=100)

    objects = BookManager()

book = Book.objects.create_book("Pride and Prejudice")
```

### 모델 로딩 커스터마이징

classmethod Model.from\_db(db,field\_names, values)

**from_db()** 메소드를 사용하여 데이터베이로부터 로딩할 때 모델 인스턴스 생성을 정의할 수 있다.

db 인수는 모델이 로드된 데이터베이스의 alias를 포함하고 field_names는 로드된 모든 필드의 이름을 포함하고 값은 field_names에 로드된 값을 포함한다.
field_names는 값과 동일한 순서로 있다. 모델의 모든 필드가 존재하면, 값은 \_\_init\_\_가 기대하는 순서로 보장된다. 즉, 인스턴스는 cls(*값)에 의해 생성될 수 있다.


필드가 지연되면 field_names에 나타나지 않는다. 이 경우 누락된 각 필드에 django.db.models.DEFERRED 값을 할당하면 된다.
새 모델을 만드는 것 외에도 from_db()메소드는 새 인스턴스의 \_state 특성에 추가 및 db 플래그를 설정해야한다. 다음은 데이터베이스에서 로드된 필드의 초기 값을 기록하는 방법을 보여주는 예이다.

```python
from django.db.models import DEFERRED

@classmethod
def from_db(cls, db, field_names, values):
    # Default implementation of from_db() (subject to change and could
    # be replaced with super()).
    if len(values) != len(cls._meta.concrete_fields):
        values = list(values)
        values.reverse()
        values = [
            values.pop() if f.attname in field_names else DEFERRED
            for f in cls._meta.concrete_fields
        ]
    instance = cls(*values)
    instance._state.adding = False
    instance._state.db = db
    # customization to store the original field values on the instance
    instance._loaded_values = dict(zip(field_names, values))
    return instance

def save(self, *args, **kwargs):
    # Check how the current values differ from ._loaded_values. For example,
    # prevent changing the creator_id of the model. (This example doesn't
    # support cases where 'creator_id' is deferred).
    if not self._state.adding and (
            self.creator_id != self._loaded_values['creator_id']):
        raise ValueError("Updating the value of creator isn't allowed")
    super().save(*args, **kwargs)
```

위의 예제는 전체 from_db()구현을 보여준다. 이 경우 from_db() 메소드에서 super()호출 만 사용하는 것이 가능하다.

## Refreshing objects from database

모델 인스턴스에서 필드를 삭제하면 다시 액세스하여 데이터베이스에서 값을 다시 로드한다.

```
>>> obj = MyModel.objects.first()
>>> del obj.field
>>> obj.field # 데이터베이스로부터 필드를 로드함
```

#### Model.refresh\_form\_db(using=None, fields=None)

모델 값을 데이터베이스에서 다시 로드해야하는 경우 refresh\_from\_db() 메소드를 사용할 수 있다. 인수 없이 이 메소드를 호출하면 다음 작업이 수행된다.

1. 모델의 모든 지연되지 않은 필드는 현재 데이터베이스에 있는 값으로 업데이트된다.
2. 관계의 값이 더 이상 유효하지 않은 이전에 로드된 관련 인스턴스가 다시 로드된 인스턴스에서 제거된다. 예를 들어, 다시 로드된 인스턴스에서 작성자 이름의 다른 모델로 foreign key가 있는 경우 obj.author\_id != obj.author.id, obj.author가 버려지고 다음 액세스 시 obj.author\_id의 값이 다시 로드된다. 

모델에 필드만 데이터베이스에서 다시 로드된다. 주석과 같은 다른 데이터베이스 종속 값은 다시 로드되지 않는다. 모든 @cached_property 속성은 지워지지 않는다.

리로딩(reloading)은  인스턴스가 로드된 데이터베이스에서 수행되거나 로드되지 않은 경우 기본 데이터베이스에서 수행된다. **using**인수는 리로딩에 사용된 데이터베이스를 강제로 실행하는 데 사용될 수 있다.

fields 인수를 사용하여 로드할 필드셋을 강제 실행할 수 있다.

예를 들어, update() 호출이 예상된 업데이트를 일으킨다는 것을 테스트하려면 다음과 비슷한 테스트를 작성할 수 있다.

```python
def test_update_result(self):
    obj = MyModel.objects.create(val=1)
    MyModel.objects.filter(pk=obj.pk).update(val=F('val') + 1)
    # At this point obj.val is still 1, but the value in the database
    # was updated to 2. The object's updated value needs to be reloaded
    # from the database.
    obj.refresh_from_db()
    self.assertEqual(obj.val, 2)
```

