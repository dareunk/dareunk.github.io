---
title: "Mandelbrot set(실습)"
categories:
    - 실습
tag:
    - javascript
    - 모던 자바스크립트 인문
toc: true
---

### 망델브로 집합_실습 

![image](https://user-images.githubusercontent.com/83913407/127492828-1ac7b7b2-861d-4122-92c1-30f0c655f3dc.png)


* #### 실습을 통해 알게된 점 

1. **망델브로 집합은 아무리 확대해도 복잡한 구조를 가지는 프랙텔 도형임을 실습을 통해 확인할 수 있었음**


2. **망델브로 집합은 발산되지 않는 복소수 c의 집합을 일컫기 때문에, 발산되지 않는 망데브로 집합에 속하는 점들은 검은색으로 표현되지만, 발산되는 애들은 그렇지 않기때문에 모양이 점차 퍼져나가는 구조를 확인할 수 있었음**


![image](https://user-images.githubusercontent.com/83913407/127493156-30df6c56-1d51-4e7b-ab62-48603b08ab92.png)

* #### html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>망델브로집합</title>
    <script src="mandel.js">
    <link rel="stylesheet" href="mandel.css">
</head>
<body>
    <canvas id="mycanvas" width="800" height="640"></canvas>
    <div>중심좌표(<span id="xc"></span><span id="yc"></span>)</div>
    <label>확대배율: <input id="magnification" type="number" value="0.65"></label>
    <label>최대 반복 횟수: <input id="maxit" type="number" value="60"></label>
    <input id="button" type="button" value="draw">
</body>
</html>
```

---


* #### css


**파일이름: mandel.css**

```css

#mycanvas{border:1px solid black;}
input{
    margin:0 10px;
    width:60px;
    text-align: center;
}
div{
    font-size: small;
    margin-bottom: 5px;
}
``` 

---


* #### javascript


**파일이름: mandel.js**

```javascript

window.onload= function(){

        var canvas = document.getElementById('mycanvas');
        var ctx = canvas.getContext('2d');
        var width = canvas.width;
        var height = canvas.height;

        var xc=-0.6, yc=0;
        draw();

        document.getElementById('button').onclick =draw;

        document.getElementById('mycanvas').onclick = function(event){
            var ix = event.offsetX;
            var iy = event.offsetY;
            var mag = parseFloat(document.getElementById('magnification').value);
            xc += (2*ix/width-1)/mag;
            yc += (2*iy-height)/mag/width;
            draw();
        };
        function draw() {
            var mag = parseFloat(document.getElementById('magnification').value);
            var maxit = document.getElementById('maxit').value;
            displayCenter(xc,yc);
            mandelbrot(ctx,xc,yc,mag,maxit);
        }
        };
    function displayCenter(xc,yc){
        document.getElementById('xc').innerHTML = xc.toFixed(3);
        document.getElementById('yc').innerHTML = yc.toFixed(3);
    }

    function mandelbrot(c,xc,yc,mag,maxit){
        var w = c.canvas.width;
        var h = c.canvas.height;
        var xmin = xc - 1/mag;
        var xmax = xc + 1/mag;
        var ymin = yc -(xmax-xmin)*h/w/2;
        var ymax = yc +(xmax-xmin)*h/w/2;
        var dx = (xmax-xmin)/w;
        var dy = (ymax-ymin)/h;
        var color = [];
        color[0] ="black";
        var L=255, dL=255/maxit;
        for(var i=maxit;i>0;i--){
            color[i] = "rgb(255,"+Math.floor(L)+",255)"; L-=dL;
        }
    
    for(var i=0;i<w;i++){
        var x = xmin +i*dx;
    for(var j=0;j<h;j++){
        var y = ymin +j*dy;
        var a=x, b=y;
        var a2=a*a, b2=b*b;
        
        for(var count=maxit; a2+b2<=4 && count; count--){
            b = 2*a*b+y;
            a = a2-b2+x;
            a2 = a*a;
            b2 = b*b;
        }
        c.fillStyle = color[count];
        c.fillRect(i,j,1,1);
    }
    }
    }
```

