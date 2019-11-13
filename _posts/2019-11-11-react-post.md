---
title: "react 설치 및 기본 구성"
date: 2019-11-11
categories: kangring
---



# Mac에서 react설치 
## 들어가기전
 : [reactjs-kr](https://reactjs-kr.firebaseapp.com/docs/installation.html, "참조")

* webpack
```
 서버에서 처리하는 로직을 JavaScript로 구현하는 부분이 많아지면서 웹 서비스 개발에서
 JavaScript로 작성하는 코드의 양도 늘어났습니다. 
 코드의 양이 많아지면 코드의 유지와 보수가 쉽도록 코드를 모듈로 나누어 관리하는 모듈 시스템이 필요해집니다. 
 그러나 JavaScript는 언어 자체가 지원하는 모듈 시스템이 없습니다. 
 이런 한계를 극복하려 여러 가지 도구를 활용하는데 그 도구 가운데 하나가 webpack입니다.
```
참고: [naver-dev](https://d2.naver.com/helloworld/0239818, "참조")
    
* JSX
```
react에 사용되는 자바스크립트의 문법 확장입니다.
React JSX 는 XML-like Syntax 를 native Javascript로 변환해줍니다.  
따라서 ” ” 로 감싸지 않는 점 주의
이전에는 개발자들이 .jsx 확장자를 사용하였지만 요즘은 .js 를 사용하는 추세
```
문법참고:[velopert-강좌 3편 JSX](https://velopert.com/867)

* 설치 
```
1.npm install -g create-react-app
2.create-react-app hello-react //react app 설치
3.nmp start //app run
```

## 문법
**1.컴포넌트명은 캬멜케이스로**

ex)MyComponent <MyComponent/>

**2.state**

컨포넌트 내부에서 변할수 있는값

class형 컴포넌트의 state
```
import React, {Component} from 'react';

class MyComponent extends Component {
    // 기본적으로 constructor서 state를 셋팅하지만 
    constructor(props){
        super(props);
        this.state={
            number:0, //number state에 초기값세팅
        }
    }
 
    //밖으로 빼도된다.
    state={
        number:0, //number state에 초기값세팅
    }
    // state의 상태값은 setState에서 변경한다.
    this.setState({
         number:this.state.number +1
    })    
    // 비동기로 인해 데이터가 변경되지 않을때 이전의 state의 값으로 새로운 state를 생성할때
    this.setState((prevState,props) => {
        return {
            //이전의 state불러옴
            number:prevState.number+1
         }
     })
    
    //steate에 접근
    const {number} = this.state.number 
    
    .
    .
    .
 ```
 
 
 
 클래스형 컴포넌드에서 input value값 동시에 여러개 변경
 ```
 handleChange = e=>{
        this.setState({
           [e.target.name]:e.target.value
        });
 }
 
 <input
  type={"text"}
  name={"number"}
  value={this.state.number}
   onChange={this.handleChange}
/>
    
 ```
 
 
 함수형 컴포넌트 useState Hooks

 ```

import React,{useState} from 'react';

const MyComponent = () => {
    const [message,setMessage] = useState('')
    //1번째인자값(message) 변수, 두번인자값(setMessage)해당 변수에 set하는 함수명
    //set함수 사용방법
    const onClickFun=()=> setMessage('hello')
     return (
        <div>
            
        </div>
    );
};

export default MyComponent;


```