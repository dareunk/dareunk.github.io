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

---

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

* ### switch문

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

* 고혈압 측정(switch문 이용)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>고혈압 여부 측정하기</title>
    <script>
        window.onload = function(){
  

            
            document.getElementById('button').onclick = function(){

            var high = parseFloat(document.getElementById('highpressure').value);
            var low = parseFloat(document.getElementById('lowpressure').value);
            var result = document.getElementById('result');
            /*이 변수선언을 이벤트 처리기 앞에 넣어주면 오류가 생김. 클릭을 누를 때 input에 있던 값들을 가지고 와야지 정확한 결과값이 도출되는 데 
            클릭 전에 변수선을 해버리면, 예상이 불가능한 input값이 조건문 high와 low변수에 할당될 수 있으므로 주의해야함 */
            
             if(high<120 && low<80){
                result.innerHTML = "정상혈압입니다.";
            }else if(high<139 || low<89){
                result.innerHTML = "고혈압 전단계입니다.";
            }else{
                result.innerHTML = "고혈압입니다. ";
            }
            };
            
        };
    </script>
    <body>
        <p>수축기 혈압(최고혈압): <input type="number" id="highpressure"></p>
        <p>이완기 혈압(최저혈압): <input type="number" id="lowpressure"></p>
        <p id="result">이곳에 판정 결과가 표시됩니다.</p>
        <input type="button" id="button" value="확인하기"> 
    </body>
    </html>
```

---

### 반복문

* #### while문 

    조건만 맞으면(true) 일정한 처리를 계속 반복해서 실행

    
    **while(조건식){문장}**


    * while문 안에서 break를 실행하면 while문에서 빠져나올 수 있음

    * while문 안에서 continue를 실행하면 while문의 시작 부분으로 되돌아감

1.factorial계산
```
function fact(n){
    var k =1, i=1;
    while(i<n){
        k *= (++i)
    }
    return k;
}
fact(5); //-> 120
```


2.  **순차(선형)검색 linear search**

```
function linearSearch(x,a){
    var i = 0;
    var n = a.length;
    while( i < n && x> a[i]) i++;
    if(x == a[i]) return i;
    return null; //만약 같은 값을 발견하지 못한 경우에는 null이 반환되게 됨//
}

var a = [2,4,6,12,34,56,123];
console.log(linearSearch(34, a)); // ->4
```


* **이차검색 binary search**
```
function binarySearch(x, a){
var n = a.length;
var left=0;, right=n-1;
while(left<right){
    var middle = Math.floor((left+right)/2);
    if(x <= a[middle]){
        right = middle;
    }
    else{
        left = middle+1;
    }
}
if(x==a[right]) return right;
return null;//만약 같은 값을 발견하지 못하면 null이 반환됨//
}
var a = [2,4,6,12,34,56,123];
console.log(linearSearch(34, a)); // ->4
```

**선형검색과 이차검색의 차이점**

이차검색이 선형검색에 비해 검색속도가 훨씬 빠름

(특히 데이터 개수가 많아질수록 이 둘의 계산속도의 차이가 확연하게 나타남)

ex. n(데이터 개수)가 10만일 때 최악의 경우 선형검색은 10만번을 검색해야 하고 이진검색은 log~2~100000이므로 약 17번만 검색하면 됨

* #### do/while문 

반복실행 여부의 순서 차이에 의해 while문과 do/while문의 차이가 발생

1.while: 반복실행 여부를 시작부분에서 판단
2.do/while: 반복실행 여부를 마지막 부분에서 판단(즉 1번은 조건에 벗어난 문장이 실행되게 됨)

**do {문장} while(조건식);**

**do/while문 끝에는 반드시 세미콜론이 불어야 함**

factorial계산

```
function fact(n){
    var k=1, i=n;
    do{
        k *= i--;
        /*--i로 작성하면 값이 빼진 후 평가되기 때문에 5부터 시작이 아닌 4부터 시작하게 되고, 결국 1일 때 0의 값이 곱해지기 때문에 반환되는 결과값이 0이 되게 됨*/
    }while(i>0);
    return k;
}
fact(5); //->120
```

Newton-Raphson으로 제곱근 구하기

(기본적으로 자바스크립트는 Math함수를 통해 제곱근 구하는 매서드를 제공하지만 Newton-Raphson을 이용하여 직접 제곱근 구하는 함수를 작성할 수 있음)

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        var EPSILON = 1.0e-10;
        var d, xnew, xold;
        var a = parseFloat(prompt("정수를 입력하세요"));
        xold = (a>1)? a:1.0;
        do{
            xnew = xold -(xold*xold-a)/(2.0*xold);
            d= (xold-xnew)/xold;
            xold=xnew;
        }while(d>EPSILON);
        document.write("sqrt("+ a + ") = " + xnew);
    </script>
</head>
<body>
    
</body>
</html>
```


* #### for문

**for(초기화 식; 조건식; 반복식) 문장**

for문은 다음 반복식에서 일반적으로 사용하는 세 가지 작업을 모두 소괄호에 넣어서 작성

1. 반복 조건의 초기화 작업

2. 반복문의 조건식

3. 반복 작업이 하나 끝났을 때 조건을 갱신하는 작업

<장점>

1. 반복처리 구조를 이해하기 쉬워짐(가독성을 높임)

2. 위의 작업을 빠트리는 실수를 사전에 방지할 수 있음

배열요소의 합계 구하기

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        function sumArray(a){
            var sum=0;
            for(var i=0;i<a.length;i++){
                sum += a[i];
            }
            return sum;
        }
        var a =[3,5,1,2,6,7];
        document.write(sumArray(a));
    </script>
</head>
<body>
</body>
</html>
```


* #### for/in문

객체 안의 프로퍼티를 순회하는 반복문

**for(변수 in 객체 표현식)문장**

```
var obj = {a:1, b:2, c:3};
for (var p in obj){
    console.log("p =" +p); 
}
 결과) p=a 
       p=b
       p=c

```

```
var obj ={a:1, b:2, c:3};
for(var p in obj){
    console.log("obj." + p + "=" + a[p]);
}
결과) obj.a =1
      obj.b =2
      obj.c =3
```

---

### 점프문 

프로그램의 다른 위치로 이동하는 제어 구문

* #### 라벨문

**라벨 이름 : 문장**

-라벨 이름에는 모든 식별자를 이용할 수 있음

-switch문장과 반복문의 경우 라벨문을 이용할 수 있음

```
loop: while(true){
    
    if(confirm("종료하시겠습니까")) break loop;
}
```


* #### break문

**break 라벨 이름;**

- switch문과 반복문 안에서만 사용할 수 있음

```
var a =[2,4,6,8,10] , b=[1,3,5,6,9,11];
loop: for(var i=0;i<a.length; i++){
    for(var j=0;j<b.length;j++){
        if(a[i]==b[j]) break loop;
    }
}
console.log("a[" + i + "] = b[" + j + "]");
//-> a[2] = b[3]
```


* #### continue문

**continue 라벨이름;**

-라벨 지정 여부와 관계없이 반복문안에서만 사용 가능함
-continue문이 실행되면 반복문 실행이 멈추고 다시 새로 반복문이 시작됨

0이상(양수)인 값만 더하기 

```
var a = [2,5,-1,7,-3,6,9];
for(var i=0,sum=0;i<a.length;i++){
    if(a[i]<0) continue;
    sum += a[i];
    //0보다 작은 값(음수)를 만나면 다시 반복문으로 돌아감

}
console.log(sum); // -> 29
```
