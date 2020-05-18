---
title: 복수 APP에서 templates 설정
category: django
order: 1
date: 2019-10-11
---

```
Django 개발 가이드라인은 "App폴더/templates/App명/템플릿파일" 처럼, 각 App 폴더 밑에 templates 서브폴더를 만들고 
다시 그 안에 App명을 사용하여 서브폴더를 만든 후 템플릿 파일을 그 안에 넣기를 권장한다 (예: /home/templates/home/index.html ).
이는 만약 복수의 App들이 동일한 이름의 템플릿을 가진 경우, View에서 잘못된 템플릿을 가져올 수 있기 때문인데, 
예를 들어, App1에 create.html이 있고, App2에 동일한 create.html 템플릿이 있는 경우, 
App2의 View에서 create.html를 지정하면, 처음 App1의 create.html을 사용하게 된다. 
이는 템플릿을 찾을 때 자신의 App 내의 템플릿을 먼저 찾는 것이 아니라, 
전체 App들의 템플릿 폴더들을 처음부터 순서대로 찾기 때문이다. 
View에서 "App2/create.html" 과 같이 템플릿명을 지정하면 이런 혼동은 없어진다.
템플릿은 물론 순수하게 HTML로만 쓰여진 Static HTML 파일일 수는 있지만, 
거의 대부분의 경우 View로부터 어떤 데이타를 전달받아 HTML 템플릿 안에 그 데이타를 동적으로 치환해서 사용한다. 
예를 들어, 위의 index 뷰에서 message 라는 데이타를 index.html 이라는 템블릿에 전달하고 그 템플릿 안에서 이를 사용하기 위해서 다음과 같이 할 수 있다.

+app1
--+templates
----+app1
------+index.html
+app2
--+templates
-----+app2
------+index.html		

호출
render(request, "app1/index.html")
render(request, "app2/index.html")
```


## pycharm에서 mysqlclient인스톨

```
설정->project->pacage에서 mysqlclient인스톨
이때 OSError: mysql_config not found 나는 경우에는 

1. brew로 mysql을 설치
터미널에서 brew install mysql 명령어를 수행하여 설치
2. 환경변수 구성
자신의 홈 디렉토리 안의 .bash_profile 등 환경 설정 파일에 아래의 변수를 추가해준다.
export PATH=$PATH:/usr/local/mysql/bin
변수 등록 전 저 경로에 mysql이 있는지 확인을 해보고 없는 경우 경로를 수정해줘야 한다.
술자의 경우 저 경로에 존재하지 않아서 해당 경로를 다시 확인하였고 /usr/local/opt/mysql/bin 이었다.
등록 후 source .bash_profile로 적용을 해주자.
3. 확인 및 재설치
터미널에서 `which mysql_config’ 를 수행하여 정상적인 경우 아래의 사진처럼 경로가 뜰 것이다.
만약 안뜬다면 mysql 경로가 잘못된 경우이므로 확인 후 수정을 하도록 하자.

4. 확인 및 재설치
3번까지 별 이상이 없는 경우 다시 mysqlclient==1.3.13을 설치해본다.
참조 : https://elfinlas.github.io/2019/01/23/pip-mysql-error/

설치에러일경우
brew install openssl
LDFLAGS=-L/usr/local/opt/openssl/lib pip install mysqlclient
<https://medium.com/@elastic7327/osx-mojave-%EC%97%90%EC%84%9C-python-%ED%8C%A8%ED%82%A4%EC%A7%80-mysqlclient%EA%B0%80-%EC%84%A4%EC%B9%98%EA%B0%80-%EC%95%88%EB%90%98%EB%8A%94-%EB%AC%B8%EC%A0%9C-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95-2269bcf49c33>


```


## django 
<https://hangpark.com/django-multi-db-relation/>


## django 클래스뷰 CBV
```
1) 컨텍스트 변수 : object_list
2) 템플릿 파일 : bookmark_list.html (모델명소문자_list.html)


