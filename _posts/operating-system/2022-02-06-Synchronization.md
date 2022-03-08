---
title: "운영체제 -Process Synchronization"
categories:
    - Operating System
tags:
    - Operating System
date: 2022-02-06
toc: true
---

## Process Synchronization

---


### Race Condition

**Race condition** = 공유하는 data에 대해서 Process들이 동시에 접근하여 '변경'의 행위를 하는 경우 일관성이 깨지는 문제(**inconsistency**). 

**<u>용어정리</u>**

| 용어 | 뜻 |
|-----|----------|
| Critical Section | 공유하는 data를 변경하려고 하는 코드 영역 |
| Entry Section | Critical Section의 앞 뒤에 존재하며, Permission의 기능을 하는 코드 영역 |


-OS에서도 Race Condition문제가 발생할 수 있다.


-Multithreaded 환경에서도 data(전역변수를 저장하고 있는 공간)에 접근하는 경우 Race Condition문제가 발생할 수 있다.

![image](https://user-images.githubusercontent.com/83913407/152631898-83877c60-bf63-40b0-902b-dfd53e2736a4.png)



-**<u>Race Condition의 해결방법</u>**

- Ciritical Section을 한 개의 process만이 실행할 수 있도록 한다. 그 외의 나머지 process들은 이 process의 shared data로의 접근이 끝나지 않는 동안은 Ciritical Section부분을 실행할 수 없다. 

- Critical Section의 앞 뒤에 Permission의 기능을 하는 Entry Section을 
삽입하여 만약 해당 공유 data를 접근한 process가 없다면 들어갈 수 있게 해주고, 없다면 들어갈 수 없게 해주는 관리자를 배치해 둔다.



### Race Condition's Solution

**<u>Race Condition's Solution의 만족 조건</u>**

1. Mutual Exclusion(상호배타): 하나의 Process만이 Critical Section에 접근할 수 있다.

2. Progress: Critical Section으로의 진입허가가 무기한으로 연기돼서는 안된다.

3. Bounded Waiting: 일정횟수만 기다리면 Critical Section에 들어갈 수 있도록 해야한다.


* #### Peterson's Solution

두 개의 process만 있을 때 적용이 가능 

_명령어 종류_

1. **turn**: Critical Section으로 들어가는 process의 순서를 나타냄(차례를 나타냄)

2. **flag**: Critical Section으로 들어갈 수 있는 준비가 되어있다는 것을 나타냄


![image](https://user-images.githubusercontent.com/83913407/152675619-fd71d2f0-c7e5-4b18-9520-ea37c7423fa2.png)

![image](https://user-images.githubusercontent.com/83913407/152675562-958485a6-2484-4a4c-b056-cc94741bd0e5.png)

**특징**: Mutual Exclusion, Progress, Bouded Waitiing만족


* #### Synchronization Hardware

_**명령어 종류**_

다음 명령어들은 <u>atomically</u>하게 동작(이해를 돕기위해 함수처럼 표현했지만 실제로 함수가 아님. 다음 동작들은 하나의 동작으로 한꺼번에 수행이 되는 것임 -> 따라서, 동작 중간에 context switching되는 것이 불가능함)

1. **TestAndSet**

![image](https://user-images.githubusercontent.com/83913407/154483426-9bdb835d-451e-471e-90dc-f9c03068edee.png)


2. **Swap**

![image](https://user-images.githubusercontent.com/83913407/154483544-1d374756-a417-4514-a54b-496dca26f0d0.png)


_**TestAndSet의 동작원리**_

![image](https://user-images.githubusercontent.com/83913407/154657088-eca3bdc5-dd8c-4619-bf9c-0ec4fb037f99.png)


1. 처음 critical section에 접근하는 process는 lock이 false이므로 ciritical에 접근이 가능하다. 이때 lock은 TestAndSet의 동작원리에 따라 true로 변환된다.


2. 그 기간동안 다른 process가 critical section에 접근하면 lock이 true이므로, 무한루프가 실행 되어 critical section으로의 접근이 불가능하다.


3. critical section으로의 접근이 끝난 process는 lock을 false로 바꾸어 잠금을 풀어준다.


4. lock이 false가 되므로, 다른 process의 critical section으로의 접근이 가능해진다.


_**Swap의 동작원리**_

![image](https://user-images.githubusercontent.com/83913407/154657168-27a61223-f7d6-4398-a247-ddffb4b5bbb0.png)


1. lock의 default는 false이다. 처음 critical section에 접근하는 process는 Swap명령어에 의해 자신의 변수인 key와 lock의 값을 서로 바꾸게 된다.


2. 그 기간동안 다른 process가 critical section에 접근하면 lock이 true이므로, Swap명령어가 실행되더라도 key와 lock모두 true여서 결국 무한루프를 빠져나가지 못해 critical section으로의 접근이 불가능하다.


3. critical section으로의 접근이 끝난 process는 lock을 false로 바꾸어 잠금을 풀어준다.


4. lock이 false이므로, 다를ㄴ process의 critical section으로의 접근이 가능해진다.


**특징**: TestAndSet과 Swap은 Bounded Waiting을 만족하지 못함


**arbitarary order**: process들이 같은 ciritical section으로 <u>동시 접근</u>하는 경우, 먼저 TestAndSet이나 Swap 실행하는 process가 ciritical section으로 진입한다.  


* #### Mutex Locks

critical section문제를 해결하는 software tool


_**명령어 종류**_

1. acquire() : lock을 거는 역할, avaliable이 true인 경우 무한루프를 빠져나오고, 또한 다른 process의 critical section으로의 접근차단을 위해 available을 false로 전환

![image](https://user-images.githubusercontent.com/83913407/154659017-f226b9ef-e0c9-4fa4-9a93-97c76956edf5.png)


2. release(): lock을 반환하는 역할, critical section으로의 접근이 끝난 process는 다른 proces의 접근을 가능하게 하기 위해 release을 이용하여 available을 true로 전환

![image](https://user-images.githubusercontent.com/83913407/154659078-f4fd79a3-47d1-4d9f-a840-8fd61136f547.png)


_**Spinlock의 문제 해결**_

**spinlock** = busy waiting: 무한루프를 도는 경우

이렇게 무한루프를 도는 경우에는 CPU를 낭비하게 되므로 wait로 보내 이러한 문제를 해결 


* #### Semaphore

critical section의 문제를 해결하면서, Synchronizaion Tool로도 작동


_**명령어 종류**_

1. wait(s) = P() operation
 
![image](https://user-images.githubusercontent.com/83913407/154660816-50aea925-5475-453c-9c32-870502eee751.png)


2. signal(s) = V() operation

![image](https://user-images.githubusercontent.com/83913407/154660887-455c4d19-fbc8-44c5-a7ac-30f8447d138c.png)


_**Semaphore의 종류**_

1. Counting Semaphore

일정한 정수 범위 내에서 동작

2. Binary Semaphore


_**Critical Section문제 해결**_


-자원이 1개인 경우에 해당

-Mutex lock과 비슷

![image](https://user-images.githubusercontent.com/83913407/154665490-cc1cfa58-f789-4018-9bb2-a0bdcd0d0a2d.png)

1. 먼저 wait를 실행하는 process가 critical section으로 진입


2. 이때 wait의 작동에 의해 S가 1작아져 0이되므로, critical section에 진입하려는 다른 process는 무한루프가 돌아 진입이 불가능해진다.


3. critical section의 진입이 끝난 process는 signal을 통해 s을 1 증가시켜 반납한다.


4. 다른 process는 S가 1이므로 critical section으로의 진입이 가능해진다.


_**Semaphore로서 작용**_

![image](https://user-images.githubusercontent.com/83913407/154666215-095ddaa0-a1f1-4041-816e-491fa9f2b24b.png)


-S2는 synch가 0이므로 무한루프에 걸려 실행이 안된다. 이후 time slice가 끝나 S1에 CPU가 할당된다. S1이 실행이 끝나면서 signal을 통해 synch가 1이 되므로 다음 S2로 CPU가 할당되면 실행이 가능해진다. 


-단점: busy waiting(spinlock)으로 인한 CPU낭비


_**Semaphore without Busy waiting**_


Waiting Queue가 각각 존재

![image](https://user-images.githubusercontent.com/83913407/155082215-9f8569ec-5b94-41b3-891a-7f9309166e51.png)

당장 critical section에 접근을 못하는 process는 while문에 의해 Busy waiting되는 것이 아니라, Run상테에서 Wait상태로 바뀌게 된다.


1. block(): waiting Queue로 들어감

![image](https://user-images.githubusercontent.com/83913407/154670822-27eb2d63-5925-4924-bd48-6c6b705ba5b3.png)

2. wakeup(): waiting Queue에서 벗어남

![image](https://user-images.githubusercontent.com/83913407/154670976-8be333cf-1086-4af7-9d35-fa526132d0ee.png)


* #### Monitors

-Semaphore가 가진 문제를 아예 차단 시키는 High Level Language construct

_semaphore가 가진 문제?_

내부적으로는 semaphore을 사용하고 있으나, 겉으로는 그것이 드러나지 않기 때문에 signal연산과 wait연산을 잘 못 구성할 일이 없음


-하나의 Process만의 monitors을 실행 가능 => mutual exclusion 만족


```
monitor monitor-name
{
    //shared variable declarations
    
    //공유하는 변수 값을 바꾸는 함수 = monitor함수를 선언
    procedure P1(...){...}
    procedure P2(...){...}
    procedure Pn(...){...}

    initalization code(...){...}
}
```

_**monitor의 동작원리**_


1. critical section에 접근하려는 process들이 줄을 섬

2. 줄을 선 순서를 바탕으로 차례대로 처리

3. 이때 하나의 process만이 monitor함수에 접근이 가능(mutual exclution만족)

_monitor함수_: shared data에 접근해 그 값을 바꾸는 함수로, 하나의 process만이 접근할 수 있다. 


_**단점**_

synchronization을 완벽하게 실행하지 못함 => condition을 이용


* #### condition 

condition 변수가 존재 -> signal과 wait를 사용 시 변수 명을 함께 붙여 호출

_**명령어 종류**_

1. x.wait(): wait를 호출한 process는 일시정지된다.

2. x.signal(): 하나의 process만을 일시정지 상태에서 깨워주는 역할을 한다.


_**condition 동작원리**_

1. critical section에 접근하려는 process들이 줄을 섬

2. 줄을 선 순서를 바탕으로 차례대로 처리

3. condition함수를 호출한 process들을 변수명을 바탕으로 줄을 서게 하여 순차적으로 처리(monitor가 가진 synchornization문제를 해결할 수 있음)


### Dining-Philosopher 

* #### Problem


조건: 철학자와 젓가락은 총 5개이고, 젓가락은 각각 1개만 존재한다.

![image](https://user-images.githubusercontent.com/83913407/155081141-1729fb9e-8a4e-4447-84ad-a24824481db7.png)


```
do{
    wait(chopstick[i]);
    wait(chopstick[(i+1)%5]);
    //eat

    signal(chopstick[i]);
    signal(chopstick[(i+1)%5]);
    //think
}while(true);
```

-semaphore가 가진 문제를 그대로 반영하고 있다.(wait와 signal의 순서가 어긋나면 안된다.) -> monitor을 활용

-starvation문제가 발생할 수 있다.

-**deadlock문제가 발생할 수 있다.**

_deadlock이 발생하는 경우의 예시_

![image](https://user-images.githubusercontent.com/83913407/155082550-cdd5bdc3-fd0c-43de-9a52-5d1019e3362e.png)


철학자2가 젓가락2번을 들고나서 context switching을 일어나 철학자3이 젓가락3번을 들게 된다. 그 후 context switching이 일어나면 철학자2는 젓가락 3을 들려고 한다. 그러나 젓가락3은 이미 철학자3이 사용하고 있으므로 철학자2는 이를 기다려야한다. 이러한 상황이 모든 철학자에게 이루어진다면 deadlock이 발생하게 된다.


* #### Solution

_**deadlock문제를 예방시킬 수 있는 방법**_


1. 철학자를 4명만 앉힌다.이러한 경우 철학자 중 한 명은 반드시 양쪽의 젓가락을 확보할 수 있게 되므로 deadlock문제가 발생하지 않는다.


2. 철학자가 젓가락 양쪽을 확보할 수 있을 때만 젓가락을 들 수 있도록 한다. 


3. 짝수는 오른쪽의 젓가락을 먼저 집도록 하고, 홀수는 왼쪽의 젓가락을 먼저 집도록 한다.


_**monitor을 활용한 해결방법**_

위의 semaphore가 가진 문제를 해결할 수 있다.

```
monitor DP //모니터이름
{
    enum{ THIKING< HUNGRY, EATING }state[5]/;
    condition self[5]; //각 젓가락마다 condition 변수가 지정
    void pickup(int i){
        state[i] = HUNGRY;
        test[i];
        if(state[i] != EATING) self[i].wait();
    }
    void putdown(int i){
        state[i] = THINKING;
        test((i+4)%5);
        test(i+1)%5);
    }
    void test(int i){
        if((state[(i+4)%5])!=EATING) &&
        (state[i]==HUNGRY) && 
        (state[(i+1)%5]!= EATING)
        {
            state[i] = EATING;
            self[i].signal();
        }
    }
    initalization_code(){
        for(int i=0;i<5;i++) state[i] = THINKING;
    }
}
```