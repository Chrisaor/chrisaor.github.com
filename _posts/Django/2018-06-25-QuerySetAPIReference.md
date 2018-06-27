---
layout: post
title:  "[The model layer] QuerySet"
date:   2018-06-24 11:30:00 +0900
categories: django
---

## QuerySet이 평가될 때

내부적으로 QuerySet은 실제로 DB에 접근하지 않고도 구성되고, 필터링고, 슬라이싱되고 또한 일반적으로 전달될 수 있다. QuerySet을 평가할 때까지 실제로 DB에 접근하지 않는다.

다음과 같은 방법으로 QuerySet을 평가할 수 있다.

- **Iteration.** 

QuerySet이 반복 가능하다는 가정하에, 처음 반복할 때 QuerySet을 평가한다. 예를 들어, DB의 모든 entry 제목을 인쇄할 수 있다.

```python
for e in Entry.objects.all():
	print(e.headline)
```

Note: 최소한 하나의 결과가 존재하는지만 확인하려면 exists()를 써라.

- **Slicing**

Limiting QuerySEt에서 설명했듯이, QuerySet은 Python의 배열 슬라이싱 구문을 사용하여 슬라이스 할 수 있다. 평가되지 않은 QuerySet을 슬라이스하면 일반적으로 평가되지 않은 다른 QuerySet이 반환되지만 Django는 슬라이스 구문의 "step" 매개변수를 하용하면 DB 쿼리를 실행하고 리스트를 반환한다. 평가된 QuerySet을 슬라이스하면 또한 리스트가 반환된다.

또한 평가되지 않은 QuerySet을 슬라이스하는 경우 평가되지 않은 또다른 QuerySet이 반환되지만 필터를 추가하거나 순서를 변경하는 등 변경이 허용되지 않는다. 이는 SQL로 잘 변환되지 않으며 명확한 의미도 없기 때문이다.

- **Pickling/Caching**

Pickling(?). 중요한 점은 결과가 DB에서 읽혀진다는 것이다.

- **repr()**

QuerySet은 repr()을 호출할 때마다 평가된다. 이는 Python 인터프리터의 편의를 위해 제공되므로 대화식 API를 사용할 때 결과를 즉시 볼 수 있다.

- **len()**

QuerySet은 len()을 호출할 때마다 평가된다. 결과 리스트의 길이를 반환한다.

Note: 실제 객체를 필요로 하기보단, 레코드 수만을 평가하려할 때 DB수준에서 SQL의 SELECT COUNT(*)를 사용하는 것이 훨씬 효율적이다. Django는 이를 count()메소드로 제공한다.

- **list()**

list()를 호출하여 강제로 QuerySet을 평가한다. 예를 들어,

```python
entry_list = list(Entry.objects.all())
```

- **bool()**

bool(), or 또는 and 문을 사용하는 것처럼 bool 컨텍스트에서 QuerySet을 테스트하면 query가 실행된다. 적어도 하나의 결과가 있으면 QuerySet은 True이고 아니면 False를 반환한다. 예를들어,

```python
if Entry.objects.filter(headline="Test"):
	print("There is at least one Entry with the headline Test")
```

Note : 적어도 하나 이상의 결과가 있는지 확인하려는 경우 아까처럼 exists()를 사용하는 것이 효율적이다.

### Pickling QuerySets

QuerySet을 pickle(?)하면 pickling 하기 전에 모든 결과가 강제로 메모리에 로드된다. Pickling은 일반적으로 캐싱의 precursor(어떤 일이 일어나기 전에 선행되는 일)로 작동한다. 캐싱된 queryset이 다시 로드될 때 결과가 이미 존재하고 사용될 준비가 되길 원할것이다.(DB를 읽는 것은 시간이 걸리는 일이고 캐싱의 목적을 파괴한다.) QuerySet을 unpickle할 때 현재 DB에 있는 결과가 아니라 query된 시점의 결과가 포함된다.

나중에 DB에서 QuerySet을 재작성하기 위해 필요한 정보만 pickle하려는 경우, QuerySet의 query속성을 pickle해라. 다음과 같은 코드를 사용하여 원본 QuerySet을 (어떤 결과도 로드되지 않고) 다시 로드할 수 있다.

```python
>>> import pickle
>>> query = pickle.loads(s) # s는 피클된 문자열이라고 가정
>>> qs = MyModel.objects.all()
>>> qs.query = query # 원래의 'query'를 복구한다.
```

query속성은 불투명한 객체이다. 쿼리 구조의 내부를 나타내고 public API의 일부가 아니다. 그러나, 여기서 설명한대로 속성의 내용을 pickle 및 unpickle하는 것은 안전하고 완벽하게 지원된다.

