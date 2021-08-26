---
title: "Regular Expresstion"
category:
    - Javascript
tag:
    - javascript
    - 모던 자바스크립트 인문
toc: true
---

### 정규표현식

**<u>정규표현식(regular expresstion)</u>**

-문자열의 패턴을 표현하기 위한 도구


-숫자 세 개로 시작한 뒤 하이픈, 마지막에는 숫자 세 개로 끝나는 형태

**/\d{3} - \d{3}/**(위의 패턴을 정규 표현식으로 표현했을 때)

![image](https://user-images.githubusercontent.com/83913407/130932190-5012f6bf-d35f-42aa-9200-4b0e072e675b.png)


-정규 표현식을 사용하면 문자열 안에서 특정 패턴을 가지는 문자열을 검색/추출/치환하는 등의 고급 문자열 처리 코드를 직접 작성하지 않고도 구현할 수 있음

-자바스크립트의 정규 표현식은 Perl의 정규 표현식을 받아들인 것으로, ECMAScript 3부터 표준이 되었음


* #### 정규 표현식의 생성 

자바스크립트의 정규 표현식은 **RegExp객체**로 표현

-> **RegExp생성자 또는 정규 표현식 리터럴**로 생성할 수 있음

```javascript
var reg = new RegExp("abc"); // RegExp생성자로 생성
var reg = /abc/; // 정규 표현식 리터럴로 생성
```

정규 표현식 패턴을 작성할 때는 숫자(0~9)와 알파벳(a~z,A~Z)둥의 일반 문자와 메타문자를 사용할 수 있음

**메타문자(meta character)**: 정규 표현식에서 사용하는 특수문자 

_종류_

**^ $ \ . * + ? () [] {} |**

위의 메타문자를 문자로서 사용할 때는 '\+'처럼 메타 문자 앞에 \를 붙여 줘야 함


---

* #### 패턴 매칭

**일치(Match)**: 어떤 문자열이 정규 표현식이 표현하는 문자열의 패턴을 가지고 있을 때 그 문자열을 보고 정규표현식과 일치한다고 함


_예시_

```javascript
var reg = /cat/;
"cat"   // 일치함
"dog"   // 일치하지 않음
"cats and dogs" //일치함
"Cat"   //일치하지 않음
"ca"    //일치하지 않음
"concatenate" //일치함
```

**패턴매칭**: 문자열이 정규 표현식과 일치하는지 확인하는 작업


---

* #### RegExp객체의 메서드 

정규 표현식을 사용하여 문자열을 처리하는 방법

1. RegExp.prototype

test, exec 메서드 사용


**test메서드**

정규 표현식과 문자열이 일치하는지 뜻하는 논리값을 반환

```javascript
var reg = /cat/;
console.log(reg.test("cats and dogs")); //-> true
console.log(reg.test("Cat")); //-> false
```


**exec메서드**

정규 표현식과 일치하는 문자열을 검색하여 일치한 문자열을 배열로 반환(일치하는 문자열을 찾지 못했을 때는 null을 반환)

```javascript
var reg = /Script/;
var result = reg.exec("JavaScript");
console.log(result[0]); //-> "Script"
```

![image](https://user-images.githubusercontent.com/83913407/130928723-223a271d-7b7a-4d65-b752-095ad70e4ecc.png)

반환된 배열에는 **index와 input프로퍼티**가 존재

index: 가장 처음 일치한 위치

input: 문자열과 일치한 정규 표현식 


2. String.prototype 

match, replace, search, split 메서드 사용


---

### 정규 표현식 패턴 작성


* #### 리터럴 문자

일반문자도 리터럴 문자에 해당됨


자바스크립트의 정규 표현식 패턴에서 리터럴문자를 사용할 수 있음


메타문자와는 달리 \n처럼 \문자로 시작하기 때문에 **리터럴문자는 문자로 표기가 불가능**( 메타문자는 문자앞에 \를 붙여줌으로써 문자로서 사용이 가능함)


_정규 표현식 리터럴 문자_

|리터럴 문자|설명|
|-----------|-------------|
|유니코드 문자| 문자 그 자체를 듯함(단, ^,$,\,.,*,+,?,(,),[,],{,},|는 제외)|
|\0|Null문자(\u0000)|
|\n|개행 문자(LF=\u000A)|
|\t|탭문자(\u0009)|
|\v|수직 탭 문자(\u000B)|
|\f|다음 페이지 문자(FF=\u000C)|
|\r|캐리지 리턴 문자(CR=\u000D)|
|\xhh|16진수 hh로 지정된 ASCII문자|
|\uhhhh|16진수 hhhh로 지정된 유니코드 문자|
|\cx|제어문자|


* #### 문자 클래스

[...]

특정 문자 집합 안의 모든 단일 문자와 일치

```javascript
console.log(/[0123456789]/.test("10 little indians")); //->true
[a-z]   // 전체 소문자 중 문자 한 개
[abcx-z]    //[a] [b] [c] [x] [y] [z]중 문자 한 개
[a-zA-Z0-9] //모든 알파벳과 숫자 중 문자 한 개
```

**만약 한글로 패턴을 만들고 싶으면 유니코드 문자를 활용해야함**

```javascript
[\u3131-\u314E] // 한글 자음 한 개
[\u314F-\u3163] // 한글 모음 한 개
[\uAC00-\uD7A3] // 한글 음계 한 개
[\uF900-\uFAFF] // CJK 호환 한자 한 개 
```

**씨로케이프 페어가 포함된 문자열과 정규표현식의 패턴매칭도 가능**


-문자를 이용하여 **범위**라는 뜻을 가질때는 앞 뒤 모두 리터럴 문자여야 함

-> 그렇지 않은 경우 -는 그냥 문자 자체를 의미함


* #### 부정 문자 클랙스 : [^...]

대괄호 안에 들어있지 '않은' 단일 문자와 일치

_예시_

```javascript
console.log(/[^0-9]/.test("135")); //-> false
console.log(/[^0-5]/.test("999")); //-> true
```

대괄호 안의 패턴이 ^로 시작하지 않으면 ^를 문자로 인식함


* #### 문자 클래스의 단축 표기


1. **임의의 문자 한개 : .**

마침표(.)는 줄 바꿈 문자를 제외한 임의의 문자 한 개와 일치

```javascript
/c.t/ // cat cool등 일치함(condition과 action과는 일치하지 않음)
/c..t/  //cool은 일치(cat이나 condition은 일치하지 않음)
```


2. **숫자와 숫자 외의 문자: /d, /D**

\d는 숫자로 해석할 수 있는 문자 한개와 일치

\d는 [0123456789]의 단축 표기

```javascript
var dateTime = /\d\d\d\d-\d\d-\d\d \d\d:\d\d/;
console.log(dateTime.test("it's 2021-08-26 18:22")); //-> true
console.log(dateTime.test("2021-Aug-26 18:22")); // -> false
```

\D 는 숫자외의 문자와 일치 

\D 는 [^0123456789]의 단축 표기

```javascript
var dateTime = /\d\d\d\d-\D\D\D-\d\d \d\d:\d\d/;
console.log(dateTime.test("2021-Aug-26 18:22")); //-> true
```


3. **단어 문자와 단어 문자 외의 문자: \w, \W**

\w는 모든 영어 단어 문자(알파벳, 숫자, 언더스코어)

\w는 [a-zA-Z0-9]의 단축표기

```javascript
console.log(/\w/.test("a")); //-> true
console.log(/\w/.test("!&#%-")); //-> false
```

\W는 영어 단어 문자가 아닌 문자와 일치


4. **공백 문자와 공백 문자 외의 문자: /s, /S**

/s는 모든 공백 문자(공백문자, 탭문자, 개행 문자 등)

```javascript
console.log(/\s\w\w/.exec("Javascript RegExp")); //-> [" Re"]
console.log(/\s\w\w/.exec("JavascriptRegExp")); //-> null
```

\S는 공백 문자가 아닌 문자와 일치


5. 문자 클래스 안에서의 이스케이프

**메타 문자를 문자 클래스 안에서 사용하면 메타문자로서의 특별한 의미는 없어지고 그냥 문자 자체를 뜻하게 됨**

_예외사항 \b_

\b는 문자열 리터럴에서 백스페이스를 의미

but, 정규 표현식 패턴에서는 단어의 경계 위치를 의미

**문자 클래스 안에서의 \b는 백스페이스를 의미**


---

* #### 반복 패턴

이 반복패턴을 이용하면 정규 표현식의 요소를 여러 번 반복하도록 지정하여 더욱 간결하게 표기할 수 있음


1. **최소 m번, 최대 n번 반복: {m,n}**

/[a-z]{6,12}/ //_알파벳 소문자가 여섯 자 이상이며 12자 이하인 문자열과 일치_


2. **바로 앞의 요소를 최소 n번 반복: {n,}**

/[a-z]{6,}/ //_알파벳 소문자가 여섯 자 이상인 문자열과 일치_


3. **바로 앞의 요소를 n번 반복: {n}**

/[a-z]{4}\d{3}/ //_알파벳 소문자가 네 자, 그 뒤에 숫자가 3자 등장하는 문자열과 일치_


4. **최대 한 번 반복: ?**

/[a-z]{4}\d?/ //_알파벳 소문자가 네 자,그 뒤에 숫자가 없거나 한 개인 문자열과 일치_


5. **최소 한 번 반복: +**

+는 바로 앞의 요소를 최소 한 번 이상 반복 


/\s+Tom\s+/ //_[Tom]의 바로 앞뒤에 공백 문자가 최소 한 자 이상 등장하는 문자열과 일치_


6. **최소 0번 반복: \***

/[a-z]{4}\d*/ //_알파벳 소문자가 네 자 등장하고, 그 뒤에 최소 0 개 이상의 숫자가 등장하는 문자열과 일치_


