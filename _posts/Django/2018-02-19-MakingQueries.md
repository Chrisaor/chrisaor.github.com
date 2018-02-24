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

--
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

--
### Filtered QuerySets are unique

QuerySet을 정제할 때마다, 이전의 생성된 QuerySet과는 독립적인 새로운 QuerySet을 갖게된다. 각각의 상세검색은 저장되고 사용되며 재사용될 수 있는 별개의 고유한 QuerySet을 생성한다.

```
>>> q1 = Entry.objects.filter(headline__startswith="What")
>>> q2 = q1.exclude(pub_date__gte=datetime.date.today())
>>> q3 = q1.filter(pub_date__gte=datetime.date.today())
```

이 세 가지 QuerySet은 각각 별개이다. 첫 번째는 "What"으로 시작하는 제목의 모든 entry들을 나타내는 QuerySet이다. 두 번째는 첫 번째 항목의 하위 집합이며 pub_date가 현재 또는 미래의 레코드를 제외하는 조건이 추가된 검색이다. 세 번째 또한 첫 번째 항목의 하위 집합이며 pub_date가 현재 또는 미래의 레코드만 선택하는 추가 조건이 있다. 초기의 QuerySet(q1)은 데이터가 정제되는 프로세스의 영향을 받지 않는다.

--
### QuerySets are lazy

QuerySet은 게으르다 - QuerySet을 생성하는 행위는 어떤 데이터베이스 활동도 포함하지 않는다. 하루 종일 필터를 함께 쌓을 수 있으며, 장고는 QuerySet이 평가 될 때까지 실제로 쿼리를 실행하지 않는다.

```
>>> q = Entry.objects.filter(headline__startswith="What")
>>> q = q.filter(pub_date__lte=datetime.date.today())
>>> q = q.exclude(body_text__icontains="food")
>>> print(q)
```

위 예제는 3번의 데이터베이스에 접근하는 것 처럼 보이지만 마지막 행(print(q))에서 데이터베이스에 한번만 접근한다. 일반적으로 QuerySet의 결과는 사용자가 **"요청"**할 때까지 데이터베이스에서 가져오지 않는다.

--
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

--
### Other QuerySet methods

