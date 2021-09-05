---
title: "Regular Expression (2)"
categories:
    - Javascript
tag:
    - javascript
    - 모던 자바스크립트 인문
toc: true
---

### 정규 표현식 - 정규 표현식 패턴 주요 구문(총 정리)


---

### 정규 표현식 패턴 주요 구문(총 정리)

|구문|설명|
|----|----------|
|abc| 문자열을 포함|
|[abc]| 문자 클래스: 문자 집합 안의 특정 문자 한개|
|[^abc]| 부정 문자 클래스: 문자 집합의 여집합 문자 한개|
|[a-z]|두 문자 사이의 모든 문자|
|.|줄 바꿈 문자를 제외한 문자 한개|
|\d|모든 숫자. [0-9]와 같음|
|\D|숫자를 제외한 모든 문자 한개. [^0-9]와 같음|
|\w|임의의 영어 단어 문자(알파벳,숫자,언더스코어)한 개|
|\W|영어 단어 문자를 제외한 문자 한개|
|\s|모든 공백 문자 한개|
|\S|공백 문자가 아닌 문자 한개|
|\b|영어 단어 경계 위치|
|\B|영어 단어가 아닌 단어의 경계 위치|
|[\b]|백스페이스|
|x{2,4}|x를 최소 2번, 최대 4번 반복|
|x{2,}|x를 두 번 이상 반복|
|x{2}|x를 두 번 반복|
|x?|x를 한 번 이하 반복|
|x+|x를 한 번 이상 반복|
|x*|x를 0번 이상 반복|
|(x)|x를 그룹화(부분 정규 표현식)**|
|\숫자|숫자 번째의 부분 정규 표현식과 일치한 값|
|(?:x)|x의 캡처링 없는 그룹화|
|x(?=y)|x다음에 y가 따라옴(전방탐색)|
|x(?!y)|x다음에 y가 따라오지 않음(전방 부정 탐색)|
|^|문자열의 시작 위치|
|$|문자열의 마지막 위치|
|x\|y\|z|x,y,z중 하나 선택|


---

### 패턴 매칭 문자열 메서드 

RegExp.prototype의 메서드는 test, exec로 <u>regular expression(1)</u>에 작성

이 곳에서는 String.prototype의 메서드 match, replace, search, split에 대해 정리


* #### 문자열 검색: search메서드

-정규 표현식 객체와 일치한 최초 문자열의 첫 번째 문자 위치를 반환

-일치하는 문자열을 찾지 못했을 때 -1 반환

-search메서드는 원본 문자열을 수정하지 않음

-**search메서드는 전역 검색을 지원하지 않음(따라서 g플래그를 설정하더라도 무시됨)**

```javascript
var s = "1 little,2 little indian";
console.log(s.search(/little/)); //-> 2 
console.log(s.search(/\d/)); //-> 0
console.log(s.search(/\bindian/)); //-> 18
console.log(s.search(/3\s/)); // -1(일치하지 않음)
```


* #### 문자열 치환: replace메서드

-첫 번째 인수로 받은 정규 표현식과 일치한 문자열을 검색한 후, 두 번째 인수로 받은 문자열을 치환한 새로운 문자열을 반환

-replace메서드는 원본 문자열을 수정하지 않음

-**g플래그를 설정하면 전역 검색이 가능함(search와의 차이점)**

```javascript
var s = "1 litte,2 little indian";
console.log(s.replace(/indian/,"boy")); //-> 1 little,2 little boy
console.log(s.replace(/little/,"big")); //-> l big,2 little indian
console.log(s.replace(/little/g,"big")); //-> 1 big,1 big indian 
```

1. **치환 패턴1 : $n**

```javascript
var person = "Tom, tom@example.com, 010-1234-5678";
var result = person.replace(/0(\d{1,4}-\d{1,4}-\d{1,4})/g, "82-$1");
console.log(result); //-> Tom, tom@example.com, +82-10-1234-5678

---

var date = "오늘은 2021년8월27일입니다";
var result = date.replace(/(\d+)년(\d)월(\d+)일/,"$1/$2/$3");
console.log(result) //-> 오늘은 2021/8/27입니다.

---

var name = "Tom Sawyer"
var result = name.replace(/(\w+)\s(\w+)/,"$2 $1");
console.log(result) // -> Sawyer Tom
```


