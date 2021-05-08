# Django 명령어 모음

## 1. Model 생성 전

### 폴더 제작

- mkdir practice
- cd practice : 폴더로 이동
- which python : 아직은 global 환경 설정

### 가상환경 제작

- python -m venv venv(가상환경 이름)
- source venv/Scripts/activate : vs 내부에서도 실행 가능
- which python : virtual 환경으로 바뀜
- pip list : 현재 깔려있는 module 확인

### 쟝고 및 프로젝트, 앱 설치

- pip install django : 쟝고 설치
- django-admin startproject practice : 'practice'라는 project 시작
- code . : vscode 열기 / Terminal을 Git Bash 에서 vscode로 전환

### 그 외의 것들

- touch .gitignore
- Requirements.txt 
  - pip freeze>requirements.txt : 해당 모듈 버젼 txt로 옮기기
  - pip install -r requirements.txt : 나중에 다운 받은 사람이 모듈 다운로드 하기

### 서버 구동 확인 및 앱 생성

- python manage.py runserver : 로켓이 보임
- python manage.py startapp orm : 앱 생성

### 앱 생성 후 해줘야 할 것

- setting(project) installed apps 추가
- urls(project) 에 path 추가(import include, path('', include()))

### 앱 내부에서 해줘야 할 것

- cd orm : orm app으로 위치 변경

- touch urls.py : urls.py 생성 후 경로 설정

  ``` urls.py 내부에서 할 것들
  urls.py
  - path import : 주기 위해서(head)
  - views와 연결 : 주기 위해서(head)
  - urlpatterns : body
  ```

- views 

  ```
  - render, redirect, HttpResponse Import
  - model과 연동
  ```

### Templates 

- Base.html 생성
  - mkdir templates
  - cd templates
  - touch base.html
- Base.html 밑처리
  
  - Bootstrap 코드, Block(title, boyd) 추가, 프로젝트 setting 경로 추가(BASE_DIR / 'templates')
- app 내부에서 html 파일 작성
  - mkdir templates
  - mkdir orm
  - cd orm
  - touch __.html
  - app/orm 폴더 app/templates 로 이동

  

## 2. Model 생성 및 이후

- pip install django_extension iptyhon : shell_plus 사용하기 위해 module 설치
- project setting에서 'django_extension' 추가
- app 내부의 model 이동 후 class 생성 후 설계도 및 추가
  - python manage.py makemigrations app : app만 makemigration처리
  - python manage.py migrate app : sqlite이 다운되있으면 sqlite3 파일 생성
- python manage.py shell_plus : sqlite에 data추가하기 위해 연결
- s.save() : 인스턴스 생성 후 저장 후 sqlite 변경사항 확인

Model이 생겼으니 app 내부에서 urls->views->templates 반복 작업

```
urls:
- C(new: 입력 render / create: 입력 데이터 처리 redirect)
- R(Index: 전체 render / detail: 개별 render key)
- U(edit: 수정 render / update: 수정된 데이터 처리 redirect key)
- D(delete: 제거 redirect key)
```

```
views:
- model import class
- 조회 명령어: class.objects.all() / class.objects.get(id=pk)
```

```
templates:
- extends base.html
- delete(onclick 기능)
- POST: Token 추가
```



## 3. Superuser(Admin) 관리

- python manage.py createsuperuser : user 생성

### Model에 Admin 계정추가

- from django.contrib.auth.models import User

### Admin 사이트 내용 관리

```
from django.contrib import admin
from . import models

class MovieAdmin(admin.ModelAdmin):
    list_display = (
        'title',
        'content',
        'created_at',
        'updated_at',
        'author'
    )
    list_display_links = (
        'title',
        'author'
    )admin.site.register(models.Movie , MovieAdmin)
```