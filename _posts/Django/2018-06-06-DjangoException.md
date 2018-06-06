---
layout: post
title:  "Django exception 기록"
date:   2018-06-06 14:20:00 +0900
categories: django
---

2018.06.04

### NoReverseMatch at /url/

당신의 URLconf에서 제공된 매개변수에 대해 매칭되는 URL을 식별할 수 없을 때 django.urls에 의해 발생한다.

#### `urls.py`

```python
urlpatterns = [
    url(r'^(\d+)/', post_detail, name='post-detail')
]
```

다음 urls.py에서 views의 메서드 post_detail에 \d+에 매치되는 인자를 전달해준다.


#### `views.py`

```python
def post_detail(request, post_id):
    post = Post.objects.get(id=post_id)
    context = {
        'post': post,
    }
    return render(request, 'blog/post_detail.html', context)
```

urls.py에 의해 파싱된 request는 views.py에서 post_detail.html로 렌더링 된다.

{% raw %}

#### `post-list.html`

```python
<a href="{% url 'post-detail' %}"><h2 class="card-title">[{{ post.id }}] {{ post.title }}</h2></a>
``` 

url을 시작으로 views를 거쳐 template으로 request process를 정방향이라고 한다면 template에서 a태그에서 post-detail url에 대한 매개 변수를 전해주는 것은 역방향이라고 할 수 있다.


하지만 post-list.html에서 {% 'post-detail' %}는 post-detail이라는 이름의 url에 필요한 인자를 전해주지 않아서 url패턴이 매칭되지 않는다. 따라서 'post.id'라는 id값을 전해주어 url에 매치될 수 있도록 해준다


```python
<a href="{% url 'post-detail' post.id %}"><h2 class="card-title">[{{ post.id }}] {{ post.title }}</h2></a>
``` 

{% endraw %}

2018.06.06

### ValueError at /write/

<img width="615" alt="2018-06-06 9 13 48" src="https://user-images.githubusercontent.com/33015649/41037699-00e113c4-69cf-11e8-9d01-0f7250639a0d.png">


```python
class Post(models.Model):
    author = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
    )
```

블로그의 특성상 글쓰는 사람은 블로그 주인에 한정되어야 한다. 블로그 주인인 superuser의 id를 author에 연결하였는데 superuser가 로그아웃이 되면 해당 에러가 발생하게됨.