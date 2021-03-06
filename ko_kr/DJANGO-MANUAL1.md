<h1>Django EZ Manual</h1>

<h4>Django 란?:</h4>

1. Python으로 작성된 Open Source 웹 애플리케이션 프레임워크이다.
2. 장고는 MVC(모델-뷰-컨트롤러) 패턴을 따르고 있다. 하지만 컨트롤러의 기능을 프레임워크 자체에서 하기 때문에  
모델, 템플릿, 뷰로 분류해 MTV 프레임워크라고 보기도 한다.
3. 장고는 데이터베이스 기반의 웹사이트를 작성하는데 있어서 수고를 더는 것이 목표이다.
4. 장고는 컴포넌트의 재사용성, 플러그인화 가능성, 빠른 개발 등을 강조하고 있다.
5. 장고는 "DRY(Don't repeat yourself: 중복배제)" 원칙을 따른다.

---

<h3>목차</h3>

0. 들어가기 전...

1. 간단한 설명 및 프로젝트 구조

2. URL 요청과 응답

3. 모델과 관리자페이지

4. 뷰와 템플릿

5. 정적 파일

6. 테스팅

7. 재사용 가능한 앱 패키징

---

<h2>0. 들어가기 전...</h2>

이제 부터 대괄호(대괄호는 왼쪽에 있는 기호가 대괄호다. [] )로 묶인 것은 자신이 마음대로 이름을 지정하거나, 혹은 자신의 설정에 맞는 값을 의미한다.  
예를 들어:  
`$ django-admin startproject [프로젝트 이름]` 을 쓰라고 하면  
자기 마음대로  
`$ django-admin startproject jinho` 라고 자기 마음대로 적을 수 있고,  
`/home/[자신의 하위 디렉토리 명]` 이면,  
자신의 기존 디렉토리 이름에 맞게 받아들여야 한다.

마지막으로..  
무엇이 자신이 마음대로 바꿔도 되는 이름인지 자세한 설명은 하지 않을 것이다. (귀찮다..)

---

<h2>1. 간단한 설명 및 프로젝트 구조</h2>

우선 Django가 잘 설치되어 있는지 버전을 확인해보자  
`$ python -m django --version`

확인되었다면 이제 Django프로젝트를 만들어 보자  

<h4>하지만 그 전에!!</h4>

<strong>  
`PHP` 를 작성해본 경험이 있는 사람은 아마 코드 전체를 `/var/www/html/`과 같은  
DocumentRoot 에 두려는 경향이 있는데  
Django 에서는 되도록이면 `/var/www/html/` 과 같은 곳에 만들지 말자  
보통은 `/home/[유저]/` 과 같은 DocumentRoot 의 바깥에 두는 것을 권한다.  
외부 사람들이 웹을 통해 Python 코드를 직접 열어볼 수 있는 위험이 있기 때문이다. 
</strong>  

<br>
<br>

확인 했다면 이제 만들어보자  
`$ django-admin startproject [프로젝트 이름]`  


만들었다면 무엇이 생성되었는지 확인해 보자.  
```text
[프로젝트 이름]/  
    manage.py  
    [프로젝트 이름]/  
        __init__.py  
        settings.py  
        urls.py  
        wsgi.py  
        asgi.py  
```
<h6>각각의 파일 기능과 의미는 이번 단원의 마지막에 상세하게 나온다.</h6>
같은가??? 아마 asgi.py는 생성이 안되었을 수도 있다. (하지만 괜찮다.)

프로젝트 디렉토리로 이동한 뒤 프로젝트를 동작해보자.  
`$ python manage.py runserver`  

자, 이제 https://127.0.0.1:8000/ 을 통해 접속해보자.  
그러면 로켓이 발사되는 모습을 볼 수 있다.  

만약, 포트를 변경하여 접속하고 싶다면: `python manage.py runserver 0:[포트번호]` 로 실행하면 된다.  

그럼 이제 프로젝트에 필요한 앱을 만들어 보자  
앱 생성: `python manage.py startapp [앱 이름]`

만들어진 앱의 디렉토리를 보자  

```text
[앱 이름]/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
    urls.py
```

<h6>각각의 파일 기능과 의미는 바로 아래 상세하게 나온다.</h2>  

---

만약 이 문서의 모든 단원을 모두 읽는다면 당신은 다음과 같은 프로젝트 구조를 가지게 될 것이다.  

