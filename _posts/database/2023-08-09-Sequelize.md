---
title: "Sequelize"
categories:
    - Database
tags:
    - Sequelize
date: 2023-08-10
toc: true
---

### Sequelize

_참고: <https://sequelize.org/>_

- 웹 서버에서 SQL문을 사용하지 않아도 데이터베이스와 양방향 소통이 가능하도록 해주는 ORM의 일종

- config/config.js: sequelize를 사용하기 위한 환경 설정(db와의 연결)

- models/index.js: 테이블을 정의하고 **관계 설정**


* #### How to install sequelize

```
npm install sequelize -S
sudo npm install -g sequelize-cli
sequelize init
```
_참고: <https://www.npmjs.com/package/sequelize>_


* #### config/config.js

1.config/config.json을 config/config.js로 수정

```
mv config/config.json config/config.js
```

2.dotenv활용해 다음과 같이 파일 수정

```
require("dotenv").config();
module.exports = {
    development: {
        username: process.env.DB_USER,
        password: process.env.DB_PW,
        database: process.env.DB_NAME,
        host: process.env.DB_HOST,
        dialect:"mysql"
    },
    production: {
        username: process.env.DB_USER,
        password: process.env.DB_PW,
        database: process.env.DB_NAME,
        host: process.env.DB_HOST,
        dialect: "mysql",
        logging:false
    }
}
```


* #### models/index.js

- sequelize인스턴스 생성을 위해 let sequelize을 다음과 같이 설정함

```
let sequelize = new Sequelize(config.database, config.username, config.password, config);
```

- 만약 config/config.json을 config/config.js로 바꾼 경우: 기존 index.js에는 default로 config.json으로 경로가 설정되어 있기 때문에 이를 수정

```
const config = require(__dirname + '/../config/config.js')[env];

```

* #### synchronize 

모델과 데이터베이스 사이의 동기화 필요

```js
// ./main.js

sequelize.sync()  // 테이블이 없으면 테이블 생성
//sequelize.sync({force:true}) // 테이블이 있으면  drop하고 재생성
//sequelize.sync({alter:true}) // 모델과 데이터베이스의 테이블이 다른 부분이 있다면, '모델'에 맞춰 변경
```


### Sequelize model 생성

* #### define이용

```js
// ./models/user.js

module.exports = (sequelize, Sequelize) => {
    const User = sequelize.define("user", {
        name: Sequelize.STRING,
        email: {
            type: Sequelize.STRING,
            primaryKey: true;
        }},{
            timestamps: false,
            tableName: "user" // or modelName: "user" 
        });
    
    return User;
}
```

* #### init이용

```js
// ./models/user.js

const {Model} = require("sequelize");
module.exports = (sequelize, Sequelize) => {
    const User extends Model{
        ... // check the below how to design class in sequelize
    };
    User.init({
        username: Sequelize.STRING,
        email: {
            type: Sequelize.STRING,
            primaryKey: true;
        }
    },{
        timestamps; false,
        sequelize,
        tableName: "user";
    });
    return User;
};
```

### Sequelize class 

- 모델에서 class를 아래의 코드와 같이 만들 수 있다 (./models/user.js 참고).

- 이때, class 내에 모델 및 테이블을 조절하는 함수를 설정하여 이용할 수 있다. (./controllers/usersController.js참고)

_권장: 유지보수 및 코드 내의 flexible을 위해 함수를 설정하는 것을 권장_


```js
// ./models/user.js

const {Model} = require("sequelize");
class User extends Model{
    static async findByPkAndUpdate(id, params){
                        let user = await User.findByPk(id);
                        if(user){
                                user = await User.update(params, {
                                        where: {id: id}
                                });
                        }
                        return user;
                }
                static async findByPkAndRemove(id){
                        let user = await User.findByPk(id);
                        if(user){
                                user = await User.destroy({
                                        where:{id:id}
                                });
                        };
                        return user;
                }
};

// ./controllers/usersController.js

const db = require("../models/index"),
    User= db.user;
const getUser = body => {
    return {
        username: body.name;
        email: body.email;
    }
};
module.exports = {
update: async(req,res,next) {
    const userId = req.params.id;
    const userParams = getUser(req.body);
    try{
        let user = await User.findByPkAndUpdate(userId,userParams);
        //if you want, you can set direction after updating. ex) res.render("thanks");
        next();
    }catch(err){
        console.log(`Error to update user ${userId} : ${err.message}`);
        next(err); //if you want to load the error's page you made when error comes up, you must put next(err); 
    }
}
};
```


### Sequelize instruction


* #### insert : create문 사용

