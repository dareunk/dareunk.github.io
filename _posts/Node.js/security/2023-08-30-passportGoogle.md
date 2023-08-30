---
title: "Passport-Google"
categories:
    - Nodejs
tags:
    - Nodejs & Security
date: 2023-08-30
toc: true
---

### Passport with Google

- Passport는 google, facebook 등 다양한 플랫폼들의 로그인 API를 활용해 간단하게 서버상에서 사용할 수 있도록 해준다.

- passport-google-ouath2 를 통해 google authentication을 진행할 수 있음

| 종류 | 기능 |
|----------|--------------|
|passport-local-sequelize| sequelize와 연결된 DB로 부터 데이터를 받아와 로그인 진행(local DB) |
|passport-local-mongoose| mongo DB로 부터 데이터를 받아와 로그인 진행(local DB)|
|passport-google-ouath2| google로 부터 데이터를 받아와 로그인 진행(external DB)|


### Process

### get a permission from google 

* Click Credentials 

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/8e0cdc6d-255b-4fa8-b64f-6b9f881600e7)


* Click Get a client Id and secret key

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/f81e3cb2-6a6b-47ea-b5c5-6d8f14130157)

* fill a form

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/1d95c5d9-5aff-4dc8-85a4-9c1b8ea75aef)

**_Authorized JavaScript origins: 본인 웹/앱사이트의 origin url_**

**_Authorized redirect URLs: google authentication이후 redirect되는 url(이 부분을 잘 유의해서 프로그래밍해야함)_**

* Check your client ID, client secret, redirect URL

<hr>

### Set client ID, client secret, redirect URL in server

* install npm, which is passport-google-oauth2 

```
npm install passport-google-oauth2 -S
```

*  put client ID, client secret, redirect URL in .env(for security)

```
//.env

GOOGLE_CLIENT_ID = "your client ID"
GOOGLE_CLIENT_SECRET = "your client secret"
GOOGLE_REDIRECT_URL = "the redirect URL you registered"

```

* set those in main page

```js
//main.js
const GoogleStrategy = require("passport-google-oauth2").Strategy;
require("dotenv").config();
const clientID = process.env.GOOGLE_CLIENT_ID,
    clientSecret = process.env.GOOGLE_CLIENT_SECRET,
    clientRedirectUrl = process.env.GOOGLE_REDIRECT_URL;

passport.use(new GoogleStrategy({
    clientId: clientID,
    clientSecret: clientSecret,
    callbackURL: clientRedirectUrl,
    passReqToCallback: true,
}, async function(request, accessToken, refreshToken, profile, done){
    //console.log(profile);
    return done(null, profile);
}));
// need to change a little bit 
//because they can't find google account from local DB while deserializing after sending a user.email from serialization.

passport.serializeUser(function(user,done){
    done(null,user);
});
passport.deserializeUser(async function(user,done){
    done(null,user);
});

```

**google에서 API를 통해 가져오는 데이터 형태를 잘 봐야함** -console.log(profile)을 통해 데이터가 어떻게 레이블링 되었는지를 확인 

<hr>

#### Authenticate

```js
//main.js
app.get("/login/google", passport.authenticate("google", {scope: ["email","profile"]}));
//to callback after authentication from google
//it must be same as redirect url you set in google platform
app.get("/login/google/callback", passport.authenticate("google", {
    failureRedirect : "/login/google",
    successRedirect: "/chatrooms",
    failureFlash:true
}));
```

<hr>

#### tip

1. 정상적으로 google을 통해 authentication을 진행하면 req.isAuthenticated()가 true, req.user에 google로 받아온 해당 account의 정보가 들어가있다. 이를 적절히 활용하여 로그인 서비스를 진행할 수 있다. 


