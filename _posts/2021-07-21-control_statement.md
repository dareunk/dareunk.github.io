---
title: "control statement"
category:
    - 실습
tag:
    - javascript
    - 모던 자바스크립트 인문
toc: true
---

### 제어구문

순차적 실행 흐름을 변화시키는 문장

#### 조건문

* #### if/else문

1. if (조건식) 문장
```
if (!name){
    name = "";
    message = "이름을 입력하세요"
}
```


2. if (조건식) 문장 1 else {문장2}

```
if(x >= 0){
    console.log("positive or zero");
}
else{
    console.log("negative");
}
```

**중첩된 if문**

if가 두 개 이상 등장 

중첩된 if문을 사용할 경우에는 중괄호({})로 묶어서 가독성을 높이는 것이 좋음

```
if(num==1){
    console.log("one");
    if(num==2){
        console.log("two");
    }
    else{
        console.log("other");
    }
}
```

**else if**

```
if(num==1){
    console.log("one");
}else if(num==2){
    console.log("two");
}else{
    console.log("other");
}
```

*### switch문

반복되는 표현식을 사용할 때 switch문을 사용하면 보다 간결하게 가독성을 높이면서 표현이 가능함

```
switch(n){
    case 1:
        console.log("one");
        break;
    case 2:
        console.log("two");
        break;
    case 3:
        console.log("three");
        break;
    case 4:
        console.log("four");
        break;
    default:
        console.log("other");
}
```


case라벨 아래 break문을 적어야 다음 case라벨 바로 앞에서 switch문을 빠져나오게 됨. 

**break문 대신 return문을 사용해도 가능**