> **버전간 pickles를 공유할 수 없다**
> 
> QuerySet의 Pickles는 생성하는데 사용된 Django의 버전에서만 유효하다. Django 버전 N을 사용해 pickle을 생성했다면, Django 버전 N+1에서도 pickle을 읽을 수 있다는 보장은 없다. Pickle을 장기 보관 전력의 일부로 사용되어서는 안된다.
>
> 아무도 모르게 손상된 객체는 pickle 호환성 오류를 진단하기가 어려울 수 있기 때문에, pickle된 Django 버전과 다른 Django에서 queryset을 unpickle할 때 RuntimeWarning가 발생된다.

## QuerySet API

QuerySet의 공식적인 선언

**class QuerySet(model=None, query=None, using=None)**

일반적으로 QuerySet과 상호작용할 때 필터를 연결해서 사용한다. 이 작업을 수행하기 위해 대부분의 QuerySet메소드는 새로운 쿼리셋을 반환한다. 이러한 방법은 섹션의 뒷부분에서 자세히 설명한다.

QuerySet클래스에서는 두 개의 public 속성이 있다. 이는 그 자체를 스스로 검사할 수 있는 요소로 사용할 수 있다.

**ordered**

QuerySet이 정렬되어 있다면 True를 반환한다. order_by()절 또는 모델의 기본 정렬 속성을 갖고 있을 경우에 해당된다. 그렇지 않으면 False를 반환.

**db**

이 쿼리가 실행될 경우 사용할 DB를 말한다.

> **Note**
> 
> QuerySet의 쿼리 매개 변수가 있으므로  특화된 query하위 클래스가 내부 쿼리 상태를 재구성할 수 있다. 매개 변수의 값은 해당 쿼리 상태의 불투명한 표현이며 public API의 일부가 아니다. 간단히 말해서 물어볼 필요가 있다면 사용할 필요가 없다.


## 새로운 쿼리셋을 반환하는 메소드(23개)

Django는 QuerySet에 의해 리턴된 결과의 타입이나 SQL쿼리가 실행되는 방식을 변경하는 등 QuerySet을 수정할 수 있는 메소드의 범위를 제공한다.

### filter()

지정된 검색 매개 변수와 일치하는 객체가 포함된 새 QuerySet을 반환함.

매개 변수가 여러개라면 SQL AND가 사용된다.

더 복잡한 쿼리를 실행해야 하는 경우는 Q 객체를 사용한다.

### exclude()

지정된 조회 매개 변수와 일치하지 않는 객체가 포함된 새 QuerySet을 반환한다.

### annotate()

제공된 쿼리 표현식 리스트로 QuerySet의 각 객체에 주석을 단다. 표현식은 단순 값, 모델 필드에 대한 참조 또는 객체와 관련하여 계산된 집계식(평균, 합계 등)이 될 수 있다.

```python
>>> from django.db.models import Count
>>> q = Blog.objects.annotate(Count('entry'))
>>> q[0].entry__count
42
```

### order_by()

기본적으로 QuerySet에 의해 반환 된 결과는 모델 메타의 정렬 옵션에 의해 주어진 튜플에 의해 정렬된다. order_by 메소드를 사용하여 QuerySet단위로의 값을 겹쳐 쓸 수 있다.

### reverse()

reverse() 메소드를 사용하여 queryset의 요소가 리턴되는 순서를 반대로 할 수 있다. reverse()를 다시 호출하면 원래의 순서대로 복원된다.

쿼리셋에서 마지막 5개의 아이템을 얻으려면

```
my_queryset.reverse()[:5]
```


### distinct()

SELET DISTINCT을 사용한 새 QuerySet을 반환한다. 조회 결과에서 중복 레코드가 제거된다.

### values()

iterable로 사용될 때 모델 인스턴스가 아닌 사전을 반환하는 QuerySet을 반환한다. 각 사전은 모델 객체의 속성 이름에 해당하는 키로 객체를 나타낸다. 다음 예에서는 value()와 일반 모델 객체를 비교해본다.

```python
# 이 리스트는 Blog객체를 포함한다.
>>> Blog.objects.filter(name__startswith='Beatles')
<QuerySet [<Blog: Beatles Blog>]>

# 이 리스트는 사전을 포함한다.
>>> Blog.objects.filter(name__startswith='Beatles').values()
<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
```

필드를 지정하면 각 사전에는 지정한 필드에 대한 필드 키/값만 포함된다.

```python
>>> Blog.objects.values('id', 'name')
<QuerySet [{'id': 1, 'name': 'Beatles Blog'}]>
```
### values_list()

사전을 반환하는 대신 튜플을 반환한다는 점을 제외하고 values()와 비슷하다. 각 튜플에는 해당 필드의 값 또는 values_list() 호출로 전달 된 표현식이 들어있으므로 첫 번째 항목이 첫 번째 필드가 된다.

```python
>>> Entry.objects.values_list('id', 'headline')
<QuerySet [(1, 'First entry'), ...]>
```

### dates()

datetime.date 객체의 목록으로 평가되는 QuerySet을 반환한다. 

### datetimes()

datetime.datetime 객체의 목록으로 평가되는 QuerySet을 반환한다.

### none()

