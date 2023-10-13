---
title: "Docker Instructions(2) - container"
categories:
    - Open source
tags:
    - Open source
date: 2023-10-13
toc: true
---

Container 관련 명령어

### docker 명령어 - Container 

### Container 목록 출력

```
docker container ls #최신
docker ps #예전
```

- docker container ls :  정지/종료 된 container들은 보여주지 않음 → 실행 중인 container들만 출력

- docker container ls -a: 실행 중인 container뿐만 아니라 정지/종료 된 컨테이너를 포함해서 모두 출력

#### Container 실행

```sql
docker [container] run [options] 이미지명[:태그] [실행명령]
```

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/c6f52b6c-5530-4fde-b641-f671fc5533bb)

- 이미지를 기반으로 컨테이너 실행

- [pull] + create + start + [command]

    - 만약 해당 이미지가 local에 없는 경우, 자동적으로 다운로드(pull)해줌

    - command명령을 주면 container 내에서 실행됨

- options 종류

    - 보통 -i와 -t 옵션은 같이 씀 : -it

        - -it옵션을 쓰면 컨테이너 shell에 붙어서 작업이 가능해짐

| -i | 키보드 입력을 컨테이너 표준 입력에 연결하여 키보드 입력을 컨테이너 shell 등에 보냄 |
| --- | --- |
| -t | 터미널을 통해 대화형 조작이 가능하도록 함 |
| -d | 백그라운드로 컨테이너를 돌려 터미널과 연결하지 않음 |
| - -name | 컨테이너에 이름을 설정함. 시스템에서 유일한 이름이여야 하며 이 옵션을 생략하면 자동적으로 unique하게 만들어진 이름이 부여 |
| - -rm | 컨테이너가 종료되면 종료 상태의 컨테이너를 자동으로 삭제 |
| - - restart | 컨테이너 종료 시 적용할 재시작 정책 지정                                     [no]|on-failure|on-failure:횟수n|always|                              |
| - -env | 컨테이너의 환경 변수 지정 |
| -v, - -volume=호스트 경로:컨테이너 경로 | 호스트 경로와 컨테이너 경로의 공유 volume을 설정(Bind mount) |
| -h | 컨테이너의 호스트명 지정(미지정 시 컨테이너 ID가 호스트명으로 지정) |
| -p [Host포트]:[Container 포트], - -publish | 호스트 포트와 컨테이너 포트 연결 |
| -P,  - -publish-all=[true|false] | 컨테이너 내부의 노출된 포트를 호스트의 임의의 포트에 게시 |
| - -link=[container:container_id] | 동일 호스트의 다른 컨테이너와 연결 설정으로 IP가 아닌 컨테이너 이름을 이용해 통신  |


#### Container run 옵션 : - -name

```
docker run --name 컨테이너이름 이미지명[:태그]
```

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/c6f52b6c-5530-4fde-b641-f671fc5533bb)

- 동일한 이름의 container을 생성하려고 하면 자동적으로 막아짐

    - 해결방법 1) 기존 동일 이름의 container을 rm으로 삭제하고 생성

    - 해결방법 2) 애초에 이런 일이 발생하지 않도록 처음 만들 때부터  - -rm 옵션을 이용해 container을 run

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/98c0b004-c0d6-44f5-87ea-9df99c6400bd)


#### Container run 옵션: - -rm

- 앞서 언급했던 이름 겹침의 문제를 예방할 수 있음

- container test 시 사용하면 편리


#### Container 생성

```
docker [container] create [options] 이미지명
```

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/d086117f-e8fc-4520-8f60-b55c567f9b2a)

- create도 [pull]의 기능을 가지고 있음

- create를 통해 만든 container의 status는 Created

#### Container 시작

```
docker [container] start [options] 컨테이너명
```

- 생성된 snapshot(이미지)를 동작

#### 실행 중인 Container에 접속

```
docker [container] attach [options] 컨테이너명
```

