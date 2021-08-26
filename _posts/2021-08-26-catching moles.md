---
title: "두더지 잡기 게임"
category:
    - 실습
tag:
    - javascript
    - 모던 자바스크립트 인문
toc: true
---

### 두더지 잡기 게임 - Map 이용

![image](https://user-images.githubusercontent.com/83913407/130968812-3c8e9ff0-527d-469c-9e21-8391c3b26454.png)


* #### Code

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        function makeMoles(nx, ny){
            var molesState = new Map();
            var W = 50, SPACE = 10;
            for(var i=0;i<nx;i++){
                for(var j=0;j<ny;j++){
                    var element = document.createElement("div");
                    element.style.width = W + "px";
                    element.style.height = W + "px";
                    element.style.background = "url(./두더지.png)";
                    element.style.position = "absolute";
                    element.style.opacity = 0.2;
                    element.style.transition = "transform 0.5s ease-in-out, opacity 0.5s ease";
                    document.body.appendChild(element);
                    element.style.left = SPACE + i*(W+SPACE) + "px";
                    element.style.top = 2*SPACE + j*(W+SPACE) + "px";
                    molesState.set(element,{x:i, y:j, opacity:0.2});
                    element.onclick = function clickEventHandler(e){
                        var element = e.currentTarget;
                        var state = molesState.get(element);
                        if(state.opacity >= 0.5){
                            document.body.removeChild(element);
                            molesState.delete(element);
                        }
                    };
                }
            }
            return molesState;
        }
        window.onload = function(){
            var TIME_INTERVAL = 1000, DISPLAY_TIME = 1050;
            var molesState = makeMoles(7,4);
            var timer = setInterval(function appearMole(){
                if(molesState.size == 0){
                    clearInterval(timer);
                    document.body.innerHTML = "game over";
                    return;
                }
                var n = Math.floor(Math.random()*molesState.size);
                var elements = molesState.keys();
                var count = 0;
                for(var element of elements){
                    if(count++ ==n) break;
                }
                var state = molesState.get(element);
                element.style.opacity = 1;
                state.opacity = 1;
                element.style.transform = "translateY(-10px)";
                setTimeout(function hideMole(){
                    element.style.opacity = 0.2;
                    state.opacity = 0.2;
                    element.style.transform ="translateY(0px)";
                },DISPLAY_TIME);
            },TIME_INTERVAL);
        };
    </script>
</head>
<body>
</body>
</html>
```

_두더지.png_

![두더지](https://user-images.githubusercontent.com/83913407/130968329-7cb88517-ff7b-4c2b-ac33-8c6c7ba81960.png)


* #### Code분석

![image](https://user-images.githubusercontent.com/83913407/130974234-32ebc750-32e1-4d66-a185-d3c254470397.png)

* #### 실수 했던 점

molesState.delete(element);를 moleSState.delete(element)로 잘못 작성했을 때 

- 뒤로 갈 수록 두더지가 나오는 속도가 지연되었음


분석) 이미 투명도가 1이되어 화면에서 지워진 아이가 무작위적으로 고르는 n = Math.floor(Math.radom()*molesState.size)에 참여하므로 
중복적으로 참여되어 계산에 포함되었다.

-> 즉, 화면에 보이지는 않지만 뒤에 가서는 없어진 공간에서도 코드 작업을 수행하기 때문에 두더지가 나오는 속도가 느려진 것으로 착각한 것임


**결론) 이미 화면에서 사라진 두더지를 두더지가 나오는 위치계산에 포함시키지 않기 위해 molesState.delete(element)를 이용해 molesSate에서 제거해줌**