---
layout: post
title:  "[The model layer] Meta options"
date:   2018-06-21 11:30:00 +0900
categories: django
---

구글 번역기의 도움을 (많이) 받아 장고 공식 문서를 번역하였습니다.

---

이 문서는 모델의 메타 클래스 내부에 줄 수 있는 모든 가능한 메타데이터 옵션을 설명함.

Available **Meta** options

### abstract

**Options.abstract**

만약 abstract = True라면 이 모델은 abstract base class이다.

### app_label

**Options.app_label**

모델이 INSTALLED_APPES에서 애플리에키션 외부에 정의된 경우 해당 모델이 속한 앱을 선언해야한다.

```
app_label = 'myapp'
```

app_label.object_name 또는 app_label.model_name 형식으로 모델을 나타내려면 model._meta.label 또는 model._meta.label_lower를 각각 사용할 수 있다.

### get_latest_by

**Options.get_latest_by**

모델의 필드 이름 목록 또는 필드 이름(일반적으로 DateField, DateTimeField 또는 IntegerField)이다. 이 값은 모델 관리자의 latest() 및 earliest() 메소드에서 사용할 기본 필드를 지정한다.

예를 들어:

```python
# Latest by ascending order_date.
get_latest_by = "order_date"

# Latest by priority descending, order_date ascending.
get_latest_by = ['-priority', 'order_date']
```

### order\_with\_respect\_to

**Option.order\_with\_respect\_to**

이 객체를 지정된 필드 (일반적으로 ForeignKey)에 대해서 순서 붙일 수 있게 한다. 이것은 부모 객체와 관련하여 관련 객체를 정렬 할 수 있게 하는데 사용할 수 있다. 예를 들어, Answer이 Question 객체와 관련이 있고 Question에 둘 이상의 Answer이 있고 Answer의 순서가 중요한 경우 다음을 수행하면 된다.

```python
from django.db import models

class Question(models.Model):
    text = models.TextField()
    # ...

class Answer(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    # ...

    class Meta:
        order_with_respect_to = 'question'
```

order_with_respect_to가 설정되면 관련 객체의 순서를 검색하고 설정하는 두 가지 메소드가 제공된다. Get_RELATED_order() 및 set_RELATED_order(). 여기서 RELATED는 소문자로 된 모델 이름이다. 예를 들어, Question 객체에 여러개의 관련 Answer 객체가 있다고 가정하면 반환되는 목록에는 관련 Answer객체의 기본 키가 포함된다.

```
>>> question = Question.objects.get(id=1)
>>> question.get_answer_order()
[1, 2, 3]
```

Question 객체의 관련 Answer 객체 순서는 Answer 기본 키 목록을 전달하여 설정할 수 있다.

```
>>> question.set_answer_order([3, 1, 2])
```

관련 객체에는 get_next_in_order() 및 get_previous_in_order()의 두 가지 메소드가 있다. 이 메소드는 적절한 순서로 객체에 액세스하는 데 사용할 수 있다. Answer객체가 id에 의해 정렬되었다고 가정한다.

```
>>> answer = Answer.objects.get(id=2)
>>> answer.get_next_in_order()
<Answer: 3>
>>> answer.get_previous_in_order()
<Answer: 1>
```

### ordering

**Options.ordering**

객체의 기본 순서는 객체 목록을 가져올 때 사용한다.

```python
ordering = ['-order_date']
```

문자열 및/또는 쿼리식으로 된 튜플 또는 리스트이다. 문자열 앞에 "-"가 붙어있다면 내림차순이고 "?"가 붙어있다면 무작위로 정렬된다. 기본은 오른차순. 예를 들어 pub_date 필드를 오름차순으로 정렬하려면 다음과 같이 쓰면 된다.

```python
ordering = ['pub_date']
```

내림차순으로 정렬하려면

```python
ordering = ['-pub_date']
```

pub_date는 내림차순으로 하고 author는 오름차순으로 하려면

```python
ordering = ['-pub_date', 'author']
```

쿼리 식을 사용할 수도 있다. Author가 오름차순으로 되어있고 null값은 마지막으로 두려면 다음과 같이 쓴다.

```python
from django.db.models import F

ordering = [F('author').asc(nulls_last=True)]
```

### indexes

**Options.indexes**

모델에 정의하려는 인덱스 목록:

```python
from django.db import models

class Customer(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)

    class Meta:
        indexes = [
            models.Index(fields=['last_name', 'first_name']),
            models.Index(fields=['first_name'], name='first_name_idx'),
        ]
```

### unique_together

**Options.unique_together**

합께 취해진 필드 이름 셋은 고유해야한다.

```python
unique_together = (("driver", "restaurant"),)
```

함께 고려할 때 고유해야하는 튜플의 튜플이다. 장고 관리자에서 사용되며 데이터베이스 수준에서 적용된다. (즉, 적절한 UNIQUE 문이 CREATE TABLE문에 포함됨)

편의상 unique_together는 단일 필드 집합을 처리할 때 단일 튜플이 될 수 있다.

```python
unique_together = ("driver", "restaurant")
```

ManyToManyField는 unique_together에 포함될 수 없다. (이것이 의미하는 바는 분명하지 않다)
ManyToManyField와 관련된 unique 유효성을 검사해야 하는 경우 signal또는 명시적 through모델을 사용해라.

제약 조건 위반 시 모델 유효성 검사 중에 일어난 ValidationError는 unique_together 오류 코드를 갖는다.

### index_together

**Option.index_together**

> 대신 indexes옵션을 사용해라
> 
> 새로운 indexes 옵션은 index\_together보다 많은 기능을 제공한다. index\_together는 향후 사용되지 않을 수 있다.

함께 취한 필드 이름 집합이 인덱싱된다.

```python
index_together = [
    ["pub_date", "deadline"],
]
```

이 필드 목록은 함께 색인이 생성된다. (즉, 적절한 CREATE INDEX문이 수행된다) 편의상 index_together는 단일 필드 집합을 처리할 때 단일 목록이 될 수 있다.

```python
index_together = ["pub_date", "deadline"]
```

### verbose_name

**Options.verbose_name**

객체를 사람이 읽을 수 있도록 표현하다. 단수형

```python
verbose_name = "pizza"
```

이것이 주어지지 않으면 알아서 변형시킨다. `CamelCase`는 `camel case`가 됨

### verbose_name_plural

**Option.verbose\_name\_plural**

객체의 복수형 이름

```python
verbose_name_plural = "stories"
```

주어지지 않으면 장고는 verbose_name에 s를 붙인 것으로 사용한다.







