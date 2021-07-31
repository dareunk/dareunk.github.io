---
title: "자바스크립트 프로그램 실행 과정"
category:
    - Javascript
tag:
    - javascript
    - 모던 자바스크립트 인문
toc: true
---  

### 프로그램의 평가와 실행 과정 

### 실행 가능한 코드 _ Executable Code

실행가능한 코드를 만나면 코드를 평가(Evaluation)해서 실행문맥(Execution Context)만듦.


**실행 가능한 코드**

실행문맥을 초기화하는 환경과 과정이 달라 구분


1. 전역 코드

2. 함수 코드

3. eval 코드


### 실행문맥 _ Execution Context

1. 실행 가능한 코드가 실제로 실행되고 관리되는 영역


2. 실행에 필요한 모든 정보를 컴포넌트 여러 개가 나누어 관리하도록 만들어짐


* #### 중요 컴포넌트

**1. 렉시컬 환경(LexicalEnvironment) 컴포넌트**
 
**2. 변수환경(VariableEnvironment) 컴포넌트**

-렉시컬 환경 컴포넌트와 변수 환경 컴포넌트는 같은 유형의 타입을 가지고 있음 - 렉시컬 환경 타입의 컴포넌트

-with문을 사용할 때를 제외하고는 내부 값이 같으므로 렉시컬 환경 컴포넌트 = 변수 환경 컴포넌트라고 생각해도 무방함 

**3. 디스 바인딩(This Binding) 컴포넌트**


_자바스크립트의 객체 표현을 통해 표현한 실행문맥_

```
//실행문맥
ExecutionContext ={
    //렉시컬 환경 컴포넌트
    LexicalEnvironment: {},
    //변수 환경 컴포넌트
    VariableEnvironment: {},
    //디스 바인딩 컴포넌트
    ThisBinding: null,
}
```


3. 실행문맥은 **스택(stack)**이라는 구조로 관리 

_스택_

일종의 자료구조로, 데이터를 아래에서부터 쌓아 올려 마지막으로 추가한 데이터를 먼저 꺼내는 **후입선출(LIFO, Last In First Out)방식


스택을 호출 스택(call stack)이라고도 부르는 이유?

호출되어 함수의 실행문맥이 push되고, return문이 실행되어 제어권이 호출한 코드로 돌아가면 스택에서 pop됨

**push**: 가장 윗부분에 데이터를 쌓는 행위

**pop**: 가장 윗부부에서 데이터를 빼내는 행위

![image](https://user-images.githubusercontent.com/83913407/127735763-b81e2861-15bb-490a-a45d-afa8f75653cf.png)


아래와 같은 경우 모두 실행문맥이 새로 만들어져 위에 push됨!


-함수 안에 있는 코드를 실행하는 도중에 다른 함수를 호출하는 경우

-중첩함수 

-재귀함수 

**중첩함수의 경우에는 외부 함수의 실행 문맥안에 실행문맥에 push(중첩)되는 것이 아니라, 중첩함수의 실행문맥이 새로 만들어져 위로 push가 되는 것**

---

### LexicalEnvironment Component

자바스크립트 엔진이 자바스크립트 코드를 실행하기 위해 자원을 모아둔 곳

함수 또는 유효범위 안에 있는 식별자와 그 결과값이 저장되는 영역

**구성: 환경 레코드 (Environment Record) / 외부 렉시컬 환경 참조(Outer Lexical Environment Reference)컴포넌트**


_자바스크립트의 객체 표현을 통해 표현한 렉시컬 환경 컴포넌트_

```
//렉시컬 환경 컴포넌트
LexicalEnvironment:{
    //환경 레코드
    EnvironmentRecord:{},
    //외부 렉시컬 환경 참조
    OuterLexicalEnvironment Reference:{}
}
```

* #### Environment Record

1. 렉시컬 환경 안의 식별자와 그 식별자가 가르키는 값의 묶음이 실제로 저장되는 곳 

2. ECMAScript3의 변수객체(Variable Object)와 매우 비슷한 역할 수행

3. 자바스크립트 엔진이 유효범위 안의 식별자와 결과값을 바인드해서 환경 레코드에 기록 


    - **환경 레코드에 저장되는 내용**

    1. 함수의 인자

    2. 함수 안에서 선언된 중첩 함수의 참조

    3. 함수 안에서 var로 선언된 지역 변수

    4. arguments()

**구성: 선언적 환경 레코드 / 객체 환경 레코드**

_자바스크립트의 객체 표현을 통해 표현한 환경 레코드_
```
//환경 레코드
EnvironmentRecord: {
    //선언적 환경 레코드 
    DeclarativeEnvrionmentRecord:{},
    //객체 환경 레코드
    ObjectEnvironmentRecord:{}
}
```

**선언적 환경 레코드_Declarative Environment Record**

1. 함수와 변수, catch문의 식별자와 실행 결과가 저장되는 곳

2. 식별자와 그 결과값이 키와 값의 쌍으로 저장되고 관리되는 곳 


**객체 환경 레코드_Object Environment Record**

실행 문맥 외부에 별도로 저장된 객체의 참조에서 데이터를 읽거나 씀 

ex) 전역 개체의 경우에는 그 객체의 키와 값의 쌍을 복사해오는 것이 아니라 객체의 **참조**를 가져와서 객체 환경 레코드의 **bindObject**라는 프로퍼티에 바인드


* #### Outer Lexical Environment Reference

1. 함수를 둘러싸고 있는 코드가 속한 렉시컬 환경 컴포넌트의 참조가 저장