함수형 뷰에서반복되는 공통 부분을 패턴화하여 사용하기 쉽게 추상화 해두었다.
아래와 같이 최소한 그 뷰가 어떤 모델을 사용할 것인지만 지정해주면 나머진 모두 상위 클래스(여기서는 List generic view)가 모든 걸 알아서 해준다.
뷰는 간단 명료해야 한다.
뷰 코드의 양은 적으면 적을수록 좋다.
뷰 안에서 같은 코드를 반복적으로 사용하지 않는다.
뷰는 프레젠테이션 로직에서 관리하고 비즈니스 로직은 모델에서 처리한다. 매우 특별한 경우에만 폼에서 처리한다.
403, 404, 500 에러 핸들링에는 CBV를 이용하지 않고 FBV를 이용한다.
믹스인은 간단명료해야 한다.
https://wikidocs.net/9623

```

## Form
<https://wayhome25.github.io/django/2017/05/06/django-form/><br>
<https://docs.djangoproject.com/en/2.2/ref/forms/widgets/><br>
<https://django-doc-test-kor.readthedocs.io/en/old_master/topics/forms/index.html><br>
```
주요역할 (custom form class)
입력폼 html 생성 : as_table(), as_p(), as_ul() 기본 제공
입력폼 값 검증 (validation)
검증에 통과한 값을 사전타입으로 제공 (cleaned_data)

Form vs Model Form (폼과 모델폼의 차이점)
Form (일반 폼) : 직접 필드 정의, 위젯 설정이 필요
Model Form (모델 폼) : 모델과 필드를 지정하면 모델폼이 자동으로 폼 필드를 생성

# Form (일반 폼)
class PostForm(forms.Form):
	title = forms.CharField()
	content = forms.CharField(widget=forms.Textarea)

# Model Form (모델 폼)
class PostForm(forms.ModelForm):
	class Meta:
		model = Post
		fields = ['title', 'content']

```
* widgets 
----------
<https://docs.djangoproject.com/en/2.2/ref/forms/widgets/>

```
위젯은 Django가 HTML 입력 요소를 표현한 것입니다. 위젯은 HTML 렌더링 및 위젯에 해당하는 GET / POST 사전에서 데이터 추출을 처리합니다.
내장 위젯에 의해 생성 된 HTML은 HTML5 구문 (targeting)을 사용 합니다. 예를 들어, XHTML 스타일 대신 과 같은 부울 속성을 사용합니다
```

## 로그인
초보몽키 <a href='https://wayhome25.github.io/django/2017/03/01/django-99-my-first-project-2/'><br>
Django 자습서 인증 <https://wikidocs.net/6650><br>
<https://jamanbbo.tistory.com/16><br>
<https://ssungkang.tistory.com/entry/Django-10-%ED%9A%8C%EC%9B%90%EA%B0%80%EC%9E%85%EB%A1%9C%EA%B7%B8%EC%9D%B8%EB%A1%9C%EA%B7%B8%EC%95%84%EC%9B%83-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0>  <br>
하위 내용발췌 <https://blog.hannal.com/2015/06/start_with_django_webframework_08/><br>

```
(1) Django 내장 인증 기능
Django에 내장된 인증 체계는 django.contrib.auth라는 경로(name space)인 Python 패키지에 모여 있으며, Django 개념으로는 Django App으로
기본 내장되어있음 


```

## User model
패스워드 변경 <https://ssungkang.tistory.com/entry/DjangoUser-%EB%B9%84%EB%B0%80%EB%B2%88%ED%98%B8-%EB%B3%80%EA%B2%BD%ED%95%98%EA%B8%B0-checkpassword>

## 미들웨어

<https://gyukebox.github.io/blog/django-%EC%BB%A4%EC%8A%A4%ED%85%80-%EB%AF%B8%EB%93%A4%EC%9B%A8%EC%96%B4-%EB%A7%8C%EB%93%A4%EA%B8%B0---rest-framework-%EB%A5%BC-%EC%9C%84%ED%95%9C-http-response-formatting/>
```
* 쉽게 말해서, http 요청 / 응답 처리 중간에서 작동하는 시스템이다.
장고는 http 요청이 들어오면 미들웨어를 거쳐서 해당 URL 에 등록되어 있는 뷰로 연결해주고, http 응답 역시 미들웨어를 거쳐서 내보낸다.
1.장고는 http request 가 들어오면 위에서부터 아래로 미들웨어를 적용시킨다.
2.장고는 http response 가 나갈 때 아래서부터 위로 미들웨어를 적용시킨다.
```