- 실행 중인 컨테이너에만 접속이 가능

- container을 run할 때 -d 옵션을 이용해 백그라운드에서 실행했다면 attach로는 종료만 가능 → 다른 동작 불가능함

    - exec명령어를 이용해 실행명령을 container에게 주는 방식으로 할 수 있음

- 원래 실행 명령이 /bin/bash이므로 명령을 자유롭게 입력할 수 있음

    - 단, DB나 서버 애플리케이션을 실행하면 출력만 확인 가능하고 입력은 불가

- bash shell에서 exit 또는 ctrl+D를 입력하면 컨테이너가 정지됨

- ctrl+q+p를 입력하면 컨테이너가 정지되지 않고 실행 중인 상태로 빠져나옴

**[ ctrl +q+p - 실행 중인 상태로 빠져나옴]** 

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/c65f710c-4414-4f0a-9587-b2c76b950b14)

**[ exit - 종료 된 상태로 빠져나옴 ]**

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/464330c3-2732-402e-83f8-9cce165a7897)

#### Container 삭제

```
docker [container] rm [options] 컨테이너명
```

- docker rm 컨테이너명: 정지된 상태의 컨테이너만 삭제

- docker rm -f 컨테이너명: 동작 중인 컨테이너도 삭제 가능 (SIGKILL)


#### 실행 중이지 않은 모든 Container 삭제

```
docker container prune [options]
```

- 실행 중이지 않은 정지/종료상태의 컨테이너들도 저장공간을 차지

- 실행 중이지 않은 모든 컨테이너를 일괄 삭제할 수 있음

#### Container 정지

```sql
docker [container] stop [options] 컨테이너명 
```

- 실행 중인 컨테이너를 정지

- -t <초> option으로 시간을 설정할 수 있음

```
docker [container] -t 10 ubuntu:1.0  #10초 후 컨테이너 정지
```

- SIGKILL 시그널을 전송해 container 프로세스를 정지

#### Container 재시작

```
docker [container] restart 컨테이너명
```

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/9d8c54bd-cc83-4f9b-a610-14ad48ac3976)

- stop+start

- container를 다시 시작

- 기존 container 프로세스를 정지하고 새로운 container 프로세스를 시작

- container동작에는 영향을 주지 않고, host의 PID(Process ID)만 변경

#### 외부(Host)에서 Container 안의 명령을 실행

```
$docker [container] exec 컨테이너명 실행명령
```

- 컨테이너가 실행되고 있는 상태에서만 사용가능
   
    - attach도 컨테이너가 실행되고 있는 상태에서만 사용이 가능
   
    - 정지된 상태에서는 사용이 불가능

- 이미 실행된 컨테이너에 apt-get, yum명령으로 패키지를 설치하거나 각종 daemon을 실행할 때 사용

- **Container run시 -d옵션을 통해 백그라운드로 container을 실행하는 경우에는 터미널에서 작업이 불가능**

    - attach를 통해 작업할 수 없음(프로세스 종료만 가능)

    - 이러한 경우에는 exec 명령어를 이용해줘야함
    
    ```
    docker exec -it 컨테이너명 /bin/bash
    ```
    

**[ 백그라운드에서 실행하는 container의 attach 명령 ]** 

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/b22764b4-a311-4036-9f67-9f926c2cbd5a)

**[ 백그라운드에서 실행하는 container의 exec 명령 ]**

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/2b2c582e-bef6-4f79-b04d-d9bc919c9ff8)


#### 표준 출력 Host에 연결

```
docker [container] logs 컨테이너명
```

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/8afc1624-e61e-40aa-a602-ccee0f6035a0)

- 현재 실행 중인 특정 docker container의 출력 내용을 확인 (STDOUT, STDERR)

- -f 옵션 사용시 새로 출력되는 내용을 계속 host로 출력할 수 있음

