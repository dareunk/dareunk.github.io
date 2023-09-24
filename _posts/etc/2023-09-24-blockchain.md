---
title: "Blockchain"
categories:
    - Information
tags:
    - Information
date: 2023-09-24
toc: true
---

Summarization about Blockchain class at Sungshin University

### Blockchain

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/5d81fc4b-eb22-41dc-98ce-b7b26f7076bf)

- 정보가 담겨있는 블록들의 체인

- 블록체인에 담겨있는 데이터는 변경이 암호학적으로 불가능함

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/06936345-9e02-464f-80e5-e75968053180)

#### Merkle tree root

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/0ddfdaa3-daa1-4e3c-a5bf-1f2921e0bdfd)

- 각 하위 정보가 hash값으로 구성

- 그 하위 정보의 상위 정보도 hash 값으로 구성

- 즉 하위 정보가 변경되면 상위 정보의 hash값도 연달아 변경됨

#### Blockchain의 hash

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/d4aa6711-b76b-4158-b4c8-23b37fc37199)

- 모든 블록들은 서로 연결되어 있기 때문에 하나의 블록에서 hash 값이 변경되면 연결이 끊어짐

-따라서, 특정 블록에서 악위적으로 데이터 변경이 이루어졌는지를 확인할 수 있음

#### 구조

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/1f69b8a0-3f9a-40ed-b239-edd3b23f955b)

1. Block body는 merkle tree root 구조로 이루어짐

2. merkle tree root로 안 이루어져있다면, 출력값이 256비트인 n개의 transaction에 대한 저장공간이 필요(n x 256)

3. but, merkle tree root구조로 이루어졌다면, 결국 출력값은 256비트 하나이므로 256비트에 대한 저장공간만 필요


#### 특징

- 탈 중앙화

- 투명성

- 불변성

    - hash

    - p2p network 구조

    - 전자서명


### Hash function

- 임의의 길이의 입력값을 고정된 길이의 출력값으로 압축

- 쉬운 연산: 암호학적 해시함수는 비트 연산자로 구성


#### Cryptographic hash function

- 암호학적 해시함수

- MD5, SHA-1, SHA-2, BLAKE, Keccak

- MD5, SHA-1같은 경우 지금은 많이 사용하지 않음, 보안적으로 크게 중요하지 않은 곳에서만 사용(ex. Git에서의 commit값)


#### Hash function의 성질

Hash function은 다음과 같은 성질을 만족해야 함

1. Preimage resistance(One-wayness): 일방향성

2. Second preimage resistance(weak collision resistance): 약한충돌저항성

3. Collision resistance(strong collision resistance): 강한충돌저항성


#### Premiage resistance(one-wayness)

- 일방향성

- z=h(x)를 만족하는 x를 찾는 것이 연산학적으로 불가능

- 따라서 hash function을 password를 db에 저장할 때 사용하여 암호화


#### Second preimage resistance(weak collision resistance)

- 약한충돌저항성

- 해시함수의 정의역은 치역보다 훨씬 큼 (해시함수의 정의역 >> 치역)

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/8ad07960-bcfb-43e9-b6ac-a620ba417a45)


- **비둘기집 원리**에 의해 같은 함수값 가지는 원소가 존재 = h(x) = h(x')를 만족하는 x!=x'인 x가 존재 

_비둘기집 원리: 4개의 비둘기 집이 있고, 이미 그 집들에 새들이 하나씩 위치해있음. 이때 다른 새 한마리가 집에 들어오려고 하면 집 중 하나는 두 명의 새들이 같이 존재하게 됨._

- 이것들이 연산학적으로 불가능하도록 해야함 

- 치역 크기가 N인 경우 N+1을 조사하면 충돌(같은 값의 해시)를 확인할 수 있음 

- 따라서 N+1의 조사가 어렵도록 함 

- **현재 컴퓨팅 환경에서 80bit(이진수 80자리)면 적당함**
    - 이런 경우 2의 80승의 경우의 수가 나옴 
    
    -  이 말은 (2의 80승 + 1)을 조사하면 충돌쌍을 찾을 수 있다는 말 -> 현실적으로 어려움 


#### Collision resistance(strong collision resistance) 

- 강한 충돌 저항성

- 공격자가 출력값, 입력값 모두 변경이 가능해짐

- Birthday attack 

    - 한 반에서 같은 생일을 가진 두 명의 학생이 존재하기 위해서는 몇 명의 학생으로 구성되어야 하느냐?

    - 비둘기집 원리에 의하면 1년 = 365, 즉 365+1의 학생이 존재하면 그 중에서는 반드시 생일이 같은 학생이 존재함

    - but, 실제로는 20명이면 50%의 확률, 40명이면 90프로의 확률 

- 출력값이 80bit로 구성되어 있다면, 실제로는 2의 40승만 조사하면면 충돌쌍이 발생함

- 따라서, 출력값이 160bit 이상으로 권장됨

    - 이 경우에는 80bit를 조사하면 충돌쌍 발생 -> 위에서 언급했든 이 정도면
    현실적으로 조사하여 충돌쌍 발생시키기 어려움

-  ex) SHA256: 출력값 256bit


#### Hash function의 특징

- 한 바트가 달라져도 전체 해시값이 달라짐

- 안정성으로 160bit 이상의 출력값 권장 

- 암호학적 난수발생기(DRBG)에 사용


### P2P Network 

- 모든 사람들이 참여 가능

- 모든 사람들이 블록체인을 감시

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/031d776a-b571-4444-9003-0f53405e544a)


### 전자서명

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/2ca979de-983d-41d1-a293-bf66af17a33c)

- 공개키 기반의 암호로 구성

- 비대칭키 암호를 역으로 이용하여 구성됨

    - 비대칭키처럼 공개키를 이용해 암호화하면 너무 느림(대칭키에 비해 1000배 가량 느림)

- 개인키를 이용해 평문에 서명을 하고, 이를 공개키로 하여 검증

- 해당 문서가 특정인에게 왔다는 것을 쉽게 검증할 수 있음

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/2ad54af3-9e2f-4b9d-8789-0f4b063f29cd)

#### 대칭키

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/a5491286-f752-462c-834c-a464b6745a23)

- from과 to가 같은 K라는 키를 이용하여 암호화 및 복화를 진행

- from이 평문(x)를 키(k)를 이용하여 암호문(y)으로 생성하여 to에게 전달

- to는 키(k)를 통해 암호문(y)를 해독하여 평문(x)로 해석


#### 비대칭키 

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/4a69ac59-eab0-4b24-bb56-5b6983aa25a0)

- 비대칭키는 공개키와 개인키로 구성

- 한 사용자는 한 쌍의 공개키와 개인키로 구성

    - A의 공개키는 A의 개인키로 연결

    - A의 개인키는 A의 공개키로 연결
    
    - 즉 '한'명의 사용자는 대응되는 특정 공개키, 개인키를 가짐

- 공개키를 이용해 암호화 진행

- 개인키를 통해 복호화

- 공개키를 알아도, 개인키를 유추하기는 어려움

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/988035af-35e1-4807-aedd-1662c2090170)
