---
layout: post
title:  "Django Documentation 2. Making queries"
date:   2018-02-19 11:30:00 +0900
categories: django
---

# Making queries

데이터 모델을 생성하면 Django는 자동으로 객체를 생성, 검색, 업데이트 및 삭제할 수있는 데이터베이스 추상화 API를 제공한다.

Weblog 모델을 예로 Query에 대해 알아볼 것이다.

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

Django는 Python 객체에서 데이터베이스 테이블 데이터를 나타내기 위해 직관적인 시스템을 사용한다. 모델 클래스는 데이터베이스 테이블을 나타내며 클래스의 인스턴스는 데이터베이스 테이블의 특정 레코드를 나타낸다.

객체를 생성하려면 키워드 인자를 사용하여 모델 클래스에 인스턴스화 한 다음 save()를 호출하여 객체를 데이터베이스에 저장한다.

모델이 mysite/blog/models.py에 있다고 가정하고 다음과 같은 예제를 실행

```
>>> from blog.models import Blog
>>> b = Blog(name='Beatles Blog', tagline='All the latest Beatles news.')
>>> b.save()
```

이것은 뒤편에서 INSERT SQL문을 수행한다. 장고는 명시적으로 save()를 호출할 때까지 데이터베이스에 접근하지 않는다. 

> 한번에 생성하고 저장하려면 create() 메소드를 사용한다.

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

> **Note:** 관리자는 모델 인스턴스가 아닌 모델 클래스를 통해서만 액세스 할 수 있으므로 "테이블 수준" 작업과 "레코드 수준" 작업을 구분할 수 있다.

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

filter()는 하나의 객체만이 query와 일치하는 경우에도 항상 QuerySet을 제공한다. 이 경우 한 개의 원소만이 들어있는 QuerySet이 될 것이다.

만약에 query와 일치하는 객체가 하나뿐인 경우를 알고 있다면 객체를 직접 반환하는 Manager에서 get()메소드를 사용하면 된다.

```
>>> one_entry = Entry.objects.get(pk=1)
```

filter()와 마찬가지로 get()과 함께 모든 쿼리 표현식을 사용할 수 있다. Field lookups를 참조해라.

get()을 사용하는 것과 [0] 슬라이스로 filter()를 사용하는 것의 차이점은 쿼리와 일치하는 결과가 없다면 get()은 DoesNotExist예외를 발생시킨다.

이 예외는 쿼리가 수행되는 모델 클래스의 속성이다. 위 코드에서 기본 키 1인 Entry객체가 없으면 장고는 Entry.DoesNotExist를 발생시킨다.

마찬가지로 장고는 하나 이상의 항목이 get()쿼리와 일치하는 경우 불만을 제기한다. 이 경우 ModelObject자체의 특성인 MultipleObjectsReturned가 발생한다.

한마디로 get() 메소드를 사용할 때는 꼭 한개의 확실한 값만 가져와야 한다는 뜻

### Other QuerySet methods

