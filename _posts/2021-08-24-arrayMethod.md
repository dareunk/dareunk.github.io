---
title: "TypedArray Method"
categories:
    - Javascript
tag:
    - javascript
    - 모던 자바스크립트 인문
toc: true
---

### TypedArray Method - 메서드 표/Map 

참고) 형식화 배열이 무엇인지 자세히 알고 싶다면, Array(2) 확인하기 


### 형식화 배열의 메소드

형식화 배열은 TypedArray.prototype의 프로퍼티를 상속받음


|copyWithin|entries|every|fill|
|---------|-------|-----|------|
|filter|find|findIndex|forEach|
|indexOf|join|keys|lastIndexOf|
|map|reduce|reduceRight|reverse|
|set|slice|some|sort|
|subarray|toLocaleString|toString|values|

---

### 1. Map


-데이터를 수집하여 활용하기 위한 객체

-값의 <u>고유한 식별 정보</u>인 '키'와 값의 쌍을 Map객체 안에 저장해서 사용

-Map객체는 외부에서 키를 사용하여 원하는 값을 추가/삭제/검색할 수 있음

-키와 값의 데이터 타입에는 제한이 없음


* #### Map객체와 Object객체의 차이

Map객체와 Object객체는 키와 값이 쌍을 이룬다는 점에서 비슷한 점이 있지만 다음과 같은 차이점이 있음


1. Map객체에는 데이터를 수집하기 위한 다양한 메서드가 마련되어 있음

2. Object객체는 키로 문자열을 사용할 수 있지만, Map객체는 키 타입에 제한이 없음

3. Map객체는 내부적으로 해시 테이블을 사용하기 때문에 데이터 검색속도가 빠름

4. Map객체는 반복가능하며, for/of문으로 순회하면 키와 값으로 구성된 배열을 반환

5. Map객체는 데이터 개수를 **size프로퍼티**로 구할 수 있지만, Objcet객체는 프로퍼티 개수를 수동으로 계산해야함.


결론) Map객체는 Object객체보다 빠르기도 하지만 다양한 데이터 수집 기능까지 제공


---

* #### Map객체의 생성

1. 인수를 지정하지 않으면 데이터가 빈 Map객체가 생성

```javascript
var map = new Map();
console.log(map); //-> Map{}
```


2. [키, 값]을 값으로 가지는 초기 데이터를 생성할 수 있음(이터러블한 객체임)

```javascript
var zip = new Map([["Tom", "131-8634"], ["Huck", "556-0002"]]);
console.log(zip); //-> Map{"Tom" => "131-8634", "Huck" => "556-0002"}
```


3. **size프로퍼티**를 이용하여 데이터(키와 쌍의 개수)의 개수를 구할 수 있음 

```javascript
var zip = new Map([["Tom", "131-8634"], ["Huck", "556-0002"]]);
console.log(zip.size); //->2
```

---

* #### Map객체의 메서드

Map객체는 Map.prototype의 프로퍼티와 메서드를 상속받음

|메서드|설명|
|------|--------------|
|clear()| Map객체 안에 있는 모든 데이터를 삭제함|
|delete(key)| Map객체에서 key가 가르키는 데이터를 삭제함|
|forEach(callback)|Map객체의 모든 데이터를 대상으로 callback함수를 실행. 이때 실행되는 순서는 데이터가 삽입된 순서|
|get(key)|Map객체에서 key가 가르키는 데이터를 반환|
|has(key)|Map객체에서 key가 가르키는 데이터가 있는지를 판정|
|keys()|Map객체에서 <u>데이터 키</u>를 값으로 가지는 이터레이터를 반환|
|values()|Map객체에서 <u>데이터 값</u>을 값으로 가지는 이터레이터를 반환|
|entries()|Map객체가 가진 <u>데이터(키와 쌍의 값)</u>을 저장한 이터레이터를 데이터를 삽입한 순서대로 반환|
|set(key,value)|Map객체에 키가 key고, 값이 value인 데이터를 추가|


1. set(key,value)

```javascript
var zip = new Map();
zip.set("Tom","131-8634");
zip.set("Huck","556-0002");
console.log(zip); //-> Map{"Tom" => "131-8634", "Huck" => "556-0002"}
```


2. get(key)

```javascript
var zip = new Map();
zip.set("Tom","131-8634");
console.log(get(Tom)); //-> 131-8634
```


3. has(key)

```javascript
var zip = new Map();
zip.set("Tom","131-8634");
console.log(zip.has("Tom")); //-> true
console.log(zip.has("Eun")); //-> false
```

4. 데이터 삭제(특정 키가 가르키는 값을 삭제 vs 전체 데이터를 삭제)

```javascript
zip.delete("Huck"); //Huck의 데이터를 삭제함
zip.clear(); // 모든 데이터를 삭제함
```


5. keys()

