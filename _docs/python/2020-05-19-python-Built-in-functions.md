---
title: Python 내장함수 (Built-in Functions) 모음
category: python
order: 1
date: 2020-05-19
comments: true
---


### math 모듈 함수
* [min()](https://docs.python.org/ko/3/library/functions.html#min)

 ```
iterable 에서 가장 작은 항목이나 두 개 이상의 인자 중 가장 작은 것을 돌려줍니다.
a = [1,2,3,4,5]
b = (1,2,3,4,5)   
print(min(a))
print(min(b))

result
1
1
 ```

* [max()](https://docs.python.org/ko/3/library/functions.html#max)

 ```
iterable 에서 가장 큰 항목이나 두 개 이상의 인자 중 가장 큰 것을 돌려줍니다.
a = [1,2,3,4,5]
b = (1,2,3,4,5)   
print(max(a))
print(max(b))

result
5
5
 ```

### string 모듈 함수
* [join()](https://zetawiki.com/wiki/%ED%8C%8C%EC%9D%B4%EC%8D%AC_join())
 ```
리스트의 특정 구분자를 추가하여 문자열로 변환
list = ['a','b','c']
print( ",".join(list) ) # a,b,c
 ```

* [split()](https://zetawiki.com/wiki/%ED%8C%8C%EC%9D%B4%EC%8D%AC_split())
 ```
str 문자열을 ','를 구분으로 하여 l 리스트로 변경
list = ['a','b','c']
print( ",".split(list) ) #  ['a', 'b', 'c']
 ```
