---
title: "python"
date: 2019-11-11
categories: kangring
---

## 리스트 컴프리헨션
https://wikidocs.net/22805
>>> [x for x in range(10)]

### 데코레이터
<http://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0-decorator/>

### mutable 데이터 타입과 immutable 데이터타입 
<https://webnautes.tistory.com/1181>
```
*mutable 객체 
객체를 생성한 후, 객체의 값을 수정 가능, 변수는 값이 수정된 같은 객체를 가리키게됨, 예)  list, set, dict
immutable 객체

*객체를 생성한 후, 객체의 값을 수정 불가능, 변수는 해당 값을 가진 다른 객체를 가리키게됨, 예) int, float, complex, bool, string, tuple, frozen set
```


### 이터레이터 제너레이터 컨테이너
<https://offbyone.tistory.com/83>
<http://pythonstudy.xyz/python/article/23-Iterator%EC%99%80-Generator?
```
            while True:
                try:
                    v = next(it)
                    print(v.id)
                except StopIteration:
                    break
```
