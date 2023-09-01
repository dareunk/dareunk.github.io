---
title: "send a mail - nodeMailer" 
categories:
    - Project
tags:
    - Project
date: 2023-09-01
toc: true
---

### nodeMailer

서버에서 특정 사용자의 이메일로 메일을 보내줄 때, 간편하게 구동할 수 있도록 도와주는 npm 


### Process

#### Install npm

```
npm install nodemailer
```

#### Setting 

```js
const nodemailer = require("nodemailer");
var generateRandomString =function(length){
    var text='';
     var possible = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
     for(var i=0;i<length;i++) {
          text += possible.charAt(Math.floor(Math.random() * possible.length));
                                    
     }
     return text;
};
const send = async(email,code) => {
    const transporter = nodemailer.
    createTransport({
        service: "gmail",
        host:"smtp.google.com",
        port: 3000,
        auth: {
            user:process.env.SENDER,
            pass:process.env.PASSWORD
        }
    });
    var text = `[Authentication code] ${code}`;
    //console.log(text);
    const option = {
        from: process.env.SENDER,
        to:email,
        subject: "authentication code",
        text:text
    };
    const info = await transporter.sendMail(option);
    return info;
};

module.exports = {
    sendMessage: async(req,res.next) => {
        let email = req.body.email;
        try{
            var code = generateRandomeString(6);
            const result = await send(email,code);
            req.flash("success", "check your email");
            
        }catch(error){
            console.log(`Failed to send a message: ${error.message}`);
            next();
        }
    }
};
```

#### result 


![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/ee1efe80-f65e-4a94-ad87-7bf62933b85e)


