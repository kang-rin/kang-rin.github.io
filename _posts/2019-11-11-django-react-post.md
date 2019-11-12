---
title: "django와ract js 연동"
date: 2019-11-11
categories: kangring
---

## 들어가기전
1.백엔드와 프론트엔드를 분리구성: 각각의 서버와 url로 서로를 연결하는방식 대신에 Cross-Domain 잇슈가 있기 때문에 cros구성해야한다.
```
django는 uWSGI나 gunicorn을 통해 항상 rest-api request에 응답할 수 있는 상태가 돼야하고 외부에는 react로 만들어진 frontend가 노출돼있어야한다. 사실상 모든 CRUD작업이 django-rest-framework에 의해서 이뤄지므로 GET, POST, PUT, DELETE작업에 대한 인증 작업 및 구분을 제대로 구현해야 한다. 한줄요약하자면 django와 react사이의 내부네트워크를 구성한 다음에 react와 client의 외부네트워크를 구성시켜야 한다
```

2.동일한 호스트에서 제공: 동일한 url에서 앱을 제공하므로 Cors overhead가 없다. 작은크기의 앱의 쉽게 유지 관리할수있다. 
간단한 사이트 및 두개의 별도의 서버를 구성하고 싶지 않을 경우<br/>
[개념참고](https://this-programmer.com/entry/%EA%B0%84%EB%8B%A8%ED%95%9C-react-JS-Django-%EC%96%B4%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EB%A7%8C%EB%93%A4%EA%B8%B0)