2. **치환 패턴2: $&**

일치한 문자열이 들어옴

```javascript
var address = "123-4567 서울특별시";
var result = address.replace(/\d{3}-\d{4}/,"(우)$&"));
console.log(result); //-> (우)123-4567 서울특별시
```


3. **문자열 치환을 처리하는 함수**

**match**: 일치한 부분 문자열($&와 같은 값)

**group1**: 첫 번째 부분 정규 표현식과 일치한 부분 문자열($1과 같은 값)

**group2**: 두 번째 부분 정규 표현식과 일치한 부분 문자열($2와 같은 값)

...

**offset**: 일치한 부분 문자열의 첫 번째 문자 위치

**inputStr**: 원본 문자열 전체


---

* #### 문자열 추출: match메서드

첫 번째 인수로 받은 정규 표현식과 일치하는 문자열을 순서대로 저장해서 **배열**로 반환


"1 little,2 little indian".match(/\d+/g); //-> ["1", "2"]


-g플래그를 설정하지 않으면 가장 처음 일치한 문자열만 배열로 반환

-RegExp.prototype의 exec메서드와 같은 역할을 한다고 보면 됨

```javascript
var url = /\b(\w+):\/{2}([\w.]+)\/(\S*)\b/;
var text = "Tom의 홈페이지 url은 http://www.example.com/~tom 입니다.";
console.log(text.match(url));
//->["http://www.example.com/~tom","http","www.example.com","~tom"]
```


---

* #### 문자열 나누기: split메서드

-첫 번째 인수로 문자열을 분할한 다음에 배열에 담아서 반환

-첫 번째 인수를 생략하면 원본 문자열 전체를 배열에 담아서 반환

-split메서드는 원본 문자열을 수정하지 안흥ㅁ


console.log("172.20.51.65".split(".")); // -> ["172", "20", "51", "65"]


```javascript
var names = " Tom Sawyer ; Huckleberry Finn ; Becky Thatcher ";
var list = names.replace(/(^\s*|\s*$)/g,"").split(/\s*;\s*/);
console.log(list);
//-> ["Tom Sawyer", "Huckleberry Finn", "Becky Thatcher"]
```


**<u>반환할 문자열의 개수 제한하기</u>**

split메서드의 두 번째 인수는 선택 사항으로, 반환할 문자열의 개수를 제한함 


console.log("1,2,3,4,5".split(",",3)); //-> ["1", "2", "3"]


---

### RegExp 객체


* #### RegExp 객체의 프로퍼티

|프로퍼티| 설명|
|------|-----------|
|source|정규 표현식 패턴 문자열을 저장|
|global|g플래그가 사용되고 있는지를 뜻하는 논리값.읽기전용|
|ingnoreCase|i플래그가 사용되고 있는지를 뜻하는 논리값.읽기전용|
|multiline|m플래그가 사용되고 있는지를 뜻하는 논리값.읽기전용|


* #### RegExp 객체의 메서드

Regular Expression(1)에서 RegExp의 메서드로 test와 exec에 대해 배웠음.


**<u>+lastIndex프로퍼티</u>**

g플래그를 설정한 정규 표현식으로 exec나 test메서드를 실행하면 일치한 문자열 바로 다음 번 문자의 위치가 정규 표현식 객체의 lastIndex프로퍼티에 저장됨

(**일치하지 않을 때는 0이 저장됨**)

```javascript
var tel = /(\d{2,5})-(\d{1,4})-(\d{4})/g";
var text = "Tom: 010-1234-5678\nHuck: 020-1234-5678\nBecky: 030-1234-5678";
console.log(tel.leastIndex); //-> 0
console.log(tel.exec(text)); // -> ["010-1234-5678", "010","1234","5678"]
console.log(tel.lastIndex); //-> 18
console.log(tel.exec(text)); // -> ["020-1234-5678", "020", "1234", "5678"]
```

