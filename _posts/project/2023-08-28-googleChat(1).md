---
title: "googleChat(1) - Introduction" 
categories:
    - Project
tags:
    - Project
date: 2023-08-28
toc: true
---

### googleChat


#### What is googleChat?

github address for code: <https://github.com/dareunk/googleChat>

googleChat is a webstie, which people can talk to in real time. A user can make their own room, and others can enter the chatroom. People can chat and their talks save in the DB. It includes every chatrooms and chat's contents unless the user who made the chatroom deletes it.

- ~~I'll add the login function using google API. People can access the website with google account without signing up directly to the website.(completed)~~


#### npm used in each function

| function | npm |
|----------|------------------------|
| login | passport, passport-local-sequelize, passport-google-oauth2,express-validator, bcrypt |
| database | mysql2, sequelize |
| chat | socket.io |


### How to create a frame for each page 


* Main 

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/ec780125-7067-4885-b205-0fc89d0287ce)

* Login - local or google

A user can logIn with local account or google account

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/e3c3ef62-57a6-44f8-b0ad-d4a297eba259)

**_login through google_**

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/2b50324c-75e3-4570-84f8-d88e82d5fea8)

**_login with local account signed up_**

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/222e4b36-e6df-4f2e-9ba3-d78c94229758)


* Sign up

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/93e5446c-5ff5-43f2-ad9a-6be5117ffcbe)


* Chatrooms

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/4c86d396-9015-4f3a-9db2-3a10ad057b37)


* Chat 

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/3e19bd3c-930f-4bb0-8157-70b7e7bc0f66)


### How to set a database

There are three tables in the database.

_There is no the table for google account_ 

1. chat 
2. chatroom
3. user 

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/f9a1e274-83d6-46fa-b919-ec0e27f79672)

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/e35dd749-a10c-4625-8cf9-dcf1c4076b20)


#### details in each tables 

* chat 

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/6ca28198-5a79-4a1b-8817-b1229616373c)

* chatroom

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/3a72ee5d-d84e-490a-8e95-5d7acfa31027)

* user

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/eae6473b-d23b-418c-a0de-93bb70e209cb)