```text
[프로젝트 이름]/
    manage.py   # 프로젝트와 상호작용하는 커맨드라인 유틸리티
    [프로젝트 이름]/   # 프로젝트를 위한 실제 py 패키지 저장
        __init__.py   # 이 디렉토리를 패키지로 다루라고 말하는 단순한 빈 파일
        settings.py   # 프로젝트의 환경 및 구성 저장
        urls.py   # 이 프로젝트의 URL 선언 저장 (어떻게 보면 "목차 ?", "필터 ?"의 기능을 한다고 봐도 됨)
        asgi.py   # ASGI 호환 웹 서버가 프로젝트를 처리할 수 있는 진입점
        wsgi.py   # 현재 프로젝트를 서비스하기 위한 WSGI 호환 웹 서버의 진입점
    [앱 이름]/   # 앱 패키지
        __init__.py   # 이 디렉토리를 패키지로 다루라고 말하는 단순한 빈 파일
        admin.py   # 앱의 관리자 사이트에 대한 설정
        apps.py   # 앱에 대한 기본 설정 정보를 담음
        migrations/   # 데이터베이스 설정 파이를 담고 있는 디렉토리
            __init__.py   # 이 디렉토리를 패키지로 다루라고 말하는 단순한 빈 파일
            0001_initial.py   # 데이터베이스 정보를 담고있는 파일
        models.py   # 데이터 모델에 대한 정보를 정의하고 저장하는 파일, 테이블을 생성하고 테이블의 필드가 정의된 파일
        static/   # 정적 파일의 정보를 담은 파일 ex) 이미지, CSS, json 파일 등등
            [앱 이름]/   # Django가 구분하기 위해 만든 디렉토리
                images/   # 이미지 파일을 담는 디렉토리 (이미지 파일을 쓸 일이 없다면 없어도 된다.)
                    [사진].jpg   # 설명 생략
                    ...
                [css 파일이름].css   # 설명 생략
                ...
        templates/   # 프론트 작업, 페이지의 디자인이 이루어질 디렉토리
            [앱 이름]/   # Django 가 구분하기 위해 만든 디렉토리
                [html 이름1].html   # 설명 생략
                [html 이름2].html   # 설명 생략
        tests.py   # 앱을 테스트 해보기 위한 파일(QA는 중요하니깐)
        urls.py   # views 에 대한 url 설정
        views.py   # 특정 기능과 templates 를 제공하는 파일 
    templates/   # 프로젝트의 템플릿 커스터마이징 디렉토리
        admin/   # Django 가 구분하기 위해 만든 디렉토리
            [...].html   # 설명 생략
```

이미 웹 프레임워크에 익숙하다면 감을 잡았을 수도 있지만 그렇지 않다면,  
아직 감이 잘 오지 않을 것이다.  
이제 본격적으로 알아보며 감을 잡아보자

---

<h2>2. URL 요청과 응답</h2>

<h3>첫 번째 뷰 제작</h3>

`[앱 이름]/views.py` 에 들어가 다음의 코드를 입력해 보자

```python
from django.http import HttpResponse

def main(request):
    return HttpResponse("Hello")
```

앱을 처음 제작하면 `[앱 이름]` 디렉토리 안에 `urls.py` 가 없다.  
만들도록 하자.  

이 `[앱 이름]/urls.py` 를 이용해 `views.py` 에 있는 함수(method)를 호출한다.  
그럼 이제 최상위 `[프로젝트 이름]/urls.py` 에서 `[앱 이름]/urls.py` 를 보게 설정해야 한다.  
`[프로젝트 이름]/urls.py` 에 들어가서  

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('[앱 이름]/', include('[앱 이름].urls')),
    path('admin/', admin.site.urls),
]
``` 

그럼 이제 `[앱 이름]/urls.py` 가 `views.py` 를 보게 하자.  
`[앱 이름]/urls.py` 에 들어가서 

```python
from django.urls import path
from . import views

app_name = '[앱 이름]'
urlpatterns = [
    # ex: /polls/
    path('', views.main, name='main'),
]
```

<strong> 파이썬을 잘 안다면 어떤 것의 이름은 자기가 알아서 설정해도 된다는 것을 알 것이다. </strong>

<h4>path() 함수에 대한 설명</h4>  

```text
path() : 2개의 필수 인수 route, view // 2개의 선택 인수 kwargs, name 총 4개의 인수가 있다.  
path() : route  
        일치하는 패턴을 받는다. ex) 값이 'main' 이라 하면  ==> http://127.0.0.1/main/ 에서 "/main/" 과 매치됨
path() : view  
        일치하는 패턴을 찾으면 HttpRequest 객체를 첫번째 인수로 하고 경로로부터 '캡처된' 값을 키워드로 함수 호출
        ex) views.main ==> views.py 의 main 함수 호출
path() : kwargs  
        임의의 키워드 인수를 view에 사전형으로 전달한다.
path() : name  
        URL 에 이름을 짓는다. 이를 이용하여 템플릿을 포함한 Django 어디에나 명확하게 참조가 가능하다.
        단 하나의 파일만 수정해도 프로젝트 내의 모든 URL 패턴을 바꿀 수 있도록 도와준다. (URL Reversing을 위해)
```

<h3>요청과 응답의 과정</h3>

```text
"클라이언트" ==127.0.0.1:8000/[앱 이름]/==> "서버"
                        전달
"서버":
1. [프로젝트 이름]/urls.py 
2. [앱 이름]/urls.py
3. [앱 이름]/views.py

1. --> 2. --> 3. 

"서버" : 1. [프로젝트 이름]/urls.py 에서 '/[앱 이름]/' 확인 후 [앱 이름]/urls.py 호출 
         2. [앱 이름]/urls.py 에서 /'' 확인 views.main 함수 호출 
         3. views.main 함수 리턴 값 반환
```

다음 <strong> "3. 모델과 관리자페이지" </strong> 는 <strong>DJANGO-MANUAL2</strong> 에서 이어진다.