2. 중첩된 함수에서 바깥 코드에 정의된 변수를 읽거나 써야 할 때, 자바스크립트 엔진이 외부 렉시컬 환경 참조를 통해 한 단계씩 렉시컬 환경을 거슬러 올라가 그 변수를 검색 

---

### This Binding Component

그 함수를 호출한 객체의 참조가 저장되는 곳

**이것이 가리키는 값이 곧 해당 실행 문맥의 this가 됨**

---

### 전역 환경과 전역 객체

* #### 전역 환경 _ Global Environment

웹 브라우저에 내장된 쟈바스크립트 인터프리터는 시작하자마자(새로운 웹 페이지를 읽어들인 후) 렉시컬 환경 타입의 전역환경을 생성

전역객체를 생성한 후 전역 환경의 객체 환경 레코드에 전역 객체의 참조를 대입 

_전역 환경_

 
```javascript
//전역 환경
GlobalEnvironment={
    ObjectEnvironmentRecord:{
        bindObject: window
    },
    OuterLexicalEnvironmentReference: null
    /* 전역 환경의 외뷔에는 다른 렉시컬 환경이 없으므로 외부 렉시컬 환경 참조에 null 할당*/
}
```


_전역 실행 문맥_

```javascript
ExecutionContext = {
    LexicalEnvironment: GlobalEnvironment,
    ThisBinding: window,
    /*전역환경에서는 this가 window를 뜻함*/
}
```

---

### 프로그램의 평가와 전역 변수 

**전역환경에서 설정한 함수와 변수는 객체 환경 레코드의 프로퍼티로 추가**


* #### 전역변수

1. 전역변수(최상위레벨에서 선언된 함수와 변수)는 window라는 전역 객체의 프로퍼티 또는 전역객체의 실행문맥에 들어 있는 **객체 환경 레코드**의 프로퍼티 


2. 최상위 레벨에서 선언된 함수와 변수는 프로그램이 평가되는 시점에 이미 객체 환경 레코드에 추가된 상태 -> **끌어올림** 현상의 실체


3. var문과 함수 선언문으로 선언한 전역 변수는 [[Configuable]]속성이 false설정되어 있어 **delete연산자로 삭제가 불가능**

![image](https://user-images.githubusercontent.com/83913407/127734874-64a7fb77-a734-4d8f-bb27-787b33a63680.png)


4. var문을 사용하지 않고 선언한 변수는 **디스 바인딩 컴포넌트**가 가르키는 객체의 프로퍼티로 추가, 즉 전역객체의 프로퍼티로 추가되며 이 경우에는 **delete연산자로 삭제가 가능**

![image](https://user-images.githubusercontent.com/83913407/127734978-f3d3359b-9414-4d7c-9585-4663b03b75f5.png)


---

### 싱글 스레드

프로그램을 실행하는 방식 (스레드: 프로그램의 처리 흐름)

싱글스레드: 프로그램 한 개의 처리 흐름으로, 프로그램을 순차적으로 실행하는 방식


멀티스레드:  프로그램 여러 개의 처리 흐름으로, 작업을 동시에 여러 개 병렬로 실행하는 방식 

![image](https://user-images.githubusercontent.com/83913407/127736246-2f10e976-25f4-4d32-a526-60172f477652.png)


**자바스크립트는 싱글 스레드로 작업을 처리**

1. 코드를 위에서 아래로 차례차례 실행해 나감

2. 스택을 아래에서 위로 push하며 진행, 하나의 작업이 끝나면 pop을 하고 바로 아래에 있는 실행 문맥을 실행

3. 하나의 실행문맥 작업이 끝날 때까지 또 다른 실행 문맥의 작업을 실행하지 않음


**자바스크립트에서 멀티 스레드로 작업을 처리하는 방법**

웹 브라우저의 API인 Web Workers을 사용하면 특정 작업을 백그라운드에 있는 다른 스레드(예를 들면, 멀티 스레드)로 처리가 가능함.

 ---

### 지역 변수

1. 지역변수와 중첩 함수의 참조는 실행 환경의 **선언적 환경 레코드**의 프로퍼티 


2. 지역변수의 값이 선언적 환경 레코드에 설정되며, 값이 없으면 undefined값으로 설정 -> **끌어올림**효과

3. 환경 레코드와 this(디스 바인딩 컴포넌트에 참조가 저장)가 설정되면 함수 안의 코드가 순차적으로 실행 

---

### this값

함수가 호출되어 실행되는 순간에 this값이 결정


This Binding component가 참조하는 객체

![image](https://user-images.githubusercontent.com/83913407/127738056-1723cc26-45a9-4745-a484-1a9ec06f145f.png)

**함수는 객체에 묶여 있지 않음**


-객체에 저장된 함수(매서드)는 객체가 그 함수를 참조하는 것임.

-위와 같이 huck.sayhello = tom.sayhello를 통해 huck라는 객체가 sayhello라는 함수를 tom객체처럼 참조하게 되는 것임.

-**위와 같이 객체 여려 개가 함수 하나를 참조할 수 있고, 이때 호출되는 상황에 의해 this값이 동적으로 변경될 수 있음**

1. 최상위 레벨 코드의 this

최상위 레벌 코드의 this = 전역 객체

```
console.log(this); //->window
```


2. 이벤트 처리기 안에 있는 this


3. 생성자 함수 안에 있는 this

this는 그 생성자로 생성한 객체를 의미


4. 생성자의 prototype 매서드에 있는 this

이때의 this는 그 생성자로 생성한 객체를 의미


5. 직접 호출한 함수 안에 있는 this

