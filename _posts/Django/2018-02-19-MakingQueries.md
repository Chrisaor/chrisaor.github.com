---
layout: post
title:  "Django Documentation 2. Making queries"
date:   2018-02-19 11:30:00 +0900
categories: django
---

# Making queries

데이터 모델을 생성하면 Django는 자동으로 객체를 생성, 검색, 업데이트 및 삭제할 수있는 데이터베이스 추상화 API를 제공합니다.

이 문서는이 API를 사용하는 방법을 설명합니다. 모든 다양한 모델 조회 옵션에 대한 자세한 내용은 데이터 모델 참조를 참조하십시오.

이 가이드 (및 참조) 전체에서 Weblog 응용 프로그램을 구성하는 다음 모델을 참조 할 것입니다.

```python
from django.db import models

class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

    def __str__(self):
        return self.name

class Author(models.Model):
    name = models.CharField(max_length=200)
    email = models.EmailField()

    def __str__(self):
        return self.name

class Entry(models.Model):
    blog = models.ForeignKey(Blog, on_delete=models.CASCADE)
    headline = models.CharField(max_length=255)
    body_text = models.TextField()
    pub_date = models.DateField()
    mod_date = models.DateField()
    authors = models.ManyToManyField(Author)
    n_comments = models.IntegerField()
    n_pingbacks = models.IntegerField()
    rating = models.IntegerField()

    def __str__(self):
        return self.headline
```

## Createing obejcts

Django는 Python 객체에서 데이터베이스 테이블 데이터를 나타내기 위해 직관적인 시스템을 사용합니다. 모델 클래스는 데이터베이스 테이블을 나타내며 클래스의 인스턴스는 데이터베이스 테이블의 특정 레코드를 나타냅니다.

객체를 생성하려면 키워드 인자를 사용하여 모델 클래스에 인스턴스화 한 다음 save ()를 호출하여 객체를 데이터베이스에 저장합니다.

모델이 mysite/blog/models.py에 있다고 가정하고 다음과 같은 예제를 실행

```
>>> from blog.models import Blog
>>> b = Blog(name='Beatles Blog', tagline='All the latest Beatles news.')
>>> b.save()
```

이것은 뒤편에서 INSERT SQL문을 수행한다. 장고는 명시적으로 save()를 호출할 때까지 데이터베이스에 접근하지 않는다. 한번에 생성하고 저장하려면 create() 메소드를 사용한다.

## Saving changes to objects

이미 데이터베이스에 있는 오브젝트의 변경사항을 저장하려면 save()를 이용한다.

예를 들어 이미 데이터베이스에 저장된 Blog의 인스턴스 b5가 주어졌을 때, 다음과 같이 이름을 변경하고 데이터 베이스에서 해당 레코드를 업데이트 할 수 있다.

```
>>> b5.name = 'New name'
>>> b5.save()
```

이것은 뒤에서 UPDATE SQL문을 수행한다. 마찬가지로 명시적으로 save()를 호출할 때까지 데이터 베이스에 접근하지 않는다.

### Saving ForeignKey and ManyToManyField fields

ForeignKey 필드를 업데이트한느 것은 정상 필드를 저장하는 것과 똑같은 방식으로 작동함. - 문제의 필드에 올바른 타입의 객체를 지정하기만 하면된다. 다음 예제는 Entry 및 Blog의 적절한 인스턴스가 이미 데이터베이스에 저장되었다고 가정하여 Entry인스턴스의 blog속성을 업데이트함. (아래에서 해당 항목을 검색할 수 있음)

```
>>> from blog.models import Blog, Entry
>>> entry = Entry.objects.get(pk=1)
>>> cheese_blog = Blog.objects.get(name="Cheddar Talk")
>>> entry.blog = cheese_blog
>>> entry.save()
```

ManyToManyField를 업데이트하는 것은 약간 다르게 작동한다. 필드에 add()메소드를 사용하여 관계에 레코드를 추가합니다. 이 예제에서는 Author의 인스턴스 joe를 entry객체에 추가한다.

```
>>> from blog.models import Author
>>> joe = Author.objects.create(name="Joe")
>>> entry.authors.add(joe)
```

한 번에 ManyToManyField에 여러 레코드를 추가하려면 다음과 같이 add()의 여러 인수를 넣으면 된다.

```
>>> john = Author.objects.create(name="John")
>>> paul = Author.objects.create(name="Paul")
>>> george = Author.objects.create(name="George")
>>> ringo = Author.objects.create(name="Ringo")
>>> entry.authors.add(john, paul, george, ringo)
```

장고는 잘못된 타입의 객체를 할당하거나 추가하려고하면 불평할 것이다.

## Retrieving objects

데이터베이스에서 객체를 검색하려면 모델 클래스의 Manager를 통해 QuerySet을 생성해야한다.

QuerySet은 데이터베이스의 객체 컬렉션을 나타낸다. 하나도 없을 수도 있고 하나 또는 그 이상의 필터를 가질 수 있다.

필터는 주어진 매개변수로 쿼리 결과의 범위를 좁힌다. SQL용어에서 QuerySet은 SELECT문과 같으며 필터는 WHERE 또는 LIMIT과 같은 제한을 두는 절이다.

