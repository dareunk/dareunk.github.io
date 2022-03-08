---
title: "운영체제 - CPU Scheduling"
categories:
    - Operating System
tags:
    - Operating System
date: 2022-02-05
toc: true
---

## CPU Scheduling 

---

**CPU의 이용률을 극대화하기 위해서는 CPU Scheduling이 필요**


### time sharing

1.time sharing방식이 필요한 이유

CPU가 배정된 process가 I/O를 요청해 wait상태에 들어가면, 그동안 CPU는 불필요허게 시간을 낭비하는 것이다. 따라서 이 시간동안 CPU를 필요로 하는 다른 process에게 이 CPU를 할당하면 효율성을 높일 수 있다. -> **time sharing방식을 이용**


2.time slice를 얼만큼으로 설정할 것인가? 

<u>time slice를 너무 짧게 설정한 경우</u>: process들이 프로그램을 마치기까지의 시간이 너무 오래걸리며, CPU의 배정을 다른 process로 배치하는 동안의 overhead(이 시간동안 CPU가 OS에게 배치되기 때문에)가 발생 


<u>time slice를 너무 길게 설정한 경우</u>: CPU가 할당되어지는 총 process의 개수가 작아진다. 즉, 다른 process에게 CPU가 할당되어질 시간을 뺏어, 다른 process에게 CPU가 할당되는 시간을 더 준다고 생각하면 된다. 


_time slice_: time sharing시 각 process에게 CPU가 배정되는 시간(동일어: time Quantum, CPU burst)

**CPU brust는 보통 8ms이내로 설정**

