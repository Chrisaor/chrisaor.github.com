---
layout: post
title:  "[시작하기] Django 훑어보기"
date:   2018-05-14 11:30:00 +0900
categories: django
---

# Django 훑어보기

Django는 급한 분위기의 뉴스룸에서 개발되어서 보편적인 웹 개발 업무를 빠르고 쉽게 만들어주도록 설계되었습니다. 여기서는 Django와 데이터 드리븐 웹 어플리케이션을 어떻게 작성하는지에 대한 개요를 보실수 있습니다.

이 문서의 목표는 Django가 어떻게 동작하는지 이해하는데 충분한 기술적 사항들을 알려주는데 있습니다. 하지만 튜토리얼이나 레퍼런스의 역할을 의도하지는 않습니다 -- 물론 우리는 그 두가지를 다 가지고있죠! 프로젝트를 시작할 준비가 되었으면 튜토리얼로 시작하거나 바로 좀더 자세한 문서로 뛰어들수 있습니다.

# 모델 설계

설사 Django를 데이터베이스 없이 쓸수 있을 지라도 데이터베이스 레이아웃을 파이썬 코드로 표현할수있는 object-relational mapper가 같이 딸려 옵니다.

data-model syntax는 당신의 모델을 표현할 풍부한 방법을 제공해줍니다 -- 이는 지금까지 수년간 데이터베이스 스키마 문제들을 해결하는데에 많은 도움을 주어왔습니다. 아래의 간단한 예제를 보시죠.

**`mysite/news/models.py`**

```python

from django.db import models

class Reporter(models.Model):
    full_name = models.CharField(max_length=70)

    def __str__(self):
        return self.full_name

class Article(models.Model):
    pub_date = models.DateField()
    headline = models.CharField(max_length=200)
    content = models.TextField()
    reporter = models.ForeignKey(Reporter, on_delete=models.CASCADE)

    def __str__(self):
        return self.headline
```

--

## 설치하기

다음으로 데이터베이스 테이블을 자동으로 생성하기위해서 Django 명령줄 유틸리티를 실행합니다.

```
$ python manage.py migrate
```

migrate 명령어는 가용한 모든 모델을 보고 데이터베이스에 아직 생성되지 않은 테이블을 만듭니다, 그뿐 아니라 스키마 제어를 위한 좀더 풍부한 기능을 옵션으로 제공합니다.

--

## 자유로운 API 즐기기

이것으로 자료에 접근하기위한 자유롭고 풍부한 Python API *`</topics/db/queries>`*를 얻을수 있습니다. API는 즉석에서 만들어지며, 코드 생성이 필요하지 않습니다.

```python
# Import the models we created from our "news" app
>>> from news.models import Article, Reporter

# No reporters are in the system yet.
>>> Reporter.objects.all()
<QuerySet []>

# Create a new Reporter.
>>> r = Reporter(full_name='John Smith')

# Save the object into the database. You have to call save() explicitly.
>>> r.save()

# Now it has an ID.
>>> r.id
1

# Now the new reporter is in the database.
>>> Reporter.objects.all()
<QuerySet [<Reporter: John Smith>]>

# Fields are represented as attributes on the Python object.
>>> r.full_name
'John Smith'

# Django provides a rich database lookup API.
>>> Reporter.objects.get(id=1)
<Reporter: John Smith>
>>> Reporter.objects.get(full_name__startswith='John')
<Reporter: John Smith>
>>> Reporter.objects.get(full_name__contains='mith')
<Reporter: John Smith>
>>> Reporter.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Reporter matching query does not exist.

# Create an article.
>>> from datetime import date
>>> a = Article(pub_date=date.today(), headline='Django is cool',
...     content='Yeah.', reporter=r)
>>> a.save()

# Now the article is in the database.
>>> Article.objects.all()
<QuerySet [<Article: Django is cool>]>

# Article objects get API access to related Reporter objects.
>>> r = a.reporter
>>> r.full_name
'John Smith'

# And vice versa: Reporter objects get API access to Article objects.
>>> r.article_set.all()
<QuerySet [<Article: Django is cool>]>

# The API follows relationships as far as you need, performing efficient
# JOINs for you behind the scenes.
# This finds all articles by a reporter whose name starts with "John".
>>> Article.objects.filter(reporter__full_name__startswith='John')
<QuerySet [<Article: Django is cool>]>

# Change an object by altering its attributes and calling save().
>>> r.full_name = 'Billy Goat'
>>> r.save()

# Delete an object with delete().
>>> r.delete()
```

--

## 동적인 관리자 인터페이스: 단순한 뼈대 세우기가 아닙니다 -- 이것은 완성된 집입니다

모델이 정의되면 Django는 전문적일고 생상 준비된 관리 인터페이스(인증된 사용자가 객체를 추가, 변경 및 삭제할 수 있게 해주는 웹 사이트)가 자동으로 생성됩니다. 관리사이트에 모델을 등록하는 것만큼 간단합니다.

**`mysite/news/models.py`**

```python
from django.db import models

class Article(models.Model):
    pub_date = models.DateField()
    headline = models.CharField(max_length=200)
    content = models.TextField()
    reporter = models.ForeignKey(Reporter, on_delete=models.CASCADE)
```

**`mysite/news/admin.py`**