데이터의 키 값을 가진 이터레이터를 반환

```javascript
var zip =  new Map([["Tom", "131-8634"], ["Huck", "556-0002"]]);
var iter = zip.keys();
for(var v of iter) console.log(v); // Tom, Huck의 순서대로 출력
``` 

6. values()

데이터의 값을 가진 이터레이터를 반환 

```javascript
var zip =  new Map([["Tom", "131-8634"], ["Huck", "556-0002"]]);
var iter = zip.values();
for(var v of iter) console.log(v); // 131-8634, 556-0002의 순서대로 출력
```

7. entries()

데이터(키,값)을 가진 이터레이터를 반환

```javascript
var zip =  new Map([["Tom", "131-8634"], ["Huck", "556-0002"]]);
var iter = zip.entries();
for(var v of iter) console.log(v);// ["Tom", "131-8634"], ["Huck", "556-0002"]의 순서대로 출력
```

8. forEach(callback)

```javascript
var zip =  new Map([["Tom", "131-8634"], ["Huck", "556-0002"]]);
zip.forEach(function(value,key,map){
    console.log(key + " => " + value);
});
//-> Tom => 131-8634
//   Huck => 556-0002
```
value: 현재 처리하는 데이터 값

key: 현재 처리하는 데이터 키

map: 처리중인 Map 객체 


**<u>Map객체의 데이터를 열거</u>**

이터러블한 성질을 이용

```javascript
var zip = new Map([["Tom", "131-8634"], ["Huck", "556-0002"]]);
for(var v of zip) console.log(v);
//["Tom", "131-8634"], ["Huck", "556-0002"]의 순서대로 출력
for(var [key, value] of zip) console.log(key, value)
// Tom 131-8634, Huck 556-0002의 순서대로 출력
```

---

### 2. Set


-중복되지 않는 유일한 데이터를 수집하여 활용하기 위한 객체

-set객체는 데이터 값의 단순집합(set)으로 간주

-set객체는 외부에서 키를 사용하여 데이터 값을 추가/삭제/검색할 수 있음

-값의 데이터 타입에는 제한이 없음


---

* #### set객체의 생성

1. 인수를 넘기지 않으면 데이터가 빈 set객체가 생성

```javascript
var set = new Set();
console.log(); //-> Set{]
```

2. 초기 데이터를 인수로 지정해서 생성이 가능(이터러블한 객체)

```javascript
var zip = new Set([1234,2363]);
```


3. **size**프로퍼티를 통해 데이터 개수를 구할 수 있음

console.log(zip.size); //->2


---

* #### 동일성의 정의

set객체에서는 NaN과 NaN이 같으며, +0과 -0이 같음


---

* #### set객체의 메서드 

set객체는 Set.prototype에서 프로퍼티와 메소드를 상속받음

|메서드|설명|
|-----|---------|
|add(value)|Set객체에 데이터 값인 value를 추가|
|clear()|Set객체 안의 모든 데이터를 삭제|
|delete(value)|Set객체에서 value를 값으로 가진 데이터를 삭제|
|forEach(callback)|Set객체의 모든 데이터를 대상으로 callback함수를 반환|
|has(value)|Set객체에서 value를 값으로 가진 데이터가 있는지를 판정|
|keys()|Set객체에서 <u>데이터 값</u>을 값으로 가지는 이터레이터를 반환|
|values()|Set객체에서 <u>데이터 값</u>을 값으로 가지는 이터레이터를 반환|


1. 데이터 열거

keys()와 values()는 데이터 값을 저장한 이터레이터를 반환

```javascript
var zip = new Set([1234, 5678]);
var iter = zip.keys();
for(var v of iter) console.log(v); // -> 1234, 5678의 순서대로 출력
```


entries()메서드는 Set객체가 가진 데이터인 [값, 값]을 저장한 이터레이터를 반환

```javascript
var zip = new Set([1234, 5678]);
var iter = zip.entries();
for(var v of iter) console.log(v);
// {1234,1234}, {5678,5678}의 순서대로 출력
```


2. forEach(callback)

```javascript
var zip = new Set([1234, 5678]);
zip.forEach(function(value1, value2, set){
    console.log(value1 + " => " + value2);
});
// -> 1234 => 1234
//    5678 => 5678
```

value1: 현재 처리하는 데이터 값

value2: 현재 처리하는 데이터 값

set: 처리중인 set객체


- 첫번째 인수와 두번째 인수는 모두 현재 처리하는 데이터 값

**같은 내용의 인수를 중복해서 받는 이유**

Set.prototype.forEach와 Array.prototype.forEach메서드의 인터페이스를 통일시키기 위함


**<u>Set객체의 데이터를 열거</u>**

이터러블한 성질을 이용

```javascript
var zip = new Set([1234, 5678]);
for(var v of zip) console.log(v); //1234, 5678의 순서대로 출력
```