```js
// ./controllers/usersController.js

module.exports = {
new: async(req,res) => { 
const user = await User.create({
    username: "dareunk",
    email: "dareunk@1234"
});
console.log(user.username); // "dareunk"
console.log(user.email); // "dareunk@1234" 
}
}
```

- post로 사용자에게 입력값을 받아 데이터베이스에 저장하는 경우에는 다음과 같이 입력값을 정돈해주는 함수를 설정하는 것이 좋음

```js
const getUser = body => {
    return {
        username: body.name;
        email: body.email;
    }
};
module.exports = {
    new: asynce(req,res)=>{
        const userInfo = getUser(req.body);
        const user = await User.create(userInfo);
        console.log(user.username); // name the user input   
        console.log(user.email); // email the user input
    }
};
```


* #### findOne / findAll 

- findOne: 조건(where문)에 해당하는 user을 찾음

```js
// ./controllers/usersController.js

show: async(req,res,next) => {
    const user = await User.findOne({
        where: {
            username: "dareunk";    
         }
    });
    console.log(user.email); // "dareunk@1234"
    next(); // must enter next(); in this case
};
showView: asynce(req,res) = { res.render("show");}
```


- findAll: 조건(where문)에 해당하는 모든 users을 찾음

```
show: async(req,res,next) => {
    const users = await User.findAll(); // return all users in the User table
    console.log(users.every(user=> user instanceof User)); // true
    console.log("All users:", JSON.stringfy(users, null, 2));
}

```


* #### Update

```js

findAndUpdate: async(req,res,next) => {
    const user = await User.findByPk(id);
    if(user){
        user = await User.update({
            where: {id:id}
        });
        return user;
    }else{
        console.log("cannot find user");
    }
}
```


* #### Destroy

```js 

findAndRemove: async(req,res,next) => {
    const user = await User.findByPk(id);
    if(user){
        user = await User.destroy({
            where: {id:id}
        });
        return user;
    }else{
        console.log("cannot find user");
    }
}
```


### Sequelize Relationship

Sequelize 관계 설정은 **./models/index.js에서 진행**


* #### Relationship 종류

**Relationship은 Association들의 조합을 통해 표현**

1. One to One : hasOne + belongsTo

2. One to Many : hasMany + belongsTo

3. Many to Many : belongsToMany + belongsToMany


* #### Association 종류


hasOne, belongsTo, hasMany에서 옵션은 선택사항(foreignKey는 자동생성)

**belongsToMany에서 옵션 중 through은 필수사항** : A와 B를 참조하는 모델을 생성

1. hasOne : 1:1관계
            A.hasOne(B): 1:1관계로, B가 A의 기본키를 참조. _B에 외래키 존재_


2. belongsTo : 1:1관계
            B.belongsTo(A) : 1:1관계로, A가 B의 기본키를 참조. _A에 외래키 존재_


3. hasMany: 1:N관계
            A.hasMany(B): 1:N관계로, B가 A의 기본키를 참조. _B에 외래키 존재_


4. belongsToMany: N:M관계
            A.belongsToMany(B, through:'C'}) 

**Sequelize가 C 모델을 생성하고, A와 B를 참조하는 외래키를 자동으로 추가**


* #### 관계 설정 예시

- N:M 관계


```js
// ./models/index.js

db.Foo = require("./foo.js")(sequelize,Sequelize);
db.Boo = require("./boo.js")(sequelize,Sequelize);

db.Foo.belongsToMany(db.Boo, {through:"C"});
db.Boo.belongsToMany(db.Foo, {through: "C"});
```


- 1:1관계


```js
// ./models/index.js

db.Foo = require("./foo.js")(sequelize,Sequelize);
db.Boo = require("./boo.js")(sequelize,Sequelize);

db.Foo.hasOne(db.Boo);
db.Boo.belongsTo(db.Foo);
```


### How to call other model

**방법 1**

```js
// ./models/chat.js

module.exports = (sequelize, Sequelize) => {
    // call User table
    const User = require("./user.js)(sequelize)(Sequelize);
}
```

**방법 2**

```js
const db = require("./index"),
    User = db.user; // need to register db. user in models/index.js 

```

### How to register models

모델을 ./models 경로안에 생성했으면, 이를 ./models/index.js에 등록해줘야 한다.

```
db.user = require("./user.js")(sequelize,Sequelize);
```


### Summary of process

**You need to make DB and tables first before starting this process**

1. Install sequelize npm

2. Revise ./config/config.js file. You should put right information about the DB you made.(like username, password, database name, etc ...)
**Need to register your id, password in your DB setting before doing it**

3. Revise ./models/index.js

4. Make models in ./models

5. Register models you made in ./models/index.js

6. Set relationships between models in ./models/index.js

7. Use data from DB with sequelize instruction(create, findAll, findOne, update, destroy)