모델의 Manager를 사용하여 QuerySet을 얻는다.  각 모델에는 하나 이상의 Manager가 있으며 기본적으로 객체라고 한다. 다음과 같이 모델 클래스를 통해 직접 액세스한다.

```
>>> Blog.objects
<django.db.models.manager.Manager object at ...>
>>> b = Blog(name='Foo', tagline='Bar')
>>> b.objects
Traceback:
    ...
AttributeError: "Manager isn't accessible via Blog instances."
```

**Note:** 관리자는 모델 인스턴스가 아닌 모델 클래스를 통해서만 액세스 할 수 있으므로 "테이블 수준" 작업과 "레코드 수준" 작업을 구분할 수 있다.

Manage는 모델에 대한 QuerySets의 주요 소스이다. 예를 들어 Blog.objects.all()은 데이터베이스의 모든 Blog객체를 포함하는 QuerySet을 반환한다.

### Retrieving all objects

테이블에서 객체를 검색하는 가장 간단한 방법은 모든 객체를 가져오는 것이다. Manager에서 all() 메소드를 사용하면된다.

```
>>> all_entries = Entry.objects.all()
```

all()메소드는 데이터베이스에 있는 모든 객체의 QuerySet을 반환한다.

### Retrieving specific objects with filters

all()에 의해 반환된 QuerySet은 데이터베이스 테이블의 모든 객체를 표시한다. 그러나 일반적으로 객체의 전체 집합중 일부만 선택하는게 보통이다.

이러한 하위 집합을 생성하려면 필터 조건을 추가하여 초기 QuerySet을 구체화해야한다. QuerySet을 구체화하는 일반적인 방법으로 2가지가 있다.

**filter(\*\*kwargs)** : 주어진 검색 매개변수와 **일치하는** 객체가 포함된 새 QuerySet을 반환한다

**exclude(\*\*kwargs)** : 주어진 검색 매개변수와 **일치하지 않는** 객체가 포함된 새 QuerySet을 반환한다.

조회 매개변수(lookup parameters, 위 함수 정의된 키워드 인자 **kwagrs)는 아래의 필드 조회에서 사용되는 형식이어야 함. 

예를 들어, 2006년 이후의 블로그 entry들의 QuerySet을 얻으려면 filter()를 다음과 같이 사용하면 된다.

```python
Entry.objects.filter(pub_date__year=2006)
```

기본 manager클래스는 다음과 같다

```python
Entry.objects.all().filter(pub_date__year=2006)
```

**Chaining filters**

QuerySet을 정제한 결과는 QuerySet 그 자체이므로 상세 검색을 연결할 수 있다


```
>>> Entry.objects.filter(
...     headline__startswith='What'
... ).exclude(
...     pub_date__gte=datetime.date.today()
... ).filter(
...     pub_date__gte=datetime.date(2005, 1, 30)
... )
```

이것은 데이터베이스의 모든 entry들에 대한 초기 QuerySet을 가져와서 필터를 추가한 다음 제외하고 다른 필터를 추가한다.
최종 결과는 2005년 1월 30일과 현재 날짜 사이에 게시된 "What"으로 시작하는 모든 entry를 포함하는 QuerySet이다.

### Filtered QuerySets are unique

QuerySet을 정제할 때마다, 이전의 생성된 QuerySet과는 독립적인 새로운 QuerySet을 갖게된다. 각각의 상세검색은 저장되고 사용되며 재사용될 수 있는 별개의 고유한 QuerySet을 생성한다.

```
>>> q1 = Entry.objects.filter(headline__startswith="What")
>>> q2 = q1.exclude(pub_date__gte=datetime.date.today())
>>> q3 = q1.filter(pub_date__gte=datetime.date.today())
```

이 세 가지 QuerySet은 각각 별개이다. 첫 번째는 "What"으로 시작하는 제목의 모든 entry들을 나타내는 QuerySet이다. 두 번째는 첫 번째 항목의 하위 집합이며 pub_date가 현재 또는 미래의 레코드를 제외하는 조건이 추가된 검색이다. 세 번째 또한 첫 번째 항목의 하위 집합이며 pub_date가 현재 또는 미래의 레코드만 선택하는 추가 조건이 있다. 초기의 QuerySet(q1)은 데이터가 정제되는 프로세스의 영향을 받지 않는다.


### QuerySets are lazy

QuerySet은 게으르다 - QuerySet을 생성하는 행위는 어떤 데이터베이스 활동도 포함하지 않는다. 하루 종일 필터를 함께 쌓을 수 있으며, 장고는 QuerySet이 평가 될 때까지 실제로 쿼리를 실행하지 않는다.

```
>>> q = Entry.objects.filter(headline__startswith="What")
>>> q = q.filter(pub_date__lte=datetime.date.today())
>>> q = q.exclude(body_text__icontains="food")
>>> print(q)
```

위 예제는 3번의 데이터베이스에 접근하는 것 처럼 보이지만 마지막 행(print(q))에서 데이터베이스에 한번만 접근한다. 일반적으로 QuerySet의 결과는 사용자가 **"요청"**할 때까지 데이터베이스에서 가져오지 않는다.

### Retrieving a single object with get()













