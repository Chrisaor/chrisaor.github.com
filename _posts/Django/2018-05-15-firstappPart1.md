---
layout: post
title:  "[시작하기] 첫 번째 장고 앱 작성하기, Part 1"
date:   2018-05-15 11:30:00 +0900
categories: django
---

# 첫 번째 장고 앱 작성하기, part1

예제로 배워봅시다.

이 튜토리얼을 통해, 간단한 설문조사(Polls) 어플리케이션을 만드는 과정을 따라해 보겠습니다.

두 파트로 구성되어 있습니다.

- 사람들이 설문 내용을 보고 직접 투표할 수 있는 개방된 사이트
- 관리자가 설문을 추가, 변경, 삭제할 수 있는 관리용 사이트

쉘 프롬프트 ($ 접두사로 표시)에서 다음 명령을 실행하여 Django가 설치되어 있고 어떤 버전인지 알 수 있습니다 :

`$ python -m django --version`

Django 가 설치 되었다면, 설치된 Django 의 버전을 확인할 수 있습니다. 만약 설치가 제대로 되지 않았다면, "No module named django" 과 같은 에러가 발생합니다.


## 프로젝트 만들기

Django를 처음 사용한다면, 초기 설정에서 주의를 기울여야 합니다. 즉, Django project 를 구성하는 코드를 자동 생성해야 하는데, 이 과정에서 데이터베이스 설정, Django 위한 옵션들, 어플리케이션을 위한 설정들과 같은 Django 인스턴스를 구성하는 수많은 설정들이 생성되기 때문입니다.

커맨드라인에서 cd 명령으로 코드를 저장할 디렉토리로 이동 한 후, 다음의 명령을 수행합니다.

```
$ django-admin startproject mysite
```

이 명령은 현재 디렉토리에서 mysite 라는 디렉토리를 생성할 것입니다. 만약 이 명령이 동작하지 않는다면, ``django-admin``를 작동하는데 발생하는 문제 를 확인하세요

> 주석
>
project 를 생성할 때, Python 이나 Django 에서 사용중인 이름은 피해야 합니다. 특히, django (Django 그 자체와 충돌이 일어납니다) 나, test (Python 패키지의 이름중 하나입니다) 같은 이름은 피해야 한다는 의미입니다.

--

>코드가 어디에서 서비스 되어야 할까요?

>과거의 PHP 를 작성해본 경험이 있다면(최근의 프레임워크 말고) 아마 코드 전체를 /var/www 같은 웹 서버의 DocumentRoot 에 넣으려고 하려고 할겁니다. Django 에서는 그러지 마십시요. 파이선 코드가 웹서버의 DocumentRoot 에 존재하는것은 좋은 생각이 아닙니다. 웹을 통해서 외부의 사람들이 Python 코드를 직접 열어볼 수 있는 위험이 있기 때문입니다. 그렇게 되면 보안에 별로 좋지 않습니다.

>작성한 코드를 /home/mycode 와 같은 DocumentRoot 의 바깥에 두는것을 권합니다.

startproject에서 무엇이 생성되는지 확인해 봅시다.

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

이 파일들은,

