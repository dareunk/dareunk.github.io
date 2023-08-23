---
title: "Express-validator"
categories:
    - Nodejs
tags:
    - Nodejs & Security
date: 2023-08-23
toc: true
---

### Express-validator

#### Overview 

1. express의 미들웨어로, express를 통해 사용자가 보내온 requests의 validation을 진행.

2. Express-validator를 활용해 특정 value에 대한 유효성 기준을 설정하여, 사용자가 그 기준에 맞게 데이터를 입력했는지 확인해줌.

3. 만약 validation을 만족했다면, error 안이 " "이고, 불만족했다면 error안에 객체가 포함됨(**isAuthenticated()가 true면 validation successes, false면 validation fails)


### Process

#### install express-validator npm

**express-validator을 설치하기 전 반드시 먼저 포함되어야 하는 설정**

1. cookieParser
2. express-session

```
npm install express-validator -S
```


#### Setting and condition

```js
// ./main.js
// const userController = require("./controllers/userController");
const {check, sanitizeBody} = require("express-validator");

app.post("/signup", [
    sanitizeBody("email").normalizeEmail({
        all_lowercase:true
    }).trim(),
    check("email", "email is invaild").isEmail(),
    check("zipCode", "zipCode is invaild").notEmpty().isInt().isLength({
        min:5,
        max:5
    }),
    check("password","password cannot be empty").notEmpty();
],  userController.validate);
```

#### Utilizing

```js
// ./controllers/userController.js
const {validationResult} = require("express-validator");

module.exports = {
   validate: async(req,res,next) => {
       const errors = await validationResult(req);
       //if errors is empty, there is no error
       // if not, there is an error
       if(!(errors.isEmpty())){
           let messages = errors.array().map(e=>e.msg);
           req.skip = true;
           //if you want to use req.flash, you need to use npm of connect-flash
           //req.flash("error", messages.join( "and"));
           return res.redirect("/signup");
       }else{
           //there is no error, so move on!
           next();
       }
   }
};
```
