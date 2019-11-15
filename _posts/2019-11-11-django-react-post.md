---
title: "django와react js 연동"
date: 2019-11-11
categories: kangring
---

## 들어가기전
1.백엔드와 프론트엔드를 분리구성: 각각의 서버와 url로 서로를 연결하는방식 대신에 Cross-Domain 잇슈가 있기 때문에 Cors구성해야한다.
```
django는 uWSGI나 gunicorn을 통해 항상 rest-api request에 응답할 수 있는 상태가 돼야하고 외부에는 
react로 만들어진 frontend가 노출돼있어야한다. 사실상 모든 CRUD작업이 django-rest-framework에 
의해서 이뤄지므로 GET, POST, PUT, DELETE작업에 대한 인증 작업 및 구분을 제대로 구현해야 한다. 
한줄요약하자면 django와 react사이의 내부네트워크를 구성한 다음에 react와 client의 외부네트워크를 구성시켜야 한다
```

2.동일한 호스트에서 제공: 동일한 url에서 앱을 제공하므로 Cors overhead가 없다. 작은크기의 앱의 쉽게 유지 관리할수있다. 
간단한 사이트 및 두개의 별도의 서버를 구성하고 싶지 않을 경우
<br/><br/>
[개념참고](https://this-programmer.com/entry/%EA%B0%84%EB%8B%A8%ED%95%9C-react-JS-Django-%EC%96%B4%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EB%A7%8C%EB%93%A4%EA%B8%B0)

**CORS (Cross-Origin Resource Sharing)**

- 요즘 사용되는 모던 브라우저는 자바스크립트 인터프린터가 도입됐으며 보안상의 문제를 막기위해 JS의 동일 출처 정책으로
Cross-Domain 이슈를 제한함(Cross-Domain Policy)
- 보안상의 문제 없이 Ajax등의 통신을 하기 위해 사용되는 메커니즘이 CORS이다.
- CORS 표준은 웹 브라우저가 사용하는 정보를 읽을 수 있도록 허가된 출처 집합를 서버에게 알려주도록 
허용하는 HTTP 헤더를 추가함으로써 동작

## 종합
첫번째 방식으로 프로젝트를 구성한다. 대신 첫번째 방식은 SPA의 특성상 검색엔진의 최적화가 되지 않기 때문에 SEO를 위한 **SSR**의 추가 작업필요


## Django Rest Framework
우선 Django부터 설치한다. Django에 대한 설치 방법은 별도로 이야기 하지 않는다.

**1.DRF설치**
```
$ pip3 install djangorestframework
```

설치가 완료되면 setting.py의 apps에 추가해준다.
```
/myapp/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django.contrib.humanize',
    'cms',
    'rest_framework', # 추가
]

#이하 추가
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.AllowAny',
    ]
}

```

**2.django-cors-headers 설치**

분리된 서버에서는 Cross-Domain의 잇슈가 있기때문에 script에서의 api통신을 통한 데이터의 접근제어를 위해 (CORS : Cross-Origin Resource Sharing)를 추가해준다.
```
/myapp/settings.py

$ pip3 install django-cors-headers

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django.contrib.humanize',
    'cms',
    'rest_framework', # 추가
    'corsheaders', # 추가
]

MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',  # 추가
    'django_hosts.middleware.HostsRequestMiddleware',
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'django_hosts.middleware.HostsResponseMiddleware',
]

CORS_ORIGIN_WHITELIST = ['http://localhost:3000']
#script안에서의 리소스 요청을 허용할 도메인 추가
```

**3.DRF API 구성**

fornt에 데이터를 전송 할 수있는 샘플 API를 구성한다. 
자세한 내용은 링크 참조
[DF API구성](https://this-programmer.com/entry/%EA%B0%84%EB%8B%A8%ED%95%9C-react-JS-Django-%EC%96%B4%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EB%A7%8C%EB%93%A4%EA%B8%B0)

**4.react 설치**
```
$ npm install -g create-react-app # 글로벌로 cra 설치
$ create-react-app app # cra로 app이라는 리액트 앱 만들기
$ cd app
$ npm start
```

**5.react에서 서버 데이터 수신하기**

[참고](https://this-programmer.com/entry/%EA%B0%84%EB%8B%A8%ED%95%9C-react-JS-Django-%EC%96%B4%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EB%A7%8C%EB%93%A4%EA%B8%B0)


## 서버 deploy

**1.react deploy**

<ul>
    <li>SPA의 정적파일을 S3에 배포하고 CloudFront를 통한 웹호스팅을 한다. </li>
    <li>AWS CLI을 통하여 local에서 s3에 소스를 deploy한다.</li>
</ul>

**소스 배포**
```
$ aws s3 sync ./build s3://버켓명 --profile=AMI사용자명

혹은 package.json에 추가 
"scripts": {
    "deploy":aws s3 sync ./build s3://버켓명 --profile=IAM사용자명
 }
 
 $ yarn deploy
```

**캐시 Purg**
```
aws cloudfront create-invalidation --distribution-id {CDN ID} --paths "/*"

```
[참고](https://react-etc.vlpt.us/08.deploy-s3.html)




## 에러대응

**1.새로고침 했을때 Access Denied

페이지 캐싱을 확인해보고 cdn의 error page탭의 커스텀 해준다.
에러 코드 403을 200으로 Response Page path는 inex.html

[참고1](https://stackoverflow.com/questions/50299204/receive-accessdenied-when-trying-to-access-a-reload-or-refresh-or-one-in-new-tab)

[참고2](https://medium.com/@tech.yangs/cloudfront-error-cache-%EC%82%BD%EC%A7%88%EA%B8%B0-cd9b21affaa2)