객체를 반환하지 않는 queryset이 만들어지고 결과에 액세스 할 때 쿼리가 실행되지 않는다.

### all()

현재의 QuerySet(또는 QuerySet 서브클래스)의 복사본을 반환한다. 모델 매니저 또는 QuerySet을 전달하고 결과에 대해 추가 필터링을 수행하려는 상황해서 유용하다.

QuerySet이 평가 될 때, 일반적으로 QuerySet은 결과를 캐시한다. QuerySet을 평가한 후 데이터베이스의 데이터가 변경된 경우 이전에 평가 된 QuerySet에서 all()을 호출하여 동일한 쿼리에 대한 업데이트 된 결과를 가져올 수 있다.


### union()

SQL의 UNION 연산자를 사용해서 두 개 이상의 QuerySet 결과를 결합한다.

### intersection()

SQL의 INTERSECT 연산자를 이용해서 두 개 이상의 QuerySet의 결과에서 공유하고 있는 요소를 반환한다.

### difference()

SQL의 EXCEPT 연산자를 사용하여 QuerySet에서 있지만 일부 다른 QuerySet에는 없는 요소만 유지한다. 차 집합을 생각하면 된다.

### select_related()

외래키 관계를 follow하는 QuerySet을 리턴한다. 쿼리를 실행할 때 추가의 관련 객체 데이터를 선택한다. 이것은 하나의 복잡한 쿼리 결과를 나타내는 성능 향상이다. 하지만 나중에 외래키 관계를 사용하면 데이터베이스 쿼리가 필요하지 않음을 의미한다.

다음 예는 일반 조회와 select_related() 조회의 차이를 보여준다.

다음은 일반 조회의 예:

```python
# 데이터베이스에 접근함
e = Entry.objects.get(id=5)

# 관련 Blog객체를 얻기위해 다시 데이터베이스에 접근함
b = e.blog
```

다음은 select_related 조회의 예:

```python
# 데이터베이스에 접근함
e = Entry.objects.select_related('blog').get(id=5)

# 데이터베이스에 접근하지 않는다. 왜냐하면 이전 쿼리에서 미리 채워졌기 때문이다.
b = e.blog
```


### prefetch_related()

특정한 조회마다 관련 객체를 일괄적으로 자동 검색하는 QuerySet을 리턴함.

select_related와 비슷한 목적이다. 데이터베이스 쿼리를 줄이기 위해 설계되었지만 전략이 매우 다르다.

select_related는 외래키나 one-to-one관계에서만 사용할 수 있다.

하지만 prefetch_related는 각 관계에 대해 별도로 조회를 수행하고 Python에서 'joining'을 수행한다. select_related가 사용할 수 없는 many-to-many에서도 사용할 수 있다.

### extra()

때로는 Django 쿼리 문법만으로 복잡한 WHERE절을 쉽게 표현할 수 없다. 이러한 엣지 케이스에서 Django는 QuerySet에 의해 생성된 SQL에 특정 절을 삽입하기 위한 후크인 extra()라는 QuerySet 수정인자를 제공한다.


### defer()

일부 복잡한 데이터 모델링 상황에서 모델에 많은 필드가 포함될 수 있거나 파이썬 객체로 변환하는데 많은 비용이 소요되는 필드가 포함될 수 있다. 특정 필드가 필요할지 모르는 상황에서 queryset의 결과를 사용하는 경우 Django에 데이터베이스에서 검색하지 않도록 지시할 수 있다.


### only()

defer()의 반대되는 메소드이다. 모델을 검색할 때 연기해서는 안되는 필드를 호출한다.

### using()

둘 이상의 데이터베이스를 사용하는 경우 QuerySet을 평가할 데이터베이스를 제어하는데 사용된다.

### select\_for\_update()

처리가 끝날 때까지 행을 lock하고 지원되는 데이터베이스에서 SELECT ... FOR UPDATE SQL문을 생성하는 queryset을 반환한다.

### raw()

raw SQL 쿼리를 취해서 실행하고 RawQuerySet 인스턴스를 반환한다.

## 쿼리셋을 반환하지 않는 메소드(17개)	

### get()

### create()

### get\_or\_create()

### update\_or\_create()

### bul_create()

### count()

### in_bulk()

### iterator()

### latest()

### earliest()

### first()

### last()

### aggregate()

### exists()

### update()

### delete()

### as_manager()

## 필드 조회

### exact

### iexact

### contains

### icontains

### in

### gt

### lt

### lte

### startswith

### istartswith

### endswith

### iendswith

### range

### date

### year

### month

### day

### week

### week_day

### quarter

### time

### hour

### minute

### second

### isnull

### regex

### iregex

## 집합 기능

### expression

### output_field

### filter

### **extra

### Avg

### Count

### Max

### Min

### StdDev

### Sum

### Variance

## Query관련 툴

### Q() objects

### Prefetch() objects

### prefetch\_related\_objects()

### FilteredRelation() objects