![image](https://user-images.githubusercontent.com/83913407/152634190-1d1d5300-56ee-48af-9c16-d0a4afb5205f.png)


---


### CPU Scheduling

* #### 발생경우

<u>**nonpreemptive한 경우**</u>


-CPU에 의해 실행되고 있는 process(running상태의 process)가 I/O등으로 인해 wait를 한 경우

-process가 주어진 time slice내에서 실행을 다해 종료된 경우
CPU Schedular & Dispatcher


<u>**preemptive한 경우**</u>


-process에게 주어진 time slice를 다 사용해서 running상태에서 ready상태로 넘어간 경우


-wait상태에 있던 process가 ready상태로 넘어간 경우(**이 경우에는 항상 scheduling이 일어나는 것이 아님 -> 다음과 같은 우선순위가 있는 경우에만 발생**)


---

* #### 필요한이유

1. CPU utilizaion 극대화


2. Throughput(생산성) 증대: 단위시간 동안 더 많은 process을 종료시킬 수 있다.


3. Turnaround time 감소: 한 프로그램이 실행되고 종료될 때까지 걸리는 시간이 짧아진다.


4. Waiting time 감소: ready상태에 있는 process들이 cpu가 할당되기까지 기다린 시간의 총 합이 짧아진다.


5. Response time 감소: 사용자가 요청했을 때 처음 결과가 나오는 시간이 짧아진다.


---


* #### CPU Schedular & Dispatcher 

Schedular: CPU를 배정할 다음 process를 결정

Dispatcher: Schedular에 의해 결정된 process에게 CPU를 할당, Context Switch를 담당

![image](https://user-images.githubusercontent.com/83913407/152634631-82fc5fd3-6ddd-4746-9ad2-df72eb491aab.png)


**Dispatch latency: P0 -> P1으로 넘어갈 떄 OS가 실행되는 시간 


---


### Scheduling Algorithms

* #### First-Come, First-Served Scheduling(FCFS)

= 선입선출: 먼저 들어온 것이 먼저 나가고, 늦게 들어온 것이 늦게 나간다.


![image](https://user-images.githubusercontent.com/83913407/152635021-7cc4de9d-b162-49e0-a772-38460dccdab3.png)

_waiting time_ : P1=0, P2=21, P3=26

**_average waiting time_**: (0+21+26)/3 = 15.7


* #### + Convoy effect
CPU brust가 작은 procees가 CPU brust가 큰 process보다 먼저 오는 것이 더 좋음 

![image](https://user-images.githubusercontent.com/83913407/152635153-77dc814b-5440-4abe-9dbc-4d54faeaf365.png)

_waiting time_ : P1=0, P2=5, P3=10

**_average waiting time_**: (0+5+10)/3 = 5


* #### Shortest-Job-First Scheduling(SJF)

= CPU brust가 짧은 process부터 CPU를 배정

1. 동일어: Shortest-Remaining-Time-First(SRTF) Scheduling 

2. 종류: nonpreemptive SJF Scheduling & preemptive SJF Scheduling 


**<u>nonpreemptive SJF Scheduling</u>**

실행 중 더 짧은 CPU brust를 가진 process가 들어와도 중간에 CPU를 넘겨주지 않는다.

![image](https://user-images.githubusercontent.com/83913407/152671312-d70d02b9-5a50-41e2-b249-b63437ea64c3.png)


**<u>preemptive SJF Scheduling</u>**

실행 중 더 짧은 CPU brust를 가진 process가 들어오면 중간에 CPU가 더 짧은 CPU brust를 가진 process에게 넘어간다.

![image](https://user-images.githubusercontent.com/83913407/152671324-2aaf4c4e-f278-46cc-840b-51a86def9008.png)


**<u>SJF는 최적의 algorithms(Optimal)</u>** 

but, next CPU brust는 직접 process를 실행시키기까지는 알 수 없으므로, 이를 현실적으로 구현하는 것은 불가능하므로 이론적으로만 이용 -> next CPU brust의 '예측치' = **Exponential Average**를 통해 SJF를 임의적으로 구현함으로써 다른 algorithms의 효율성을 판단하는 지표로 사용한다. 


**<u>Exponential Average</u>** 

next CPU brust의 예측치

![image](https://user-images.githubusercontent.com/83913407/152671001-90f5d40a-8e53-41d6-a1cd-58767e061d11.png)


* #### Priority Scheduling

= 우선순위가 큰 process가 먼저 CPU에 의해 실행 (**smallest integer = hightest priority**)

![image](https://user-images.githubusercontent.com/83913407/152671355-6c4eb18f-033f-4222-b89b-2f6c56ee6987.png)


**<u>문제</u> : starvation(indefinite blocking)**

우선순위가 낮은 process는 계속 CPU실행 권한을 받지 못해 무한히 대기하는 상황이 발생할 수 있다.


**<u>해결</u> : Aging**

우선순위를 순차적으로 증가시킴으로써, 우선순위가 낮은 process도 언젠가는 CPU를 할당받을 수 있도록 한다.


* #### Round-Robin Scheduling

= time sharing방식에서 사용하는 algorithms


**<u>time slice(time quantum)</u>**

너무 작은 경우: 너무 빈번한 context switching이 일어나 overhead가 발생한다.

너무 큰 경우: FCFS scheduling과 같다. 

**time slice는 적당한 시간으로 설정하는 것이 중요하다. time slice가 크다고 process가 최종적으로 끝나는 turnaround time이 항상 짧은 것(좋은 것)은 아니다**

![image](https://user-images.githubusercontent.com/83913407/152672346-da56aad2-414f-4267-97cd-ae03c6b6bfbd.png)


* #### Multilevel Queue Scheduling

조건: Ready Queue가 여러개인 경우

= Ready Queue마다 '독립된' 다른 Scheduling방식을 이용하여 관리하며 각 우선순위를 둬 CPU의 할당빈도를 다르게 설정

**<u>UNIX에서 사용하는 언어</u>**

1. foreground(interactive)

2. background(batch): 빨리 실행되지 않은 process들이 모여있는 줄 


![image](https://user-images.githubusercontent.com/83913407/152672321-a9beacca-2939-4049-ba04-fb4b67aac28d.png)

우선순위가 낮은 queue일수록 CPU를 할당하는 기회가 적어지기 때문에 그런 queue의 경우에는 time quantum을 길게 주거나 FCFS방식을 이용하는 방법을 이용해 
process의 **starvation문제를 해결**


* #### Multilevel Feedback-Queue Scheduling


**<u>Multilevel Queue Scheduling과의 차이</u>**

Multilevel Queue Scheduling: 자신에게 주어진 time quantum의 시간이 끝나면 본래 자신이 위치해있던 queue로 돌아간다.


Mutilevel Feedback-Queue Scheduling:자신에게 주어진 time quantum의 시간이 끝나면 low priority level의 queue로 돌아간다(**demote**). 또는 high priority level의 queue로도 돌아갈 수 있다(**upgrade**).


* #### Multiple-Processor Scheduling

조건: CPU가 여러개인 경우

**<u>Asymmetric Multiprocessing</u>**

OS를 실행하는 하나의 CPU가 정해져있기 때문에, ready Queue에서 실행을 control하는 CPU도 이 CPU이다. 따라서 하나의 ready Queue만 있으면 된다 -> **load balancing을 맞추기 쉬움**

![image](https://user-images.githubusercontent.com/83913407/152672655-f5238663-e287-4a99-bd8f-48294e1f1abf.png)


**<u>Symmetric Multiprocessing</u>**

OS를 실행하는 CPU가 정해져있지 않고, 모든 CPU가 OS를 실행할 수 있는 접근 권한을 가졌다. 따라서 CPU마다 ready Queue를 가져야 한다. -> **load balancing을 맞추기 어려움**

![image](https://user-images.githubusercontent.com/83913407/152672674-4d3a4ef4-8b4e-4fa1-9695-afe409cf69d5.png)

**<u>laod balancing을 맞추는 방법</u>**

OS의 모듈인 **task**를 통해 **push**와 **pull**기능을 이용

push migration: 많은 process를 가진 CPU는 자신의 process 일부를 task에 의해 다른 CPU에게 넘겨준다.


pull migration: process가 없어 놀고있는 CPU는 다른 CPU로부터 process의 일부를 task에 의해 가져온다.


**Processor Affinity**

Processor Affinity가 낮은 process가 migration시 이동하는 process의 기준

Processor Affinity가 높은 process인 경우: 그 CPU의 cache에 정보가 저장되어 있는 경우


![image](https://user-images.githubusercontent.com/83913407/152673270-2707c69d-210c-4ffc-8cd2-8b860c6b7590.png)


* #### 알고리즘 평가

<u>좋은 알고리즘의 평가기준</u>

1. CPU utilization

2. Response time

3. Throughput


<u>알고리즘 평가방법</u>

1. Simulations: 컴퓨터 시스템안에 프로그래밍을 통해 임의적으로 알고리즘을 구현하여 평가

2. Implementation: 실제 알고리즘의 Schedular을 만들어 algorithm을 평가, 정확하지만 비용이 비쌈.