lastIndex프로퍼티는 수정이 가능함. 

-> 값을 설정해서 원하는 문자의 위치에서 검색을 시작할 수 있음

_예시_

tel.lastIndex = 0;
var result = tel.exec(text);
console.log(result[0]); //-> 010-1234-5678


**<u>exec를 반복패턴으로 설정하는 방법</u>**

```javascript
var tel = /(\d{2,5})-(\d{1,4})-(\d{4})/g;
var text = "Tom: 010-1234-5678\nHuck: 020-1234-5678\nBecky: 030-1234-5678";
while((result = tel.exec(text)) != null){
    console.log(result[0], result[1], result[2], result[3]);
}
```

_결과_

![image](https://user-images.githubusercontent.com/83913407/131160862-19523f43-da73-4ba2-b153-cf7681218d4b.png)



---

### ECMAScript 6부터 추가된 기능

ECMAScript 6부터 추가된 정규 표현식의 기능

* #### u 플래그

정규 표현식은 문자열을 UTF-16으로 표시하는 16비트 문자열로 처리

따라서, 써로게이트 페어로 표현하는 문자를 포함하는 문자열은 적절한 처리가 불가능


* #### y플래그

-y플래그의 설정 여부는 정규 표현식 객체의 sticky프로퍼티(쓰기 가능)에 저장

-**시작 위치 고정 검색**

-y플래그를 설정하면 정규 표혀닉 객체의 lastIndex프로퍼티의 인덱스가 가리키는 그 위치부터 검색이 시작

```javascript
var text = "1 little, 2 little, 3 little indian";
ver reg = /\d+ little/y;
console.log(reg.sticky); //-> true
reg.lastIndex = 10;
console.log(reg.exec(text)); //-> ["2 little"]
console.log(reg.lastIndex)); //-> 18
reg.lastIndex = 9;
console.log(reg.exec(text)); //-> null
console.log(reg.lastIndex)); //->0(찾지 못했을 때는 0)
```


ECMASCript 6을 지원하지 않는 환경에서 u플래그와 y플래그를 설정하면 문법 오류(Syntax Error)가 발생함

**자바스크립트 실행 환경이 각 플래그를 지원하는지 확인할 수 있는방법**

-> 다음과 같은 함수

```javascript
function hasRegExpY(){
    try{
        new RegExp(".","y");
        return true;
    }catch(e){
        return false;
    }
}
```

* #### flags프로퍼티

정규 표현식 객체에 설정한 플래그의 문자열이 무엇인지 확인하기 위해서는 ...

1. ECMAScript 5까지

-각 플래그별 프로퍼티를 확인

-source프로퍼티 값의 끝 부분을 분석해야함


2. ECMAScript 6부터

-flags프로퍼티로 플래그 문자열을 쉽게 확인할 수 있음

```javascript
var reg = /abc/gi;
console.log(reg.flags); //-> gi
```


* #### RegExp의 인수로 정규 표현식을 넘겼을 떄

RegExp생성자의 첫 번째 인수로 정규 표현식 객체를 넘기면 넘긴 객체와 같은 패턴을 지닌 정규 표현식 객체가 생성됨 


-**ECMAScript 5까지**

1. 이 복사본을 **얇은 복사**로 생성했음(인수로 받은 정규 표현식 객체의 참조를 반환)

2. 플래그의 값은 쓰기가 금지되어 있어 복사본이 원본과 다른 플래그를 가질 수 없음


-**ECMAScript 6부터**

1. **깊은 복사**로 바뀌었음

2. 원본 정규 표현식 객체와는 다른 플래그를 사용하는 것이 가능해짐

```javascript
var reg = /abc/gm;
var copy = new RegExp(reg,"gi");
```

3. 정규 표현식 객체에 복사본을 만들면 각각의 객체가 별도의 lastIndex프로퍼티를 갖게 됨(다른 문자열에 같은 패턴의 정규 표현식을 적용할 때 lastIndex프로퍼티를 다시 설정하지 않아도 됨)

```javascript
var reg = /우(\d{3})-(\d{3})/;
var copy = new RegExp(reg,"gi");
```




