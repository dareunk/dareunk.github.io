---
title: "Ball Animation_공 튀기기"
categories:
    - 실습
tag:
    - javascript
    - 모던 자바스크립트 인문
toc: true
---

### 검은색 바탕에 공 튀기는 애니메이션 작성(javascript)

<u>벽에 공이 맞으면 공의 위치를 변화시키는 효과를 추가</u>


* #### code

![image](https://user-images.githubusercontent.com/83913407/130316760-db8f569f-b520-4eb2-b83c-9b05ba9cb250.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    window.onload = function(){
        var NBALL = 200; //공의 개수
        var R = 5; //공의 반지름
        var TIME_INTERVAL =33; //애니메이션 시간 간격(ms단위)
        var BACK_ALPHA = 0.3 //배경의 a값

        var canvas = document.getElementById('mycanvas');
        var ctx = canvas.getContext('2d');
        var wall = {left:0, right: canvas.width, top:0, bottom: canvas.height};

        var balls = [];
        for(var i=0; i<NBALL; i++){
            balls[i] = new Ball(wall.right/2,wall.bottom/2,R);
            balls[i].setVelocityAsRandom(2,7).setColorAsRandom(50,255);
        }

        setInterval(drawFrame, TIME_INTERVAL);

        function drawFrame(){
        
            ctx.fillStyle = 'rgba(0,0,0,'+BACK_ALPHA+')';
            ctx.fillRect(0,0,canvas.width,canvas.height);
            for(i=0; i<balls.length; i++){
                balls[i].move().collisionWall(wall).draw(ctx);
            }
        }
        };

        function Ball(x,y,r,vx,vy,color){
            this.x = x;
            this.y = y;
            this.r = r;
            this.vx = vx;
            this.vy = vy;
            this.color = color;
        }

        Ball.prototype = {

            setVelocityAsRandom: function(vmin,vmax){
                var v = vmin + Math.random()*(vmax-vmin);
                var t = 2*Math.PI*Math.random();
                this.vx = v*Math.cos(t);
                this.vy = v*Math.sin(t);
                return this;
            },
            setColorAsRandom: function(lmin,lmax){
                var R = Math.floor(lmin+Math.random()*(lmax-lmin));
                var G = Math.floor(lmin+Math.random()*(lmax-lmin));
                var B = Math.floor(lmin+Math.random()*(lmax-lmin));
                this.color = 'rgb('+R+','+G+','+B+')';
                return this;
            },

            draw: function(ctx){
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(this.x,this.y,this.r,0,2*Math.PI,true);
                ctx.fill();
                return this;
            },

            move: function(){
                this.x += this.vx;
                this.y += this.vy;
                return this;
            },
            collisionWall: function(wall){
                if(this.x - this.r <wall.left){
                    this.x = wall.left + this.r;
                    if(this.vx < 0) this.vx *= -1;
                }
                if(this.x + this. r> wall.right){
                    this.x = wall.right - this.r;
                    if(this.vx>0) this.vx *= -1;
                }
                if(this.y-this.r <wall.top){
                    this.y = wall.top + this.r;
                    if(this.vy<0) this.vy *= -1;
                }
                if(this.y + this.r>wall.bottom){
                    this.y = wall.bottom - this.r;
                    if(this.vy>0) this.vy *= -1;
                }
                return this;
            }
        };
    </script>
</head>
<body>
    <canvas id="mycanvas" width=640 height=480></canvas>
</body>
</html>
```

---

* #### analysis

![image](https://user-images.githubusercontent.com/83913407/130317727-5766e8b2-21cb-45b6-8fdc-1e0f2fd9fec7.png)

![image](https://user-images.githubusercontent.com/83913407/130317825-f1f932c8-175e-49c5-9068-94f4eef616ca.png)