7. **욕심없는 반복: 반복문자?**

```javascript
//욕심 많은 반복
console.log(/Java.*/.exec("I love Javascript")); //->["Javascript"]
//욕심 없는 반복
console.log(/Java.*?/.exec("I love Javascript")); //->["Java"]
```


* #### 그룹화와 참조

그룹화 : (...)

```javascript
var bark = /bow+(woo+f)+/;
console.log(bark.test("bowwoofwoofwooofwoooof")); //-> true
```

소괄호()로 묶은 부분은 **부분 정규 표현식**

**<u>부분 정규 표현식</u>**

일치한 값은 별도로 저장되어 나중에 그 부분을 다시 참조할 수도 있음

-> **캡쳐링**: 이렇게 일치한 값을 저장하는 동작

```javascript
var header = /<h[1-6]>.*<\h[1-6]>/;
console.log(header.test("<h1>Javascript</h1>)"); //->true
console.log(header.test("<h2>Javascript</h2>)"); //->true
var header = /<(h[1-6])>.*<\/1>/;
console.log(header.test("<h1>Javascript</h1>)"); //->true
console.log(header.test("<h2>Javascript</h2>)"); //->false
```
/1이 부분정규표현식과 일치하는 값을 참조(위에서는 h1해당)


_exec는 부분 정규 표현식과 일치한 문자열도 들어감_
```javascript
var header = /<(h[1-6])>.*<\/1>/;
console.log(header.test("<h1>Javascript</h1>"));
//->["<h1>Javascript</h1>", "<h1>"]
```