- **mysite/** 디렉토리 바깥의 디렉토리는 단순히 프로젝트를 담는 공간입니다. 이 이름은 Django 와 아무 상관이 없으니, 원하는 이름으로 변경하셔도 됩니다.

- **manage.py**: Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인의 유틸리티 입니다. manage.py 에 대한 자세한 정보는 django-admin and **manage.py** 에서 확인할 수 있습니다.

- **mysite/** 디렉토리 내부에는 project 를 위한 실제 Python 패키지들이 저장됩니다. 이 디렉토리 내의 이름을 이용하여, (**mysite.urls** 와 같은 식으로) project 어디서나 Python 패키지들을 import 할 수 있습니다.

- **mysite/__init__.py**: Python 으로 하여금 이 디렉토리를 패키지 처럼 다루라고 알려주는 용도의 단순한 빈 파일입니다. Python 초심자라면, Python 공식 홈페이지의 more about packages 를 읽어보십시요.

- **mysite/settings.py**: 현재 Django project 의 환경/구성을 저장합니다. Django settings 에서 환경 설정이 어떻게 동작하는지 확인할 수 있습니다.

- **mysite/urls.py**: 현재 Django project 의 URL 선언을 저장합니다. Django 로 작성된 사이트의 "목차" 라고 할 수 있습니다. URL dispatcher 에서 URL 에 대한 자세한 내용을 읽어보세요.

- **mysite/wsgi.py**: 현재 project 를 서비스 하기 위한 WSGI 호환 웹 서버의 진입점 입니다. How to deploy with WSGI 를 읽어보세요.


## 개발 서버

당신의 Django project 가 제대로 동작하는지 확인해 봅시다. **mysite** 디렉토리로 이동하고, 다음 명령어를 입력하십시요.

```
$ python manage.py runserver
```

커맨드라인에서 다음과 같은 출력을 볼 수 있습니다.

```
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

5월 12, 2018 - 15:50:53
Django version 2.0, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

> 주석

>현재 데이터베이스에 적용되지 않은 변경사항들(migrations)에 대한 경고들은 무시해도 됩니다. 데이터베이스에 대한 부분은 간단히 다루도록 하겠습니다.

Django 개발 서버를 시작했습니다. 개발 서버는 순수 Python 으로 작성된 경량 웹 서버입니다. Django 에 포함되어 있기 때문에 아무 설정없이 바로 개발에 사용할 수 있습니다.

이쯤에서 하나 기억할 것이 있습니다. **절대로** 개발 서버를 운영 환경에서 사용하지 마십시요. **개발 서버는 오직 개발 목적으로만** 사용하여야 합니다. (우리는 웹 프레임워크를 만들지 웹 서버를 만들지는 않거든요)

이제 서버가 동작하기 시작했으니, 자신의 웹 브라우져에서 http://127.0.0.1:8000/ 을 통해 접속할 수 있습니다. 로켓이 이륙하는 모습이 담긴 "Congratulations!" 페이지를 보게될 거에요. 잘 동작 하네요!

>**포트 변경하기**

>기본적으로, runserver 명령은 내부 IP 의 8000 번 포트로 개발 서버를 띄웁니다.

>만약 이 서버의 포트를 변경하고 싶다면, 커맨드라인에서 인수를 전달해주면 됩니다. 예를들어, 이 명령은 포트를 8080 으로 서버를 시작할 것입니다.

>`$ python manage.py runserver 8080`

>서버의 IP를 변경하려면 포트와 함께 전달하십시오. 예를 들어, 사용 가능한 모든 공용 IP를 청취하려면 (이는 Vagrant를 실행 중이거나 네트워크의 다른 컴퓨터에서 작업하고 싶을 때 유용합니다) 다음을 사용하십시오.

`$ python manage.py runserver 0:8000`

> ** 0 ** **은 ** 0.0.0.0 **의 지름길입니다. 개발 서버의 전체 문서는 : djadmin :`runserver '참조에서 찾을 수 있습니다.

---

>**runserver 의 자동 변경 기능**

>개발 서버는 요청이 들어올 때마다 자동으로 Python 코드를 다시 불러옵니다. 코드의 변경사항을 반영하기 위해서 굳이 서버를 재기동 하지 않아도 됩니다. 그러나, 파일을 추가하는 등의 몇몇의 동작은 개발서버가 자동으로 인식하지 못하기 때문에, 이런 상황에서는 서버를 재기동 해야 적용됩니다.

## 설문조사 앱 만들기

이제, 작업을 시작하기 위해 당신의 환경(project) 이 설치되었습니다.

Django 에서 당신이 작성하는 각 어플리케이션들은 다음과 같은 관례로 Python 패키지가 구성됩니다. Django 는 앱(app) 의 기본 디렉토리 구조를 자동으로 생성할 수 있는 도구를 제공하기 때문에, 코드에만 더욱 집중할 수 있습니다.

>**프로젝트 대 앱**

>project 와 app 은 무엇이 다를까요? app 은 (블로그나 공공 기록물을 위한 데이터베이스나, 간단한 설문조사 앱과 같은) 특정한 기능을 수행하는 웹 어플리케이션을 말합니다. project 는 이런 특정 웹 사이트를 위한 app 들과 각 설정들을 한데 묶어놓은 것 입니다. project 는 다수의 app 을 포함할 수 있고, app 은 다수의 project 에 포함될 수 있습니다.

당신의 app 은 Python 경로 어디라도 있을 수 있습니다. 그러나 이 예제에서는, **mysite** 같은 submodule 말고, top-level 에서 곧바로 import 할 수 있게 끔 **manage.py** 바로 옆에 생성해 보도록 하겠습니다.

app 을 생성하기 위해 **manage.py** 가 존재하는 디렉토리에서 다음의 명령을 입력해 봅시다.

```
$ python manage.py startapp polls
```

**polls** 라는 디렉토리가 생겼습니다. 이걸 펼쳐놓으면 아래와 같습니다.
```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

이 디렉토리 구조는 poll 어플리케이션의 집이 되어줄 것입니다.


## 첫 번째 뷰 작성하기
첫 번째 뷰를 작성해봅시다. "polls/view.py"를 열어 다음과 같은 파이썬 코드를 입력합니다

**`polls/views.py`**

```python
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

Django 에서 가장 간단한 형태의 view 입니다. view 를 호출하려면 이와 연결된 URL 이 있어야 하는데, 이를 위해 URLconf 가 사용됩니다.

polls 디렉토리에서 URLconf 를 생성하려면, **urls.py** 라는 파일을 생성해야 합니다. 정확히 생성했다면, app 디렉토리는 다음과 같이 보일겁니다.:

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    urls.py
    views.py
```

"polls/urls.py" 파일에는 다음과 같은 코드가 포함되어 있습니다.

**`polls/urls.py`**

```PYTHON
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

다음 단계는, 최상위 URLconf 에서 **polls.urls** 모듈을 바라보게 설정합니다. **mysite/urls.py**  파일을 열고, **django.urls.include** 를 import 하고, **urlpatterns** 리스트에 **include()** 함수를 다음과 같이 추가 합니다.


**`mysite/urls.py`**

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

**include()** 함수는 다른 URLconf 들을 참조할 수 있도록 도와줍니다. Django 가 함수 **include()** 를 만나게 되면, URL 의 그 시점까지 일치하는 부분을 잘라내고, 남은 문자열 부분을 후속 처리를 위해 include 된 URLconf 로 전달합니다.

**include()** 에 숨은 아이디어 덕분에 URL 을 쉽게 연결할 수 있습니다. polls 앱에 그 자체의 URLconf (**polls/urls.py**) 가 존재하는 한, "/polls/", 또는 "/fun_polls/", "/content/polls/" 와 같은 경로, 또는 그 어떤 다른 root 경로에 연결하더라도, 앱은 여전히 잘 동작할 것입니다.


>**언제 include() 를 사용해야 하나요?**

>**admin.site.urls** 를 제외한, 다른 URL 패턴을 include 할 때마다 항상 **include()** 를 사용해야 합니다.

이제 **index** view 가 URLconf 에서 연결되었습니다. 잘 작동하는지 확인하기 위해 다음을 입력해 보세요.

```
$ python manage.py runserver
```

브라우저에서 http://localhost:8000/polls/ 를 입력하면 **index** view 로 정의한 *"Hello, world. You're at the polls index."* 가 보일 것입니다.

path() 함수에는 2개의 필수 인수인 **route** 와 **view**, 2개의 선택 가능한 인수로 **kwargs** 와 **name** 까지 모두 4개의 인수가 전달 되었습니다. 이 시점에서, 이 인수들이 무엇인지 살펴보는 것은 의미가 있습니다.

**path() 인수: route**

**route** 는 URL 패턴을 가진 문자열 입니다.  요청이 처리될 때, Django 는 **urlpatterns** 의 첫 번째 패턴부터 시작하여, 일치하는 패턴을 찾을 때 까지 요청된 URL 을 각 패턴과 리스트의 순서대로 비교합니다.

패턴들은 GET 이나 POST 의 매개 변수들, 혹은 도메인 이름을 검색하지 않습니다. 예를 들어, **https://www.example.com/myapp/** 이 요청된 경우, URLconf 는 오직 **myapp/** 부분만 바라 봅니다. **https://www.example.com/myapp/?page=3**, 같은 요청에도, URLconf 는 역시 **myapp/** 부분만 신경씁니다.

**path() 인수: view**
Django 에서 일치하는 패턴을 찾으면, HttpRequest 객체를 첫번째 인수로 하고, 경로로 부터 '캡처된' 값을 키워드 인수로하여 특정한 view 함수를 호출합니다. 나중에 이에 대한 간단한 예제를 살펴보겠습니다.

**path() 인수: kwargs**
임의의 키워드 인수들은 목표한 view 에 사전형으로 전달됩니다. 그러나 이 튜토리얼에서는 사용하지 않을겁니다.

**path() 인수: name**
URL 에 이름을 지으면, 템플릿을 포함한 Django 어디에서나 명확하게 참조할 수 있습니다. 이 강력한 기능을 이용하여, 단 하나의 파일만 수정해도 project 내의 모든 URL 패턴을 바꿀 수 있도록 도와줍니다.

request 와 response 의 기본 흐름을 이해하셨다면, 튜토리얼 2장 에서 데이터베이스 작업을 시작해보세요.