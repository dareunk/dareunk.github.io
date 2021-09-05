---
title: "Enter_event"
categories:
    - 실습
tag:
    - javascript
    - 모던 자바스크립트 인문
toc: true
---

### enter 

* #### html


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>enter</title>
    <script href="enter.js"></script>
</head>
<body>
    <input type="text" placeholder="enter your id">
    <button type="submit">submit</button>
    <div id="msgbox"></div>
<body>
</html>
```

---

* #### Javascript - 누적되는 경우


![image](https://user-images.githubusercontent.com/83913407/126176041-92b942e9-2a8b-46f7-b480-e32b0732ea4d.png)



```javascript
window.onload = function(){

const $input = document.querySelector('input[type=text]');
const $button = document.querySelector('button');
const $msgbox = documnet.querySelector('#msgbox');


$button.onclick = function{

    const p = document.createElement('p');
    const content = document.createTextNode($input.value);
    p.appendChild(content);
    $msgbox.appendChild(p);
    $input.value='';
}
// 버튼을 클릭했을 때 값을 msgbox안에 띄움(누적되는 방식)

Sinput.onkeyup = e =>{
    if(e.key !== 'Enter') return;
    Smsgbox.textContent = e.target.value;
    e.target.value='';
}
//input에 값을 입력하고 Enter버튼을 입력했을 때 input안의 값이 msgbox안에 나타남

};
```

---

* #### Javascript - 일회성 


![image](https://user-images.githubusercontent.com/83913407/126175446-92e5075f-cabd-463d-bd25-867cbead542c.png)



```javascript
window.onload = function(){

const $input = document.querySelector('input[type=text]');
const $button = document.querySelector('button');
const $msgbox = documnet.querySelector('#msgbox');


$button.onclick = function{

    $msgbox.textContent = $input.value
}
// 버튼을 클릭했을 때 값을 msgbox안에 띄움

Sinput.onkeyup = e =>{
    if(e.key !== 'Enter') return;
    Smsgbox.textContent = e.target.value;
    e.target.value='';
}
//input에 값을 입력하고 Enter버튼을 입력했을 때 input안의 값이 msgbox안에 나타남

};

```