**[ 중지/종료된 container의 logs 표시 ]**

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/0b25ecd4-1b24-45a2-9d96-1670a1df9ca7)

#### Container와 Host 간의 파일 복사

```
docker [container] cp 컨테이너명:원본파일경로 대상파일경로 # container -> host로 cp
docker [container] cp 원본파일경로 컨테이너명:대상파일경로 # host-> container로 cp
```

- 해당 명령어는 local에서만 진행

**[ host→container로 cp]**

- $ touch hello.txt → local(./)에 hello.txt 파일 추가

- $ echo hello > hello.txt

- $ docker cp hello.txt httpd:/hello.txt

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/15c806e0-c1e0-424d-a492-5633c72305d3)

**[ container → host로 cp]** 

- $ echo hi > hello.txt  → in container

- docker cp httpd:/hello.txt ./ → in local

    - container안에서 하는 것 아님!

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/7b12091b-66f6-4d76-9a26-58161b414a64)

#### Container 사용 현황 확인

```
docker [container] stats 컨테이너명
```

- container의 시스템 리소스 사용량을 실시간으로 확인할 수 있음

    - CPU 사용량(%), memory 사용량과 제한량, I/O, PIDS등

- 리눅스 계열 운영체제의 top명령어와 비슷한 역할

- 중지된 container의 상태도 확인 가능 → 다 0으로 되어있음


#### Container에서 변경된 파일 확인

```
docker [container] diff 컨테이너명
```

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/696bd203-c707-4d83-83e2-182423ec026d)

- 이미지로부터 컨테이너가 실행된 이후로 변경된 사항들에 대해 보여줌

    - 변경된 파일, 디렉토리의 목록을 출력

- 비교 기준: 컨테이너를 생성한 이미지 내용

- A: 추가된 파일, C:변경된 파일, D:삭제된 파일


#### Container의 세부 정보 확인

```sql
docker [container] inspect 컨테이너명
```

- 컨테이너의 세부 내용을 출력

    - 할당 IP주소, image 명 및 hash값, network 정보 등


#### Container의 변경 사항을 이미지로 생성

```
docker [container] commit [options] 컨테이너명 이미지명[:태그]
```

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/2ed3c578-ead0-4deb-a508-52fd36b9c83b)

- 기존의 이미지와는 다른 새로운 명의 이미지를 생성

- 새롭게 추가된 layer을 제외하고는 기존 이미지와 동일한 layer들은 공유됨

    - 새로운 이미지를 생성했다해서 그 layer들이 또 생성되는 것 아님 X

    - = 새롭게 추가된 layer을 제외하고서는 나머지 layer들의 hash값 같음

    - 만약 기존의 이미지가 4개의 layers을 가지고, 이후 하나의 파일을 R/W해서 추가한다면 5개의 layers가 됨

- options 종류

| -a | author 설정  | ex) “eunjin” |
| --- | --- | --- |
| -m | commit message 설정 | ex) “add hello.txt file in root” |

- 새롭게 생성된 이미지들의 layer들은 모두 read-only → 이미지로 생성되면 새롭게 생성된 layer도 read-only

### Run과 Start의 차이

- run은 새로운 컨테이너를 이미지로 부터 만든 후 그 컨테이너를 가동
   
    - [pull] + create 기능을 추가적으로 가지고 있음 (↔ run)

- start은 기존에 존재하는 컨테이너를 실행시킬 때 사용
 
    - create로 만들어져 존재하는 컨테이너를 실행시킴
 
    - + stop은 기존에 실행되는 컨테이너를 중지시킴

### 실행 중인 Container을 삭제할 수 있는 방법 정리

1.rm의 -f옵션 이용해 강제 삭제

```
docker [container] rm -f (실행중인)컨테이너명
```

2.실행 중인 컨테이너를 정지 시킨 후 삭제

```
docker stop (실행중인)컨테이너명
docker [container] rm (중지된)컨테이너명
```