대부분 데이터베이스에서 객체를 검색해야 할 때 all(), get(), filter() 및 exclude()를 사용한다. 그러나 그것은 거기있는 모든 것과는 거리가 멀다. QuerySet메소드의 전체 목록은 [QuerySet API 레퍼런스](https://docs.djangoproject.com/en/2.0/ref/models/querysets/#queryset-api)를 참조해라.

### Limiting QuerySets

Python의 문자열 슬라이스 구문의 하위집합을 사용하여 QuerySet을 특정 수의 결과로 제한할 수 있다. SQL의 LIMIT 및 OFFSET절과 같다.

예를 들어 처음 5개의 객체를 반환할 때 **(LIMIT 5)**:

```
>>> Entry.objects.all()[:5]
```

다음은 6번째부터 10번째 객체를 반환한다. **(OFFSET 5 LIMIT 5)**:

```
>>> Entry.objects.all()[5:10]
```

음수 탐색(i.e. **Entry.objects.all()[-1]**)은 지원되지 않는다.

일반적으로 QuerySet을 슬라이스하면 새 QuerySet이 반환된다. 쿼리는 평가되지 않지만 예외로 슬라이스 문에 step의 기능이 들어가면 쿼리는 평가된다(데이터베이스에 접근한다).

예를 들어 다음과 같이 처음 10개의 객체중 짝수 번째의 객체를 꺼내오기 위해 매번 쿼리가 실행될 것이다.

```
>>> Entry.objects.all()[:10:2]
```

슬라이스된 쿼리셋으로부터 더 필터링을 하거나 정렬을 하는 것은 모호한 특성 떄문에 금지된다.

리스트가 아닌 단일 객체를 검색하려면 (예: SELECT foo FROM bar LIMIT 1) 슬라이스 대신 간단한 색인을 사용해라. 예를들어, Entry의 제목을 알파벳 순으로 정렬한 후 데이터베이스의 첫번째 항목을 반환한다.

```
>>> Entry.objects.order_by('headline')[0]
```

이 것은 다음과 완전히 같다

```
>>> Entry.objects.order_by('headline')[0:1].get()
```

만약에 일치하는 개체가 없는 경우 여기서 첫 번째 구문은 IndexError를 발생시키고 두 번째 구문은 DoesNotExist를 발생시킬 것이다. 자세한 내용은 [get()](https://docs.djangoproject.com/en/2.0/ref/models/querysets/#django.db.models.query.QuerySet.get)을 참조.

### Field lookups

필드 검색은 SQL WHERE절에서 중요한 부분을 어떻게 명시하는지에 관한 것이다. 그것들은 QuerySet 메소드 filter(), exclude() 및 get()에 대한 키워드 인자로 지정된다.

기본 검색 키워드 인자는 field__lookuptype = value형식을 취한다.(이중 밑줄)

```
>>> Entry.objects.filter(pub_date__lte='2006-01-01')
```

다음 SQL문으로 변역된다.

```sql
SELECT * FROM blog_entry WHERE pub_date <= '2006-01-01';
```

> 파이썬에는 이름과 값이 런타임에 평가되는 임의의 name-value 인수를 허용하는 함수를 정의할 수 있는 기능이 있다. 자세한 내용은 공식 파이썬 튜토리얼 [Keyword Arguements](https://docs.python.org/3/tutorial/controlflow.html#tut-keywordargs)를 참조.

찾아보기에 지정된 필드는 모델 필드의 이름이어야한다. ForeignKey의 경우는 예외로 _id 접미사로 필드 이름을 지정할 수 있다.

이 경우, value 매개 변수에는 외부 모델의 기본 키의 raw value값이 포함될 것으로 예상된다.

```
>>>Entry.objects.filter(blog_id=4)
```

잘못된 키워드 인수를 전달하면 조회함수가 TypeError를 발생시킨다.

데이터베이스 API는 약 20개의 조회 유형(lookup types)을 지원한다. 다음은 몇가지 일반적으로 사용되는 것들을 소개한다.

**exact**

완벽한 일치

```
>>> Entry.objects.get(headline__exact="Cat bites dog")
```

SQL로 번역하면

```sql
SELECT ... WHERE headline = 'Cat bites dog';
```

조회 유형을 제공하지 않으면(즉, 키워드 인수에 밑줄이 두 개 포함되어 있지 않은 경우) 조회 유형은 exact라고 가정한다.

```
>>> Blog.ojbects.get(id__exact=14)
>>> Blog.objects.get(id=14)
```

이것은 **exact** 조회가 일반적인 경우이기 때문에 편의상 사용된다.

**iexact**

case-insensitive match

```
>>> Blog.objects.get(name__iexact="beatles blog")
```

대소문자를 구분하지 않고 매치하기 때문에 "Beatles Blog", "beatles blog", "BeAtlEs BlOg" 등도 검색 결과에 나올 것이다.

**contains**

Case-sensitive한 포함여부 테스트

```python
Entry.objects.get(headline__contains='Lennon')
```

SQL문으로 번역하면

```sql
SELECT ... WHERE headline LIKE '%Lennon%';
```

case-insensitive 버전으로 **icontains**를 사용하면 된다.

또다른 검색 방법으로 **startswith**, **endswith** 그리고 대소문자 구분 없는 **istartswith**, **iendswith**이 있다. 

### Lookups that span relationships

장고는 자동으로 SQL JOIN을 처리하면서 조회시 관계들을 따르는 강력하고 직관적인 방법을 제공한다.

관계를 확장하려면 원하는 필드에 도달할 때까지 모델에서 관련 필드의 필드이름을 이중 밑줄로 구분하여 사용해라.

다음 예제는 Blog이름이 'Beatles Blog'인 모든 Entry개체를 검색한다.

```
>>> Entry.objects.filter(blog__name='Beatles Blog')
```

이 확장은 원하는 만큼 깊게 들어갈 수 있다. 역 방향으로도 가능하다. 역관계를 나타내기 위해서 모델의 소문자 이름을 사용하면 된다.

다음 예제에서 헤드라인에 'Lennon'이 포함된 Entry가 하나 이상 있는 모든 Blog객체를 검색한다.

```
>>> Blog.objects.filter(entry__headline__contains='Lennon')
```

여러 관계를 거쳐 필더링하고 중간 모델 중 하나가 필터 조건을 만족하는 값을 갖지 않으면 Django는 이를 빈 값 (모든 값이 NULL임)으로 처리하지만 유효하며 객체이다. 해당하는 값이 없어도 어쨌든 에러가 발생하지 않는다는 뜻이다.

```
Blog.objects.filter(entry__authors__name='Lennon')
```

(관련된 Author모델이 있는 경우) entry와 연관된 작성자가 없는 경우 작성자가 누락되었다고 오류가 발생하지 않고 단지 이름이 첨부되지 않은 것으로 처리한다.

일반적으로 사용자가 원하는 일일 것이다. 혼란스러울 수 있는 경우는 isnull을 사용하는 경우다.

```
Blog.objects.filter(entry__authors__name__isnull=True)
```

이 구문은 author에 빈 이름이 있는 블로그 개체와 entry에 author가 비어있는 Blog객체를 반환할 것이다. 후자의 객체를 원하지 않으면 다음과 같이 작성할 수 있다.

```
Blog.objects.filter(entry__authors__isnull=False, entry__authors__name__isnull=True)
```

**Spanning multi-valued relationships**

ManyToManyField 또는 역방향 ForeignKey를 기반으로 객체를 필터링하는 경우 두가지 종류의 필터가 존재한다.

Blog/Entry관계(one-to-many relation)를 고려해봤을 때, headline이 'Lennon'이며 2008년에 출간된 entry가 있는 블로그를 찾는다고 가정하자. 아니면, 2008년도에 출간된 entry나 'Lennon'이라는 headline이 있는 Blog를 찾고 싶을 수도 있다.
(두 가지 조건이 동시에 들어간 것을 찾는지 또는 각 각 따로 찾는지에 대한 이야기)

하나의 블로그와 관련된 여러 entry들이 있으므로 이러한 쿼리는 모두 가능하며 각 상황에서 의미가 있다.

ManyToManyField와 동일한 유형의 상황이 발생할 수 있다. 예를 들어 entry에 tags라는 ManyToManyField가 있는 경우, 우리는 '음악'과 '밴드'라는 태그에 링크된 entry를 찾고 싶거나 '음악'이라는 이름과 '공개'상태의 태그를 포함하는 항목을 원할 수도 있다.

이러한 상황을 처리하기 위해 장고는 filter() 호출을 일관성있게 처리한다. 한번의 filter()호출안에 들어있는 모든 항목이 동시에 적용되어 모든 요구사항과 일치하는 항목을 필터링한다.

연속적인 filter()호출은 객체 셋을 추가로 제한하지만 multi-valued 관계의 경우 기본 filter()호출에 의해 선택된 객체가 아닌 기본 모델에 링크된 모든 객체에 적용된다.

약간 혼란스러워질 수 있기 때문에, 예제를 통해 확실하게 알아보자. 'Lennon'이라고 쓰인 헤드라인과 2008년에 출판된 entry가 모두 포함된 블로그를 선택하려면 다음과 같이 작성하면 된다.

```python
Blog.objects.filter(entry__headline__contains='Lennon', entry__pub_date__year=2008)
```

헤드라인에 'Lennon'이 포함된 entry와 2008년에 게시된 entry가 포함된 블로그를 모두 선택하려면 다음과 같이 작성하면 된다.

```python
Blog.objects.filter(entry__headline__contains='Lennon').filter(entry__pub_date__year=2008)
```

'Lennon'과 2008년 출품을(각 각 따로) 모두 포함하는 블로그가 하나뿐이며 2008년 작 entry중 'Lennon'이 포함되지 않은 블로그가 있다고 가정해보자. 첫 번째 쿼리는 블로그를 반환하지 않지만 두 번째 쿼리는 해당 블로그를 반환한다.

그러니까 다시 정리하면 **헤드라인이 Lennen**이면서 **2008년 출품**이 동시에 걸린 entry는 없고, 해당 키워드가 서로 다른 entry에 있을 때 첫 번째 쿼리는 블로그를 찾지 못하고 두 번째 쿼리는 해당 블로그를 반환한다.

두 번째 쿼리를 다시 쉽게 말하면 헤드라인이 'Lennon'인 블로그를 찾는다. 그리고 결과에서  2008년에 작성된 entry가 있는 블로그를 다시 찾는다. 따라서 두 번째 쿼리에만 블로그 결과가 나오는 까닭이다.

> **Note**
multi-value 관계에 걸쳐있는 쿼리에 대한 filter()의 동작은 exclude()에 대해 동일하게 구현되지 않는다. 대신 하나의 exclude()호출의 조건이 반드시 동일한 항목을 참조하는 것은 아니다  
예를 들어, 다음 쿼리는 헤드라인에 'Lennon'이라는 entry와 2008년 작성을 모두 포함하는 블로그를 제외한다.

```
Blog.objects.exclude(
    entry__headline__contains='Lennon',
    entry__pub_date__year=2008,
)
```

> 그러나 filter()를 사용할 때와 달리 두 조건을 모두 충족하는 항목을 기반으로 블로그를 제한하지 않는다. 2008년에 게시된 'Lennon'으로 게시된 항목이 포함되지 않은 블로그를 모두 선택하려면 두 가지 검색어를 입력해야한다.

```
Blog.objects.exclude(
    entry__in=Entry.objects.filter(
        headline__contains='Lennon',
        pub_date__year=2008,
    ),
)
```