대부분 데이터베이스에서 객체를 검색해야 할 때 all(), get(), filter() 및 exclude()를 사용한다. 그러나 그것은 거기있는 모든 것과는 거리가 멀다. QuerySet메소드의 전체 목록은 [QuerySet API 레퍼런스](https://docs.djangoproject.com/en/2.0/ref/models/querysets/#queryset-api)를 참조해라.

--
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

--
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

--
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

--
### Filters can reference fileds on the model

지금까지는 모델 필드의 값과 상수를 비교하는 필터를 만들었다. 그러나 모델 필드의 값을 동일한 모델의 다른 필드와 비교하려면?

장고는 이러한 비교를 허용하는 F 식을 제공한다. F()의 인스턴스는 쿼리 내의 모델 필드에 대한 참조로 사용된다. 이 참조를 쿼리 필터에 사용하여 동일한 모델 인스턴스의 두 개의 다른 필드 값을 비교할 수 있다.

예를 들어, pingback보다 많은 코멘트가 있는 모든 블로그 entry목록을 찾으려면 pingback 수를 참조하는 F()객체를 생성하고 쿼리에서 해당 F() 객체를 사용하면 된다.

여기서 pingback은 다른 블로그에서 사용자의 콘텐츠로 연결되는 페이지를 다시 게시할 때 블로그 프로그램에 수신되는 알림이다. 즉, 퍼가기 같은 걸 했을 때 원글 주인에게 주는 알림.

```
>>> from django.db.models import F
>>> Entry.objects.filter(n_comments__gt=F('n_pingbacks'))
```

장고는 상수 및 다른 F()객체와 함께 F()객체를 사용하여 더하기, 빼기, 곱하기, 나누기, 나머지 및 지수 산술을 지원한다. pingback보다 2배 많은 코멘트가 포함된 모든 블로그 entry를 찾으려면 쿼리를 다음과 같이 수정하면 된다.

```
>>> Entry.objects.filter(n_comments__gt=F('n_pingbacks') * 2)
```

pingback 카운트와 코멘트 카운트의 합보다 작은 모든 entry를 찾으려면 다음과 같은 쿼리를 실행한다.

```
>>> Entry.objects.filter(rating__lt=F('n_comments') + F('n_pingback'))
```

이중 밑줄 표기법을 통해 F()객체의 관계를 확장할 수도 있다. 이중 밑줄이 있는 F()객체는 관련 객체에 액새스하는데 필요한 JOIN을 진행한다.

예를 들어, 작성자 이름이 블로그 이름과 동일한 entry를 검색하려면 다음과 같은 쿼리를 작성할 수 있다.

```
>>> Entry.objects.filter(authors__name=F('blog__name'))
```

날짜 및 날짜/시간 필드의 경우 timedelta객체를 더하거나 뺄 수 있따. 다음은 게시된 후 수정한지 3일 이상된 모든 항목을 반환한다.

```
>>> from datetime import timedelta
>>> Entry.objects.filter(mod_date__gt=F('pub_date') + timedelta(days=3))
```

F()객체는 .bitand(), .bitor(), .bitrightshift() 및 .bitleftshift()에 의한 비트연산을 지원한다.

```
>>> F('somefield').bitand(16)
```

--
### The pk lookup shortcut

편의상 장고는 'primary key'를 나타내는 pk조회 바로가기를 제공한다. 예제 블로그 모델에서 기본 키는 id필드이므로 세 구문은 동일하다.

```
>>> Blog.objects.get(id__exact=14) # Explicit form
>>> Blog.objects.get(id=14) # __exact is implied
>>> Blog.objects.get(pk=14) # pk implies id__exact
```

pk의 사용은 __exact 쿼리에만 국한되는 것이 아니다. 쿼리 용어를 pk와 결합하여 모델의 기본 키에 대한 쿼리를 수행할 수 있다.

```
# Get blogs entries with id 1, 4 and 7
>>> Blog.objects.filter(pk__in=[1,4,7])

# Get all blog entries with id > 14
>>> Blog.objects.filter(pk__gt=14)
```

pk 조회는 조인 전체에서 작동한다. 예를 들어, 다음 세 문장은 같다

```
>>> Entry.objects.filter(blog__id__exact=3) # Explicit form
>>> Entry.objects.filter(blog__id=3)        # __exact is implied
>>> Entry.objects.filter(blog__pk=3)        # __pk implies __id__exact
```

--
### Escaping percent signs and underscores in LIKE statements

LIKE 문과 같은 필드 조회(iexact, contains, icontains, startswith, istartswith, endswith 그리고 iendwith)는 두 개의 특수문자 %, _ 를 자동으로 이스케이프 한다.(LIKE 문에서 % 기호는 다중 문자 와일드카드를 나타내며 밑줄은 단일 문자 와일드카드를 나타낸다)

이는 직관적으로 작동해야 함을 의미하므로 추상화가 누출되지 않는다. 예를 들어 %기호가 포함된 모든 항목을 검색하려면 %기호를 다른 문자로 사용해야한다.

```
>>> Entry.objects.filter(headline__contains='%')
```

Django takes care of the quoting for you; the resulting SQL will look something like this:

```sql
SELECT ... WHERE headline LIKE '%\%%';
```

밑줄에 대해서도 동일하다. %기호와 _은 모두 투명하게 처리됨.

--
### Caching and QuerySets

각 QuerySet에는 데이터베이스 액세스를 최소화하기 위한 캐시가 포함되어있다. 어떻게 작동하는지 이해하면 가장 효율적인 코드를 작성할 수 있다.

새로 생성된 QuerySet에서 캐시는 비어있다. QuerySet이 처음 평가될 때 (이때, 데이터베이스 쿼리가 발생한다) 장고는 쿼리 결과를 QuerySet의 캐시에 저장하고 명시적으로 요청된 결과를 반환한다.(예: QuerySet이 반복되는 경우 다음 요소로 넘어가기)
QuerySet의 후속 평가는 캐시된 결과를 다시 사용한다.

이 캐싱 동작을 잘 알아두어야 한다. QuerySets를 올바르게 사용하지 않으면 쿼라기 제대로 작동하지 않을 수 있다. 예를 들면 다음 두 개의 QuerySets를 만들고 평가한 다음 결과를 버린다.

```
>>> print([e.headline for e in Entry.objects.all()])
>>> print([e.pub_date for e in Entry.objects.all()])
```

즉, 동일한 데이터베이스 쿼리가 두 번 실행되어 데이터베이스 로드가 효과적으로 배가된다. 또한 두 리스트에 같은 데이터베이스 레코드가 포함되지 않을 수도 있다. 왜냐하면 두 요청 사이에 Entry에 무언가 추가되거나 삭제되었을 수도 있기때문이다.

이를 방지하기 위해서 단순히 QuerySet을 저장하고 다시 사용한다.

```
>>> queryset = Entry.objects.all()
>>> print([p.headline for p in queryset]) # Evaluate the query set.
>>> print([p.pub_date for p in queryset]) # Re-use the cache from the evaluation.
```

**When QuerySets are not cached**

QuerySet은 항상 결과를 캐시하지 않는다. QuerySet의 일부만 평가할 때 캐시가 검사되지만, 결과값에 변동이 없는 경우 다음 쿼리에서 반환되는 항목은 캐시되지 않습니다. 특히, 이는 배열 슬라이스나 인덱스를 사용하여 QuerySet을 제한해도 캐시가 새롭게 채워지지 않는다.

예를 들어, QuerySet 객체에서 반복적으로 특정 인덱스를 얻으면 매번 데이터베이스를 조회하게된다.

```
>>> queryset = Entry.objects.all()
>>> print(queryset[5]) # Queries the database
>>> print(queryset[5]) # Queries the database again
```

그러나, 전체 QuerySet이 이미 평가가 되었다면, 캐시를 대신 체크한다.

```
>>> queryset = Entry.objects.all()
>>> [entry for entry in queryset] # Queries the database
>>> print(queryset[5]) # Uses cache
>>> print(queryset[5]) # Uses cache
```

다음은 전체 QuerySet을 평가하여 캐시를 채우는 다른 작업의 예이다.

```
>>> [entry for entry in queryset]
>>> bool(queryset)
>>> entry in queryset
>>> list(queryset)
```

> Note: 단순히 QuerySet을 print하는 것으로 캐시가 채워지는 것은 아니다. 이는 \_\_repr\_\_()을 호출하면 전체 QuerySet의 일부만 반환되기 때문이다.


## Complex lookups with Q objects

filter()등의 키워드 인자 쿼리는 'AND'로만 사용이된다. 만약에 OR문이 필요한 경우처럼 조금 더 복잡한 쿼리를 실행해야 할 때 Q 객체를 사용하면 된다.

Q객체는 키워드 인수 모음을 캡슐화하는데 사용되는 객체이다. 이러한 키워드 인수는 위의 "Field lookups"에서처럼 지정된다.

예를 들어, Q객체는 하나의 LIKE query를 캡슐화한다.

```python
from django.db.models import Q
Q(question__startswith='What')
```

Q객체는 &와 \|를 사용하여 결합될 수 있다. 연산자가 두개의 Q객체에 사용되면 새로운 Q객체가 생성된다.

예를 들어 이 구문은 두개의 "question__startswith"쿼리의 OR을 나타내는 하나의 Q객체를 생성한다.

```python
Q(question__startswith='Who') | Q(question__startswith='What')
```

위 구문은 다음 SQL WHERE절과 같다.

```sql
WHERE question LIKE 'Who%' OR question LIKE 'What%'
```
Q오브젝트를 &와 |로 결합하여 복잡한 구문을 만들 수 있다. 또한 Q 객체는 ~ 연산자를 사용해 부정을 나타낼 수 있기 때문에 일반 쿼리와 부정(NOT) 쿼리를 결합한 조회가 가능하다.

```python
Q(question__startswith='Who') | ~Q(pub_date__year=2005)
```

키워드 인자(예: filter(), exclude(), get())를 사용하는 각 조회 함수는 하나 이상의 Q객체를 위치 인자로 전달할 수도 있습니다. 조회함수에 여러개의 Q객체 인수를 제공하면 인자는 "AND"로 묶인다.

```python
Poll.objects.get(
    Q(question__startswith='Who'),
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6))
)
```

SQL로 번역하면

```sql
SELECT * from polls WHERE question LIKE 'Who%'
    AND (pub_date = '2005-05-02' OR pub_date = '2005-05-06')
```

조회 함수는 Q오브젝트와 키워드 인자를 동시에 받을 수 있다. 조회 함수에 제공된 모든 인수(키워드 인자 또는 Q 객체)는 "AND"로 묶인다. 주의할 점은 Q객체가 먼저 쓰이고나서 그 뒤에 키워드 인자를 받아야한다.

```python
Poll.objects.get(
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)),
    question__startswith='Who',
)
```

절대 아래처럼 쓰면 안된다.

```python
# INVALID QUERY
Poll.objects.get(
    question__startswith='Who',
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6))
)
```

-- 

## Comparing objects

두 개의 모델 인스턴스를 비교하려면 표준 파이썬 비교연산자인 ==을 쓰면 된다. 이면에서는 두 모델의 primary key값을 비교한다.

Entry예제를 써서 비교해보면 다음과 같다.

```
>>> some_entry == other_entry
>>> some_entry.id == other_entry.id
```

모델의 기본 키가 id가 아닌 경우에도 문제가 없다. 비교할 때는 항상 기본키를 사용한다. 예를 들어, 모델의 primary key필드의 이름이 name일 경우에도, 위 구문은 동일하다.

```
>>> some_obj == other_obj
>>> some_obj.name == other_obj.name
```

## Deleting objects

delete 메소드는 편리하게 delete()로 쓴다. 이 메소드는 즉시 객체를 삭제하고 삭제된 개체의 수와 객체 타입 마다 삭제된 숫자로 된 딕셔너리를 반환한다.

```
>>> e.delete()
(1, {'weblog.Entry': 1})
```

**(삭제된 객체수, {객체의 타입: 그 타입의 수})**

객체를 대량으로 삭제할 수도 있다. 모든 QuerySet에는 해당 QuerySet의 모든 멤버를 삭제하는 delete()메소드가 존재한다.

예로 pub_date 연도가 2005인 모든 Entry객체를 삭제해보자.

```
>>> Entry.objects.filter(pub_date__year=2005).delete()
(5, {'webapp.Entry': 5})
```

가능할 때마다 SQL에서 순전히 실행될 것이므로 각 각의 객체 인스턴스의 delete()메소드가 반드시 프로세스 중에 호출되지는 않는다는 것을 명심해야한다.

모델 클래스에 커스텀 delete()메소드를 다시 쓰고 그 것을 호출하려면 QuerySet의 대량 delete()메소드를 사용하는 대신 QuerySet을 반복하고 각 각체에서 delete()를 호출하여 해당 모델의 인스턴스를 "수동으로" 삭제해야한다.

객체를 삭제할 때 장고는 기본적으로 ON DELETE CASCADE SQL동작을 따라한다(?). 다시 말하면, 삭제 될 객체를 가리키는 foreign key를 객체는 함께 삭제된다. 

```python
b = Blog.objects.get(pk=1)
# 이 구문은 Blog와 그 모든 Entry객체들을 삭제할 것이다.
b.delete()
```

이 cascade 동작은 ForeignKey에 대한  on_delete인수를 통해 사용자 정의할 수 있다.

delete()는 Manager 자체에서 숨겨진 유일한 QuerySet 메서드임. 이는 실수로 Entry.objects.delete()를 요청하지 않고서는 모든 항목을 삭제하지 못하게 하는 방어책이다.

그러니까 모든 객체를 정말 지우려면, 명시적으로 전체 쿼리셋을 먼저 부르고 실행해야한다.

```python
Entry.objects.all().delete()
```

## Copying model instances

모델 인스턴스를 복사하는 방법은 따로 없지만 모든 필드의 값을 복사해서 새 인스턴스를 만드는건 쉽게 할 수 있다. 가장 단순하게 pk를 없음으로 설정할 수 있다. (pk를 없애면 데이터베이스에 들어가지 않으니까?)

```python
blog = Blog(name='My blog', tagline='Blogging is easy')
blog.save() # blog.pk == 1

blog.pk = None
blog.save() # blog.pk == 2
```

상속을 사용하면 상황은 좀 더 복잡해진다. 그냥 블로그의 하위 클래스를 고려해라

```python
class ThemeBlog(Blog):
	theme = models.CharField(max_length=200)
	
django_blog = ThemeBlog(name='Django', tagline='Django is easy',theme='python')
django_blog.save() # django_blog.pk == 3
```

상속이 작동하는 방법에 따라서 pk와 id를  None으로 설정해야한다.

```python
django_blog.pk = None
django_blog.id = None
django.blog.save() # django_blog.pk == 4
```

이 프로세스는 모델의 데이터베이스 테이블에 포함되지 않은 관계를 복사하지 않는다. 예를 들어, Entry에는 Author에 대한 ManyToManyField가 있지만 Entry를 복사한 다음에 새로운 Entry에 대해 many-to-many 관계를 설정해야한다.

```python
entry = Entry.objects.all()[0] # 이전의 생성된 entry
old_authors = entry.authors.all()
entry.pk = None
entry.save()
entry.authors.set(old_authors)
```

OneToOneField의 경우, 일대일 고유 제한 조건을 위반하지 않도록 관련 개체를 복사하고, 이를 세 객체 필드에 할당해야한다. 예를 들어, entry가 이미 위와 같이 복사되었다고 가정한다면

```python
detail = EntryDetail.objects.all()[0]
detail.pk = None
detail.entry = entry
detail.save()
```

## Updating multiple objects at once

때로는 QuerySet의 모든 객체에 대해 필드를 특정 값으로 설정하고 싶을 수도 있다. update() 메소드를 사용하면 된다.

```python
# pub_date가 2007년인 모든 헤드라인을 업데이트함
Entry.objects.filter(pub_date__year=2007).update(headline='Everything is the same')
```

이 방법을 사용하여 비 관계 필드(non-relation fields)와 ForeignKey필드만 설정할 수 있다. 비 관계 필드를 업데이트하려면 새 값을 상수로 제공해야한다. ForeignKey필드를 업데이트하려면 새 값을 가리킬 새 모델 인스턴스로 설정해라.

```
>>> b = Blog.objects.get(pk=1)

# b(Blog객체)에 속하도록 모든 entry를 업데이트함
>>> Entry.objects.all().update(blog=b)
```

update() 메소드는 즉시 적용되며 쿼리와 일치하는 행의 수를 반환한다. (일부 행에 이미 새 값이 있는 경우 업데이트된 행 수와 다를 수 있다)

업데이트 되는 QuerySet에 대한 유일한 제한은 하나의 데이터베이스 테이블(모델의 메인 테이블)에만 액세스 할 수 있다는 것이다.

관련 필드를 기준으로 필터링할 수 있지만 모델의 메인 테이블의 column만 업데이트 할 수 있다.

```
>>> b = Blog.objects.get(pk=1)

# 이 블로그에 속한 모든 헤드라인을 업데이트한다
>>> Entry.objects.select_related().filter(blog=b).update(headline='Everything is the same')
```

update()메소드는 SQL문으로 직접 변환된다. 직접 업데이트를 위한 대량 작업이다.

모델에서 save() 메소드를 실행하지 않거나, save()를 호출한 결과인 pre_save 또는 post_save 시그널은 내보내거나, auto_now필드 옵션을 사용한다.

QuerySet의 모든 항목을 저장하고 각 인스턴스에서 save()메소드가 호출되도록 하는 특별한 함수는 없다. 그냥 반복문으로 불러와서 save()해야한다.

```python
for item in my_queryset:
	item.save()
```

업데이트 호출은 F 식을 사용하여 모델의 다른 필드 값을 기반으로 한 필드를 업데이트 할 수도 있다.

이것을 현재 값을 기반으로 카운터를 증가시킬 때 특히 유용하다. 예를 들어, 블로그의 모든 entry에 대해 pingback의 수를 늘리려면 다음과 같이 한다.

```
>>> Entry.objects.all().update(n_pingbacks=F('n_pingbacks') + 1)
```

그러나, filter나 exclude절에서의 F()객체와 달리 업데이트에서 F()객체를 사용할 때 join을 도입할 수는 없다. 업데이트 중인 모델의 로컬 필드만 참조할 수 있다.

F() 객체를 사용하여 조인을 진행하려고 하면 FieldError가 발생한다.

```
# FieldError가 발생하는 예제
>>> Entry.objects.update(headline=F('blog__name'))
```

## Related objects

ForeignKey, OneToOneField 또는 ManyToManyField같은 모델에서 관계를 정의하면 해당 모델의 인스턴스는 관련 객체에 액세스하기 위한 편리한 API를 갖게된다.

예를 들어, Entry객체 e는 블로그 속성인 e.blog에 액세스하여 관련 Blog객체를 가져올 수 있다. (이면에서는 이 기능은 파이썬 descriptor에 의해 구현된다)

장고는 관계의 "다른" 측면인 관련 모델에서 관계를 정의하는 모델에 대한 링크인 API accessors를 만든다.

예를 들어, Blog객체 b는 entry_set 속성(b.entry_set.all())을 통해 관련된 모든 Entry객체의 목록에 액세스할 수 있다.

### One-to-many relationships

**Forward**

모델에 ForeignKey가 있는 경우, 해당 모델의 인스턴스는 모델의 단순 속성을 통해 관련(foreign) 객체에 액세스할 수 있다.

예:

```
>>> e = Entry.objects.get(id=2)
>>> e.blog # 관련된 Blog객체를 반환
```

외부 키 속성을 통해 가져오고 설정할 수 있다. 예상하다시피, foreign key에 대한 변경 사항은 save()를 호출할 때까지 데이터베이스에 저장되지 않습니다.

예:

```
>>> e = Entry.objects.get(id=2)
>>> e.blog = some_blog
>>> e.save()
```

ForeignKey 필드가 nul=True로 설정되어 있으면 (즉, NULL값을 허용하는 경우) None을 할당하여 관계를 제거할 수 있습니다.

예:

```
>>> e = Entry.objects.get(id=2)
>>> e.blog = None
>>> e.save() # "UPDATE blog_entry SET blog_id = NULL ...;"
```

관련 객체에 처음 액세스할 one-to-many관계에 전달 액세스(forward access)가 캐시된다. 동일한 객체 인스턴스에서 foreign key에 대한 후속 엑세스가 캐시된다.

예:

```
>>> e = Entry.objects.get(id=2)
>>> print(e.blog) # 데이터베이스를 조회하여 연결된 블로그를 검색함
>>> print(e.blog) # 데이터베이스를 조회하지 않고 캐시에서 가져옴
```

select_related() QuerySet 메소드는 모든 one-to-many 관계의 캐시를 미리 재귀로 채운다.

예:

```
>>> e = Entry.objects.select_related().get(id=2)
>>> print(e.blog) # 캐시에서 가져옴
>>> print(e.blog) # 캐시에서 가져옴
```

--

### following relationship "backward"

모델에 ForeignKey가 있는 경우 foreign-key 모델의 모든 인스턴스를 반환하는 Manager에 액세스 할 수 있다. 기본적으로, Manager의 이름은 FOO_set이다. 여기서 FOO는 소문자로 된 모델의 이름이다. 이 Manager는 위의 "Retrieving objects" 섹션에서 설명한 것처럼 필터링하고 조작할 수 있는 QuerySet들을 반환한다.

예:

```
>>> b = Blog.objects.get(id=1)
>>> b.entry_set.all() # Blog에 걸려있는 모든 Entry객체들을 반환

# b.entry_set은 QuerySet을 반환하는 Manager
>>> b.entry_set.filter(headline__contains='Lennon')
>>> b.entry_set.count()
```

ForeignKey 정의에서 related_name 매개변수를 설정하여 FOO_set으로 된 이름을 바꿀 수 있다. 예를 들어 Entry모델이 blog=ForeignKEy(Blog, on_delete=models.CASCADE, related_name='entries')로 변경된 경우 위에서 봤던 예제 코드는 이렇게 될 것이다.

```
>>> b = Blog.objects.get(id=1)
>>> b.entries.all() # Blog에 걸려있는 모든 Entry객체들을 반환

# b.entries 는 QuerySet을 반환하는 Manager
>>> b.entries.filter(headline__contains='Lennon')
>>> b.entries.count()
```

--

### Using a custom reverse manager

기본적으로 역 관계에 사용되는 RelatedManager 해당 모델의 기본 관리자의 하위 클래스다. 특정 쿼리에 대해 다른 관리자를 지정하려면 다음 구문을 사용하면 된다.

```python
from django.db import models

class Entry(models.Model):
    #...
    objects = models.Manager()  # 기본 Manager
    entries = EntryManager()    # 사용자정의 Manager

b = Blog.objects.get(id=1)
b.entry_set(manager='entries').all()
```

EntryManager가 get_queryset()메소드에서 기본 필터링을 수행하면 해당 필터링이 all()호출에 적용된다.

물론, 사용자정의 reverse manager를 지정하면 사용자정의 메소드를 호출 수 있다.

```
b.entry_set(manager='entries').is_published()
```

--

### Additional methods to handle related objects

위 "Retrieving objects"에서 정의된 QuerySet 메소드에 더하여, ForeignKey Manager에는 관련된 오브젝트 셋을 다루기 위한 다음과 같은 메소드가 추가됨. 

**add(obj1, obj2, ...)**    
지정된 모댈 객체를 관련 객체 셋에 추가함

**create(\*\*kwargs)**  
새 객체를 만들어 저장하고 관련 객체 셋에 넣는다. 새롭게 생성된 객체를 반환한다.

**remove(obj1, obj2)**  
관련 객체 셋에 지정된 모델 객체를 삭제함

**clear()**  
관련 객체 셋에서 모든 객체를 제거함

**set(objs)**
관련 객체 셋을 대체함

관련 셋의 멤버를 할당하려면 반복가능한 객체 인스턴스 또는 primary key 값 목록에 set()메소드를 사용해라.

```python
b = Blog.objects.get(id=1)
b.entry_set.set([e1, e2])
```

이 예제에서, e1과 e2는 완전한 Entry인스턴스이거나 정수로된 primary key값이 될 수 있다.

clear()메소드를 사용할 수 있는 경우, iterable의 모든 객체(위 예제에서는 list)가 추가되기전에 기존 객체가 entry_set에서 제거된다.

clear()메소드를 사용할 수 없는 경우, iterable의 모든 객체가 기존 요소를 제거하지 않고 그냥 추가된다.

이 섹션에서 설명하는 각 "역방향" 작업은 데이터베이스에 즉각 영향을 준다. 모든 추가, 생성 및 삭제가 즉시 자동으로 데이터베이스에 저장됨.

--

### Many-to-many relationships

MTM관계의 양끝은 다른 쪽 끝으로 자동 API 액세스를 얻는다. API는 위의 "역방향" OTM(one-to-many)관계와 동일하게 작동한다.

유일한 차이점은 속성 이름을 지정하는 데에 있다. ManyToManyField를 정의하는 모델은 해당 필드 자체의 속성 이름을 사용하지만 반면 "역방향"모델은 원래 모델의 소문자 모델 이름에 '_set' 추가하면된다.(역방향 OTM관계와 같은 방식으로)

이해를 도울 수 있는 예제는 다음과 같다.

```python
e = Entry.objects.get(id=3)
e.authors.all() # Entry에 대한 모든 Author객체를 반환한다
e.authors.count()
e.authors.filter(name__contains='John')

a = Author.objects.get(id=5)
a.entry_set.all() # 이 Author에 대한 모든 Entry객체를 반환한다
```

FK(Foreignkey)와 마찬가지로 ManyToManyField는 related_name을 지정할 수 있다. 위의 예에서는 Entry의 ManyToManyField가 related_name = 'endtries'를 지정한 경우 각 Author 인스턴스는 entry_set 대신 entries 속성을 갖는다.

--

### One-to-one relationships

OTO(one-to-one)관계는 MTO(many-to-one)와 비슷하다. 모델에 OneToOneField를 정의하면 해당 모델의 인스턴스는 모델의 단순 속성을 통해 관련 객체에 액세스 할 수 있다.

```python
class EntryDetail(models.Model):
    entry = models.OneToOneField(Entry, on_delete=models.CASCADE)
    details = models.TextField()

ed = EntryDetail.objects.get(id=2)
ed.entry # 관련 Entry객체를 반환한다
```

차이점은 "역"쿼리에서 보인다. OTO관계 관련 모델도 Manager 객체에 액세스 할 수 있지만 Manager는 객체 집합이 아닌 단 한개의 객체만을 나타낸다.

```
e = Entry.objects.get(id=2)
e.entrydetail # 관련 EntryDetail 객체를 반환함
```

이 관계에 객체가 할당되어 있지 않으면 장고는 DoesNotExist 예외를 발생시킨다. 전달 관계를 지정하는 것과 같은 방법으로 인스턴스를 역 관계에 지정할 수 있다.

```
e.entrydetail = ed
```

--

### How are the backward relationships possible?

다른 객체 관계형 mapper에서는 양측에 관계를 정의해야한다. 장고 개발자는 이를 DRY(Do not Repeat Yourself)원칙을 위반한다고 생각하므로 장고에서는 한쪽에서 관계를 정의하는 것만으로 충분하도록 만들었다.

그러나 모델 클래스가 다른 모델 클래스가 로드 될 때까지 어떤 다른 모델 클래스가 이 모델 클래스와 관련되어 있는지 모른다면 어떻게 할까?

대답은 [app_registry](https://docs.djangoproject.com/en/2.0/ref/applications/#django.apps.apps)에서 확인할 수 있다. 장고가 시작되면 INSTALLED_APPS에 나열된 각 응용 프로그램을 가져온 다음 각 응용 프로그램 내부에 models 모듈을 가져온다.

새로운 모델 클래스가 생성될 때마다 장고는 모든 관련 모델에 역방향 관계를 추가한다. 관련 모델을 아직 가져오지 않은 경우는 장고는 관련 모델을 가져올 때 관계의 트랙을 유지하고 추가한다.

이러한 이유로 INSTALLED_APPS에 나열된 앱에서 사용중인 모든 모델을 정의하는 것이 특히 중요하다. 그렇지 않으면 후방 관계가 제대로 작동하지 않을 수 있다.

--

### Queries over related objects

관련 객체를 포함하는 쿼리는 일반 값 필드가 포함된 쿼리와 동일한 규칙을 따른다. 매치시킬 쿼리의 값을 지정할 때 객체 인스턴스 자체나 객체의 기본 키 값을 사용할 수 있다.

예를 들어 id=5를 가지고 있는 Blog객체 b가 있다고 하면 다음 3개의 쿼리는 각각 같은 의미를 나타낸다.

```
Entry.objects.filter(blog=b) # 객체 인스턴스를 이용한 쿼리
Entry.objects.filter(blog=b.id) # 인스턴스의 아이디를 이용한 쿼리
Entry.objects.filter(blog=5) # 직접 아이디를 이용한 쿼리
```



## Falling back to raw SQL

장고가 처리하기엔 어려운 매우 복잡한 SQL쿼리를 작성해야한다면 수동으로 SQL을 작성할 수 있다. 장고는 raw SQL쿼리를 작성하기 위한 몇가지 옵션이 있다. [Performing raw SQL queries를 참고](https://docs.djangoproject.com/en/2.0/topics/db/sql/)

마지막으로 Django 데이터베이스 레이어는 단지 데이터베이스에 대한 인터페이스다. 다른 툴,  언어 또는 데이터베이스 프레임워크를 통해 데이터베이스에 액세스할 수 있다. 데이터베이스를 다루는데 장고만의 그런건 없다.


---

**2018.02.24 장고 MakingQueries 문서 번역 끝**



