---
layout: post
title:  "[Forms] Overview"
date:   2018-03-07 11:30:00 +0900
categories: django
---

구글 번역의 도움을 (많이) 받아 장고 공식 문서를 번역하였습니다.

# 폼 사용하기

> 이 문서는 웹 폼의 기본적인 내용과 장고에서 어떻게 사용되는지 소개한다. Form API의 자세한 내용은 [The Forms API](https://docs.djangoproject.com/en/2.0/ref/forms/api/), [Form and field validation](https://docs.djangoproject.com/en/2.0/ref/forms/validation/)을 참고

콘텐츠를 게시하는 것만 가능한 웹사이트를 개발하는 목적이 아니라면 form을 이해하고 사용해야한다. 

장고는 사이트 방문자로부터 입력을 받아 양식을 작성한 다음 입력을 처리하고 응답하는데 도움이되는 다양한 도구와 라이브러리를 제공한다.

## HTML 폼

HTML에서 form은 방문자가 텍스트 입력, 옵션 선택, 객체 또는 컨트롤 조작 등과 같은 작업을 수행할 수 있는 \<form\>...\</form\>내의 요소 모음이다. 이렇게 수행된 작업을 서버로 보내주는 역할을 한다.

이러한 양식 인터페이스 요소 중 일부(텍스트 입력, 확인란)는 매우 간단하며 HTML 자체에 내장되어 있다. 날짜 선택을 하거나 슬라이더를 이동하는 인터페이스는 일반적으로 HTML form \<input\>요소와 자바스크립트 및 CSS를 사용해서 효과를 얻을 수 있다.

\<input\> 요소 뿐만 아니라 form도 두 가지를 지정해야한다.  

- where : 사용자의 입력에 해당하는 데이터가 반환되어야 하는 URL
- how : 데이터가 어떻게 반환되어야 하는지에 대한 HTTP method

예를 들어, 장고 관리자의 로그인 form은 여러 개의 \<input\>요소를 포함합니다. 하나는 사용자 이름의 type='text', password는 type='password', 다른  로그인 버튼의 type='submit'이다. 또 사용자가 볼 수 없는 몇 가지 숨겨진 텍스트 필드가 포함되어 있으며 이는 장고가 다음에 수행할 작업을 결정할 때 사용한다.

또한 폼 데이터가 \<form\>의 action속성 -/admin/-에 지정된 URL로 보내져야하며 메소드 속성 -post에 지정된 HTTP 메커니즘을 사용하여 보내야한다고 브라우저에 알린다.

\<input type='submit' value='Log in'\> 요소가 트리거되면 데이터는 /admin/로 리턴된다.

### GET과 POST

**GET**과 **POST**는 폼을 다룰 때 사용할 수 있는 유일한 HTTP 메소드이다.

장고의 로그인 폼은 **POST**메소드를 사용하여 반환된다. **POST**메소드는 브라우저가 form 데이터를 묶어서 전송하기 위해 인코딩하고 서버로 보낸 후 그에 대한 응답을 수신한다.

반대로 **GET**은 제출된 데이터를 문자열로 묶어 URL을 작성하는 데 사용된다. URL에는 데이터를 전송해야하는 주소는 물론 데이터와 키와 값도 포함된다. 장고 문서에서 무언가를 찾으려 클릭하면 http://docs.djangoproject.com/search/?q=forms%release=1 형식의 URL이 생성되는 것을 볼 수 있다.

**GET**과 **POST**는 일반적으로 다른 목적으로 사용된다.

시스템의 상태를 변경하는 데 사용할 수 있는 요청(예: DB변경)은 **POST**를 사용해야한다. **GET**은 시스템 상태에 영향을 주지 않는 요청에만 사용해야한다.

**GET**은 비밀번호 form에 적합하지 않다. 비밀번호가 URL에 표시되고 브라우저 기록 및 서버로그에도 일반 텍스트로 표시되기 때문이다. 대량의 데이터나 이미지와 같은 바이너리 데이터에도 적합하지 않다. admin form에 **GET**요청을 사용하는 웹 어플리이션은 보안에 위험이 있다. 공격자가 시스템의 민간함 부분에 액세스하기 위해 form의 요청을 모방하는 것이 쉬워진다. POST는 장고의 [CSRF protection](https://docs.djangoproject.com/en/2.0/ref/csrf/)과 같은 다른 보호 기능과 결합되어 액세스를 보다 효율적으로 제어한다.

반면 **GET** 요청을 나타내는 URL은 북마크, 공유 그리고 다시제출하기 같은 것이 쉽게 가능하기 때문에 **GET**은 웹 서치 form같은 것에 적합하다. 

## 장고에서 폼의 역할

Form을 다루는 건 꽤 복잡한 일이다. 장고 관리자를 생각해보자. 여러가지 타입의 데이터 항목을 form에 나타내고, HTML로 렌더링되고, 편리한 인터페이스로 편집되고, 서버로 반횐되고, 유효성이 검사되고 끝낸 저장되거나 전달되도록 준비하는 많은 일들을 한다. 

장고의 form기능은 이 작업의 상당 부분을 단순화하고 자동화 할 수 있으며, 대부분의 프로그래머가 작성한 코드에서 수행할 수 있는 것보다 더 안전하게 수행할 수 있다.

장고는 form에 관련하여 다음의 3가지로 분리된 작업을 처리한다.

- 데이터 준비 및 재구성하여 렌더링 준비
- 데이터에 대한 HTML 양식 작성
- 클라이언트로부터 제출된 form 및 데이터 수신 및 처리

이 모든 작업을 수동으로 수행하는 코드를 작성하는 것은 가능하지만 장고가 모든 작업을 처리할 수 있다.

## 장고 안의 폼

HTML 양식을 간단히 설명했지만, HTML \<form\>은 필요한 부품중 하나일 뿐이다.

웹 앱 프로그램의 컨텍스트에서, 'form'은 HTML \<form\>, 생성된 장고 form, submit될 때 반환된 구조화된 데이터 또는 이 부분들의 end-to-end 작업 콜렉션에 속한다.(번역 다시)

### 장고 폼 클래스

이 구성 시스템의 핵심은 장고의 Form클래스다. 장고 모델이 객체의 논리적 구조, 동작 및 파트가 우리에게 표시되는 방식을 설명하는 것과 거의 같은 방식으로 Form클래스는 form을 설명하고 form이 작동하고 나타나는 방식을 결정한다.

모델 클래스의 필드가 데이터베이스 필드에 매핑되는 것과 같이 form 클래스의 필드는  HTML의 form \<input>요소에 매핑된다.
(ModelForm은 Form을 통해 모델 클래스의 필드를 HTML 폼의 \<input>요소로 매핑한다. 이것이 장고 관리자의 기반이 된다)

form의 필드 자체는 클래스이다. form이 제출되면 form 데이터를 관리하고 유효성 검사를 수행한다. DateField와 FileField는 매우 다른 종류의 데이터를 처리하고 그 데이터와 다른 작업을 수행해야한다.

form 필드는 브라우저 사용자에게 HTML '위젯'으로 표시된다. 각 필드 유형은 기본적인 위젯 클래스가 있지만 필요에 따라 다시 정의할 수 있다.

### 폼을 인스턴스화, 처리하고 렌더링하기

장고에서 객체를 렌더링할 때 우리는 일반적으로 다음을 수행한다.

1. 뷰에서 가져오기 (예를 들어, 데이터베이스에서 가져오기)
2. 템플릿 컨텍스트에 전달하기
3. 템플릿 변수를 사용하여 HTML 마크업으로 확장하기

템플릿에서 폼을 렌더링하는 것은 다른 종류의 객체를 렌더링하는 것과 거의 같지만 몇가지 중요한 차이점이 있다.

데이터를 포함하고 있지 않은 모델 인스턴스의 경우, 템플릿에서 뭘 하는 것이 거의 불가능하다. 

따라서 뷰에서 모델 인스턴스를 처리 할 때 일반적으로 데이터베이스에서 꺼내옵니다. 우리가 폼을 처리할 때 그것을 뷰에서 인스턴트화한다.

폼을 인스턴스화 할 때 폼을 비워두거나 미리 채울 수 있다. 예를들어

- 저장된 모델 인스턴스의 데이터(수정을 위한 관리 폼의 경우)
- 다른 출처에서 수집한 데이터
- 이전 HTML 폼 제출에서 받은 데이터

마지막으로 가장 흥미로운 케이스는 사용자가  웹사이트를 읽기만 하는 것이 아니라 정보를 다시 보낼 수 있게 하기 때문이다.

## 폼 만들기

### 진행되어야 할 일

유저의 이름을 입력받기 위해 웹사이트에서 간단한 폼을 만든다고 가정한다. 템플릿에는 다음과 같이 작성한다.

{% raw %}

```html
<form action="/your-name/" method="post">
    <label for="your_name">Your name: </label>
    <input id="your_name" type="text" name="your_name" value="{{ current_name }}">
    <input type="submit" value="OK">
</form>
```

{% endraw %}

POST 메소드를 사용하여 폼 데이터를 URL **/your-name/**을 반환하도록 브라우저에 지시한다. "Your name:"이라는 텍스트 필드와 "OK"라고 표시된 버튼이 나타난다. 템플릿 컨텍스트에 **cuurrent_name** 변수가 있으면 **your_name** 필드를 미리 채우는 데 사용된다. 

HTML 폼을 포함하는 템플릿을 렌더링하고 current_name 필드를 적절하게 제공할 수 있는 뷰가 필요하다.

폼이 제출될 때 서버에 전송된 POST 요청에는 폼 데이터가 포함된다.

다음으로 해당 /your-name/ URL에 해당하는 뷰가 필요하며 request에서 적절한 키/값 쌍을 찾은 다음 처리한다.

이러한 폼은 매우 간단하다. 실무에서 폼은 수 십 또는 수 백개의 필드가 포함될 수 있으며 그 중 많은 필드가 미리 채워져야 할 수도 있다. 사용자는 작업을 완료하기 전에 편집-제출 싸이클을 여러번 검토해야 할 것이다.

폼을 제출하기 전에도 브라우저에서 일부 유효성 검사를 수행해야 할 수도 있다. 우리는 훨씬 복잡한 필드를 사용하여 사용자가 달력에서 날짜를 선택하는 등의 작업을 수행할 수 있게한다.

이 점에서 우리로썬 장고를 써서 일하는게 훨씬 쉽다.

### 장고에서 폼 형성하기

#### 폼 클래스

우리는 HTML 폼이 어떻게 보여지길 원하는지 이미 알고있다. 장고에서 폼의 시작은 다음과 같다.

**`forms.py`**

```python
from django import forms

class NameForm(forms.Form):
	your_name = forms.CharField(label='Your anme', max_length=100)
```

이는 단일 필드 (your_name)가 있는 폼 클래스를 정의한다. 필드에 인간 친화적인 레이블을 적용함. 레이블이 렌더링될 때 \<label\>에 나타난다.(이 경우 지정된 레이블은 실제로 생략한 경우 자동으로 생성되는 레이블과 동일하다)

필드의 최대 허용 길이는 max_length에 의해 정의된다. 이 것은 두 가지 일을 하는데 HTML \<input\>에 maxlength = "100"을 넣는다. 그러면 브라우저가 사용자로 하여금 100글자 보다 많은 수의 문자를 입력하지 못하도록 한다. 또한 장고가 브라우저에서 폼을 돌려받으면 데이터 길이를 확인한다.

폼 인스턴스는 모든 필드에 대한 유효성 검사 루틴을 실행하는 is_valid() 메소드가 있다.

이 메소드가 호출되었을 때 모든 필드에 유효한 데이터가 들어있다면 다음과 같다.

- True를 돌려준다.
- 폼의 데이터를 그것의 cleaned_data 속성에 배치한다.

전체 폼을 처음 렌더링 할 때 다음과 같이 표시된다.

```html
<label for="your_name">Your name: </label>
<input id="your_name" type="text" name="your_name" maxlength="100" required />
```

여기에서 <form> 태그 또는 제출 버튼이 포함되지 않는다. 

### 뷰

장고 웹 사이트로 보내진 폼 데이터는 일반적으로 뷰에 의해 처리된다. 이 것은 우리가 동일한 로직의 일부를 재사용할 수 있게 해준다.

폼을 처리하기 위해선 게시할 URL에 대한 뷰에서 폼을 인스턴스화 해야한다.

**`views.py`**

```python
from django.http import HttpResponseRedirect
from django.shortcuts import render

from .forms import NameForm

def get_name(request):
    # POST 요청인 경우 폼 데이터를 처리해야한다.
    if request.method == 'POST':
        # 폼 인스턴스를 만들고 request로 온 데이터로 채운다:
        form = NameForm(request.POST)
        # 유효한지 검사:
        if form.is_valid():
            # 필요에 따라 form.cleaned_data에서 데이터를 처리
            # ...
            # 새로운 URL로 리디렉션:
            return HttpResponseRedirect('/thanks/')

    # GET (또는 다른 방법)으로 빈 폼이라면
    else:
        form = NameForm()

    return render(request, 'name.html', {'form': form})
    ```

이 뷰에 GET 요청이 도착하면 빈 폼 인스턴스를 만들어 렌더링할 템플릿 컨텍스트에 배치한다. 이것은 URL을 처음 방문할 때 발생할 수 있는 일이다.

POST 요청을 사용하여 폼을 제출하면 뷰에서 폼 인스턴스를 다시 작성하고 요청한 데이터로 채운다. form = NameForm(request.POST)  "폼에 데이터 바인딩"이라고 한다.(이제는 바운드 형식)

폼의 is_valid() 메서드를 호출한다. True가 아니라면 폼이 있는 템플릿으로 돌아간다. 이번에는 폼이 더 이상 비어있지 않으므로 HTML 폼은 이전에 제출된 데이터로 채워지며 필요에 따라 편집 및 수정할 수 있다.

is_valid()가 True이면 cleaned_data 속성에서 모든 유효성이 검사된 폼 데이터를 찾을 수 있다. 이 데이터를 사용하여 데이터베이스를 업데이트하거나 브라우저로 HTTP 리디렉션으로 보내기 전에 다른 처리를 수행하여 다음에 어디로 가야하는지 알려준다.

### 템플릿

{% raw %}

name.html 템플릿에 많은 작업을 할 필요가 없다. 가장 간단한 예는 다음과 같다.

```html
<form action="/your-name/" method="post">
    {% csrf_token %}
    {{ form }}
    <input type="submit" value="Submit" />
</form>
```

모든 폼의 필드와 속성은 장고의 템플릿 언어에 의한 {{ form }}의 HTML 마크업으로 풀린다.

{% endraw %}

> 폼과 CSRF(Cross Site Request Forgery, 교차 사이트 요청 위조 방지)
> 
> 장고는 CSRF에 대한 보호기능을 쉽게 사용할 수 있다. CSRF 보호가 활성화 된 상태에서 POST를 통해 폼을 제출할 때 앞의 예와 같이 csrf_token 템플릿 태그를 사용하면 된다.

--
> HTML5 인풋 타입과 브라우저 유효
> 
> 양식에 URLField, EmailField 또는 정수 필드타입이 포함된 경우 장고는 URL, 이메일 및 숫자 HTML5 인풋 타입을 사용한다.

> 기본적으로 브라우저는 필드들에 대해 자체적으로 유효성 검사를 적용할 수 있다. 이는 장고의 유효성 검사보다 엄격할 수 있다.
> 이 동작을 비활성화하려면 form 태그에 novalidate 속성을 설정하거나 TextInput과 같이 필드에 다른 위젯을 지정하면 된다.

우리는 이제 장고 폼에 의해 설명되고 뷰에의해 처리되고 HTML \<form\>으로 렌더링 되는 작업이 이루어지는 웹 폼을 가질 수 있게 된다.


## 장고 폼 클래스 더 알아보기

모든 폼 클래스는 django.forms.Form 또는 django.forms.ModelForm의 하위 클래스로 작성된다. ModelForm은 Form의 하위 클래스라고 생각해도 된다. Form과 ModelForm은 실제로 BaseForm 클래스에서 일반적인 기능을 상속받지만 그에 대한 세부 사항은 중요하지 않다.

> 모델과 폼
> 
> 실제로 폼이 장고 모델을 직접 추가하거나 편집하는데 사용한다면, ModelForm으로 많은 시간, 노력 및 코드를 절약할 수 있다. 왜냐하면 모델 클래스에서 적절한 필드와 속성과 함께 폼을 작성하기 때문이다.

### 바운드와 언바운드 폼 인스턴스

바운드와 언바운드 폼의 차이를 아는 것은 중요하다

- 언바운드 폼에는 연결된 데이터가 없다. 사용자에게 렌더링을 하면 비어있거나 기본값을 포함시킨다.

- 바운드 폼은 데이터를 포함하고 있기 때문에 해당 데이터의 유효성을 검사하는데 사용된다. 잘못된 바운드 폼이 렌더링되면 사용자에게 수정해야 할 데이터를 알려주는 인라인 오류 메시지를 포함한다.

폼의 is_bound 속성은 폼에 데이터가 바운드되어 있는지 여부를 알려준다.

### 필드 더 알아보기

개인 웹사이트에서 "contact me" 기능을 구현한다고 가정해보자

**`form.py`**

```python
from django import forms

class ContactForm(forms.Form):
    subject = forms.CharField(max_length=100)
    message = forms.CharField(widget=forms.Textarea)
    sender = forms.EmailField()
    cc_myself = forms.BooleanField(required=False)
```

우리가 이전에 썼던 폼은 하나의 필드만 사용된 **your_name**,  **CharField** 였지만 이번의 폼에서는 subject, message, sender 및 cc_myself라는 네 개의 필드가 있다.
CharField, EmailField 및 BooleanField는 사용 가능한 필드 유형중 세 가지이다. 

### 위젯

각 폼 필드에는 해당 위젯 클래스가 있으며, 이 클래스는 \<input type="text"\>와 같은 HTML 폼 위젯에 해당한다.

대부분의 경우, 필드에는 쓸만한 기본 위젯이 있다. 예를 들어 기본적으로 CharField에는 HTML에서 \<input type="text"\>을 생성하는  TextInput 위젯이 있다. 만약 \<textarea>가 필요하다면, 메시지 필드에 했던 것 처럼 폼 필드를 정의할 때 적절한 위젯을 지정하면 된다.

### 필드 데이터

폼과 함께 제출된 데이터가 무엇이든간에 is_valid()를 호출하여 유효성을 검사하고 is_valid()가 True를 반환하면 폼 데이터는 form.cleaned_data에 사전 형식으로 저장된다. 그 데이터는 파이썬 타입으로 잘 변환될 것이다.

> **노트**
> 
> 이 시점에서 request.POST에서 유효성 검사하지 않은 데이터에 직접 액세스 할 수 있지만 검증된 데이터를 사용하는 것이 더 좋다.

위의 contact 폼 예제에서 cc_myself는 부울값이다. 마찬가지로 IntegerField 및 FloatField와 같은 필드는 값을 각각 Python int 및 Float로 변환한다.

이 폼을 처리하는 뷰에서 폼 데이터를 처리하는 방법은 다음과 같다.

**`views.py`**

```python
from django.core.mail import send_mail

if form.is_valid():
    subject = form.cleaned_data['subject']
    message = form.cleaned_data['message']
    sender = form.cleaned_data['sender']
    cc_myself = form.cleaned_data['cc_myself']

    recipients = ['info@example.com']
    if cc_myself:
        recipients.append(sender)

    send_mail(subject, message, sender, recipients)
    return HttpResponseRedirect('/thanks/')
```

> **Tip**
> 
> 장고로 이메일 보내기에 대해서 더 알고싶다면 [Sending email](https://docs.djangoproject.com/en/2.0/topics/email/) 참고

일부 필드 타입은 추가적으로 처리를 해주어야한다. 예를 들어, 폼을 사용하여 업로드 된 파일을 다른게 처리해야한다. (request.POST보다는 request.FILES에서 검색할 수 있다). 폼으로 파일 업로드를 처리하는 방법에 대한 자세한 내용은 [Binding uploaded files to a form](https://docs.djangoproject.com/en/2.0/ref/forms/api/#binding-uploaded-files) 참고

## 폼 템플릿으로 작업하기

{% raw %}

폼을 템플릿으로 가져오려면 폼 인스턴스를 템플릿 컨텍스트에 배치하기만 하면 된다. 따라서 폼이 컨텍스트에서 폼이라고 하면 {{ form }}은 \<label\> 및 \<input\> 요소를 적절하게 렌더링한다.

### 폼 렌더링 옵션

> 추가적인 폼 템플릿 도구
> 
> 폼 output는 \<form\> 태그 또는 폼의 제출 제어가 포함되지 않는다는 사실을 잊으면 안된다. 직접 제공해야한다.

\<label>/<input> 쌍에 대해서는 다른 출력 옵션이 있다.

- {{ form.as_table }}은 <tr> 태그로 싸인 테이블 셀로 렌더링함

- {{ form.as_p }}은 <p> 태그로 싸인 것을 렌더링함

- {{ form.as_ul }}은 <li> 태그로 싸인 것을 렌더링함

\<table\>또는 \<ul\> 요소를 직접 제공해야함

ContactForm 인스턴스에 대한 {{ form.as_p }}의 결과는 다음과 같다.

```html
<p><label for="id_subject">Subject:</label>
    <input id="id_subject" type="text" name="subject" maxlength="100" required /></p>
<p><label for="id_message">Message:</label>
    <textarea name="message" id="id_message" required></textarea></p>
<p><label for="id_sender">Sender:</label>
    <input type="email" name="sender" id="id_sender" required /></p>
<p><label for="id_cc_myself">Cc myself:</label>
    <input type="checkbox" name="cc_myself" id="id_cc_myself" /></p>
```

{% endraw %}

각 폼 필드에는  id_<field-name>으로 설정된 ID 속성이 있으며, 이는 함께 제공되는 레이블 태그에 의해 참조된다. 이는 폼이 화면 판독기 소프트웨어와 같은 보조기술이 액세스 할 수 있도록 하는데 중요하다. 또한 [레이블과 ID가 생성되는 방식을 사용자정의](https://docs.djangoproject.com/en/2.0/ref/forms/api/#ref-forms-api-configuring-label) 할 수 있다.

더 알아보려면 [HTML으로 출력되는 폼](https://docs.djangoproject.com/en/2.0/ref/forms/api/#ref-forms-api-outputting-html)을 참조

### 필드 직접 렌더링하기

{% raw %}

우리는 장고가 폼의 필드를 자동으로 언팩하도록 할 필요는 없고 원한다면 수동으로 처리할 수 있다. (예를 들어 필드 재정렬) 
각 필드는 {{ form.name_of_field }}를 사용하여 폼의 속성으로 사용할 수 있으며 장고 템플릿에서 적절하게 렌더링된다. 예를 들면

```html
{{ form.non_field_errors }}
<div class="fieldWrapper">
    {{ form.subject.errors }}
    <label for="{{ form.subject.id_for_label }}">Email subject:</label>
    {{ form.subject }}
</div>
<div class="fieldWrapper">
    {{ form.message.errors }}
    <label for="{{ form.message.id_for_label }}">Your message:</label>
    {{ form.message }}
</div>
<div class="fieldWrapper">
    {{ form.sender.errors }}
    <label for="{{ form.sender.id_for_label }}">Your email address:</label>
    {{ form.sender }}
</div>
<div class="fieldWrapper">
    {{ form.cc_myself.errors }}
    <label for="{{ form.cc_myself.id_for_label }}">CC yourself?</label>
    {{ form.cc_myself }}
</div>
```

완전한 \<label> 요소는 label_tag()를 사용하여 생성할 수 있다. 예를 들면

```html
<div class="fieldWrapper">
    {{ form.subject.errors }}
    {{ form.subject.label_tag }}
    {{ form.subject }}
</div>
```

{% endraw %}

### 에러메세지 폼 렌더링하기

{% raw %}

물론 이러한 유연성을 기대하는 것은 더 많은 작업을 요한다. 지금까지 우리는 폼 에러를 표시하는 방법에 대해 걱정할 필요가 없었다. 왜냐하면 이것이 폼 오류를 처리했기 때문이다.

이 예제에서 우리는 각 필드에 대한 오류를 처리하고 폼 전체에 대한 오류를 처리해야한다. 폼 맨 위에 있는 {{ form.non_field_errors }} 및 각 필드의 오류에 대한 템플릿 조회를 참고해라.

{{ form.name_of_field.errors }}를 사용하면 폼 오류 목록이 표시되고 순서가 지정되지 않은 리스트로 렌더링된다. 이것은 다음과 같이 보일 것이다.

```html
<ul class="errorlist">
    <li>Sender is required.</li>
</ul>
```

이 목록에는 오류 목록의 CSS 클래스가 스타일을 지정할 수 있다. 오류 표시에 대해 사용자 정의하면 오류를 반복으로 표시할 수 있다.

```html
{% if form.subject.errors %}
    <ol>
    {% for error in form.subject.errors %}
        <li><strong>{{ error|escape }}</strong></li>
    {% endfor %}
    </ol>
{% endif %}
```

필드가 아닌 오류 (form.as_p()와 같은 헬퍼를 사용할 때 폼의 맨 위에 렌더링되는 숨겨진 필드 오류 및 /또는 비공개 필드 오류)는 필드 별 오류와 구별할 수 있도록 추가적인 필드가 아닌 클래스가 렌더링 된다. 예를 들어, {{ form.non_field_errors }}는 다음과 같다.

```python
<ul class="errorlist nonfield">
    <li>Generic validation error</li>
</ul>
```
{% endraw %}

오류, 스타일 및 템플릿의 폼 속성 작업에 대한 자세한 내용은 [폼 API](https://docs.djangoproject.com/en/2.0/ref/forms/api/)를 참조.

### 폼 필드 루핑하기

{% raw %}

각 폼 필드에 대해 동일한 HTML을 사용하는 경우 {% for %} 루프를 사용하여 차례대로 각 필드를 반복하여 중복 코드를 줄일 수 있다.

```html
{% for field in form %}
    <div class="fieldWrapper">
        {{ field.errors }}
        {{ field.label_tag }} {{ field }}
        {% if field.help_text %}
        <p class="help">{{ field.help_text|safe }}</p>
        {% endif %}
    </div>
{% endfor %}
```

**{{ field }}** 에 쓸 수 있는 유용한 속성은 다음과 같다.

**{{ field.label }}**

Email address같은 필드의 레이블

**{{ field.label_tag }}**

필드의 레이블이 적절한 HTML \<label> 태그에 싸인다. 여기에는 폼의 label_suffix가 포함된다. 예를 들어 기본 label_suffix는 콜린이다

```html
<label for="id_email">Email address:</label>
```

**{{ field.id_for_label }}**

이 필드에서 사용할 ID(위의 예에서 id_email). 레이블을 수동으로 구성하는 경우 label_tag 대신 이 레이블을 사용할 수 있다. 예를 들어 인라인 자바스크립트가 있고 필드의 ID를 하드코딩하지 않으려는 경우에도 유용하다.

**{{ field.value }}**

필드 값. 예) **someone@example.com**

**{{ field.html_name }}**

인풋 원소의 이름 필드에 사용될 필드의 이름. 폼 접두어가 설정되어 있으면 이를 계정으로 가져온다.

**{{ field.help_text }}**

필드와 관련된 도움말 텍스트

**{{ field.errors }}**

이 필드에 해당하는 유효성 검사 오류가 포함된  **\<ul class="errorlist">**를 출력합니다. 오류에 대한 {% for error in field.errors %} 루프의 오류를 사용하여 오류 표시를 사용자 정의할 수 있다. 이 경우, 루프의 각 객체는 에러 메시지를 포함한 간단한 문자열이다.

**{{ field.is_hidden }}**

이 특성은 폼 필드가 숨겨진 필드이면 True이고 그렇지 않으면 False이다. 템플릿 변수로 딱히 유용하지는 않지만 다음과 같은 조건부 테스트에 유용할 수 있다.

```html
{% if field.is_hidden %}
   {# Do something special #}
{% endif %}
```

**{{ field.field }}**

이 BoundField가 wraping하는 폼 클래스의 Field인스턴스이다. 이 속성을 사용하여 필드 속성에 액세스 할 수 있다. {{ char_field.field.max_length }}

> **함께 보기**
> 
> 속성과 메서드에 관한 전체 목록은 [BoundField](https://docs.djangoproject.com/en/2.0/ref/forms/api/#django.forms.BoundField)를 참조

{% endraw %}

### 숨겨지거나 보이는 필드의 루핑

장고의 기본 폼 레이아웃에 의존하는 것과 달리 템플릿의 폼을 수동으로 배치하는 경우 \<input type="hidden"> 필드를 보여지는 필드와 다르게 처리하고 싶을 수 있다.
예를 들면, 숨겨진 필드에는 아무것도 표시되지 않으므로 오류 메시지를 필드 옆에 배치하면 사용자에게 혼동을 줄 수 있으므로 해당 필드의 오류를 다르게 처리해야한다.

장고는 hidden_fields()와 visible_fields() 각각 루프할 수 있는 두 가지 메서드를 제공한다. 다음은 이 두 가지 방법을 사용하기위해 수정된 예제이다.

{% raw %}

```html
{# Include the hidden fields #}
{% for hidden in form.hidden_fields %}
{{ hidden }}
{% endfor %}
{# Include the visible fields #}
{% for field in form.visible_fields %}
    <div class="fieldWrapper">
        {{ field.errors }}
        {{ field.label_tag }} {{ field }}
    </div>
{% endfor %}
```

이 예제는 숨겨진 필드의 오류를 처리하지 않는다. 일반적으로 숨겨진 필드의 오류는 폼 조작이 정상적인 폼 상호작용으로 변경되지 않으므로 폼 변조의 신호다. 그러나 폼 오류에 대한 오류 표시도 쉽게 삽입할 수 있다.

### 재사용가능한 폼 템플릿

사이트에서 여러 위치의 폼에 동일한 렌더링 로직을 사용하는 경우 폼의 루프를 독립형 템플릿에 저장하고 include 태그를 사용하여 다른 템플릿에서 재사용하여 중복을 줄일 수 있다.

```html
# In your form template:
{% include "form_snippet.html" %}

# In form_snippet.html:
{% for field in form %}
    <div class="fieldWrapper">
        {{ field.errors }}
        {{ field.label_tag }} {{ field }}
    </div>
{% endfor %}
```

템플릿에 전달된 폼 객체가 컨텍스트 내에서 다른 이름을 갖는 경우 include 태그의 with 인수를 사용하여 별칭을 지정할 수 있다.

```html
{% include "form_snippet.html" with form=comment_form %}
```

자주 이런 일을 하는 경우에는 맞춤식 태그를 만드는 것이 좋다.

<끝>