**<u>캡쳐링 없는 그룹화</u>**

**(?:...)**를 표기하면 캡쳐링 없는 그룹화를 만들 수 있음

```javascript
var postalCode = /(?:\d{3})-(?:\d{3})/;
console.log(postalCode.test("345-231")); //-> true
console.log(postalCode.exec("345-231")); //-> ["345-231"]
```

그룹화를 하면 캡쳐링이 되어 exec로 표현했을 때 뒤에 부분 정규 표현식이 표현됨

![image](https://user-images.githubusercontent.com/83913407/130945807-840e086a-1501-48f7-abb6-6fd935bf8db1.png)

**위의 경우에는 ?:를 이용하여 캡쳐링 없는 그룹화를 만듦으로써 exec를 사용하여도 뒤에 부분 정규 표현식이 표현되지 않음**


---

* #### 위치를 기준으로 매칭

**앵커**: 문자열의 위치를 패턴으로 지정하는 문자


1. **문자열의 시작 위치: ^**

![image](https://user-images.githubusercontent.com/83913407/130946437-31124916-48f7-4370-b9a9-82ed012c7fb3.png)


2. **문자열의 마지막 위치: $**

![image](https://user-images.githubusercontent.com/83913407/130946720-ea05251f-62f6-4e7a-8ba7-73ff77d282af.png)


3. **영어 단어의 경계: \b**

![image](https://user-images.githubusercontent.com/83913407/130967757-6ecacc90-8581-4100-8a28-8c6d2ec5d8cf.png)