```python
from django.contrib import admin

from . import models

admin.site.register(models.Article)
```

운영자에 의해서나 고객, 혹은 단지 개발자 당신 스스로에 의해 수정될 수 있는 이 사이트는 다음과 같은 철학을 가지고 있습니다. -- 단지 컨텐츠를 관리하기위한 백엔드 인터페이스를 만드는 데에 힘을 쏟지 말자.

Django 앱을 생성하는 하나의 전형적인 작업 흐름은 일단 모델을 만들고 관리자 사이트를 올려서 가능한 빨리 작동할 수 있게 만드는 것입니다, 그래서 당신의 운영자(혹은 고객)이 데이터 입력을 시작할 수 있게 합니다. 그러면 밖으로 데이터를 표현하는 방법을 개발합니다.

## URL 설계

깔끔하고 우아한 URL 계획은 고품질의 웹 어플리케이션에 매우 중요한 부분입니다. Django는 아름다운 URL 설계를 장려하며 URL에 .php 나 .asp 같은 불필요한 내용들을 넣지 않습니다.

앱을 위한 URL을 설계하기 위해서 URLconf 파이썬 모듈을 생성해야 합니다. 이것은 URL패턴과 파이선 콜백 함수 간의 간단한 매핑 정보를 담고 있는 여러분의 앱에 대한 목차입니다. URLconf는 또한 파이썬 코드와 URL간의 결합도를 낮춰줍니다.

아래의 코드는 위의 Reporter/Article 예제에 대해서 URLconf를 어떻게 쓰는지 보여줍니다.

**`mysite/news/urls.py`**

```python
from django.urls import path

from . import views

urlpatterns = [
    path('articles/<int:year>/', views.year_archive),
    path('articles/<int:year>/<int:month>/', views.month_archive),
    path('articles/<int:year>/<int:month>/<int:pk>/', views.article_detail),
]
```

위 코드는 URL 경로들을 파이썬 콜백 함수들("views")로 연결해 줍니다. 경로를 나타내는 문자열들은 매개 변수 태그들을 사용하여 URL 로부터 값을 "캡처"합니다. 사용자가 페이지를 요청하면, Django 는 각 경로를 순서대로 실행하고, 요청된 URL 과 일치하는 첫번째 것에서 정지하게 됩니다. (만약 아무것도 매치하는 것이 없다면, Django 는 특수한 사례인 404 view 를 호출합니다.) 이 경로들은 로드할 때 정규표현식으로 컴파일 되어 있기 때문에 엄청나게 빠릅니다.

URL 패턴들 중 하나가 매치하면, Django 는 주어진 파이썬 함수인 view 를 호출합니다. 각각의 view 에는 요청 메타데이터가 포함된 요청 개체와 패턴에 잡힌 값이 전달 됩니다.

예를 들어, 사용자가 URL "/articles/2005/05/39323/"로 요청을 보냅니다, 그러면 Django는 다음처럼 함수를 호출하게 됩니다. **news.views.article_detail(request, year=2005, month=5, pk=39323)**.

## 뷰를 작성합니다

각각의 뷰는 다음의 두가지 중에 하나를 수행할 책임이 있습니다.: 요청된 페이지의 내용을 담고 있는 HttpResponse 객체를 반환 하거나, Http404와 같은 예외를 발생시키는 것 입니다. 나머지는 여러분에게 달려있습니다.

일반적으로 뷰는 파라미터들에 따라 데이터를 가져오며 템플릿을 로드하고 템플릿을 가져온 데이타로 렌더링합니다. 아래는 위에서 만든 **year_archive**에 대한 예제 뷰입니다.

**`mysite/news/views.py`**

```python
from django.shortcuts import render

from .models import Article

def year_archive(request, year):
    a_list = Article.objects.filter(pub_date__year=year)
    context = {'year': year, 'article_list': a_list}
    return render(request, 'news/year_archive.html', context)
```

이 예제는 Django의 template system을 사용합니다. Django 템플릿 시스템은 몇몇 강력한 기능들을 가지고 있지만 프로그래머가 아닌 사람도 사용하기에 어렵지 않도록 간결함을 유지하도록 노력하였습니다.

--

## 여러분만의 템플릿 작성

위의 코드는 **news/year_archive.html **템플릿을 로드합니다.

장고는 여러분이 템플릿들중 중복을 최소화할 수 있게 하는 템플릿 검색 경로를 가지고 있습니다. 여러분의 장소 셋팅에 DIRS 템플릿을 확인하기 위한 디렉토리의 목록을 명시합니다. 만약 첫번째 디렉토리에 템플릿이 존재하지 않으면, 두번째 디렉토리, 그 외 디렉토리를 점검합니다.

**news/year_archive.html** 템플릿을 발견했다고 합시다. 아마도 이런 모양일 것입니다.

**`mysite/news/templates/news/year_archive.html`**

```python
{% extends "base.html" %}

{% block title %}Articles for {{ year }}{% endblock %}

{% block content %}
<h1>Articles for {{ year }}</h1>

{% for article in article_list %}
    <p>{{ article.headline }}</p>
    <p>By {{ article.reporter.full_name }}</p>
    <p>Published {{ article.pub_date|date:"F j, Y" }}</p>
{% endfor %}
{% endblock %}
```