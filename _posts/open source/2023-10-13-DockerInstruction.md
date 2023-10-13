---
title: "Docker Instructions(1) - image"
categories:
    - Open source
tags:
    - Open source
date: 2023-10-13
toc: true
---

image 관련 명령어

### Docker 이미지 명령어 구성

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/d85289be-5649-4230-beeb-c88189ecd8f7)


- dockerfile: docker에서 이미지를 생성하기 위한 용도로 작성하는 파일, 즉 만들 이미지에 대한 정보를 기술해둔 탬플릿 → 이 dockerfile을 build하면 image가 만들어짐 

- pull, search: docker에 로그인을 하지 않아도 이용할 수 있음


### docker 명령어 - 이미지

#### 이미지 검색

```
docker search 이미지명[:태그] 
```

- 이미지 검색은 docker hub의 로그인 기능을 필요로 하지 않음

- docker hub 홈페이지에서 직접 검색이 가능


#### 이미지 다운

```
docker [image] pull [options] 이미지명[:태그]
```

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/9b8e342d-f9c4-4c67-8097-c3b1c763b233)


- docker hub의 registry로부터 local로 해당 이미지 받음

- 이미지 다운은 docker hub의 로그인 기능을 필요로 하지 않음

- 이미지의 기본 형식 <네임스페이스>/<이미지명><태그>

    - docker hub의 default 네임스페이스는 library

    - [docker.io/](http://docker.io/)library/httpd:lates
    
    - docker hub의 default 태그는 latest

- 다운로드한 이미지는 여러 레이어로 구성

    - 이미지마다 포함된 레이어의 개수 다름

    - 각각의 레이어는 hash 값으로 구분

- 이미지의 digest: docker registry에서 관리하는 이미지 고유 식별 값(hash 값)

- 이미지를 다운받는 방법은 여러가지

| 명시적으로 최신 버전 지정 | $ docker pull httpd:latest |
| --- | --- |
| 이미지 식별 정보인 digest 지정 | $ docker pull httpd:sha256:5201…(생략) |
| docker hub registry 명시적으로 지정 | $ docker pull library/httpd:latest                              $ docker pull http://docker.io/library/httpd:latest               $ docker pull index.docker.io/library/httpd:latest           |
| 외부 registry 주소를 이용                        ( 이러한 경우 웹 주소 url에서 도메인 시작 주소인 https://를 붙이지 않고 이미지 주소를 써야함) | $ docker pull gcr.io/google-samples/hello-app:1.0 |


#### 이미지 목록 출력

```sql
docker image ls #최신
docker images #예전
```

- 활용도면에서는 최신 형태의 명령어가 좋음

    - docker <객체이름> ls로 통일해서 사용할 수 있기 때문

    - ex) docker container ls

- created는 이미지가 docker hub에 만들어진 시점

    - local에 다운받았을 때의 시점 X

- 태그만 다른 동일한 이미지명을 가진 이미지를 한 번에 조회하는 방법

    - 태그가 다른 이미지들이 출력됨

```
docker images 이미지명 #이때는 태그를 붙이면 안됨
```

#### 이미지 세부 정보 조회

```sql
docker image inspect [options] 이미지명[:태그]
```

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/abd31d13-ca0f-4af1-aba7-bd71491567cb)

- Id: 이미지 id

- Created: 생성일

- DockerVersion: Docker 버전

- Architecture: CPU 아키텍쳐

- RootFS: 이미지 다이제스트 정보, 이미지가 몇 개의 layers로 구성되어 있는지 알 수 있음

- GraphDriver: 이미지 layer 저장정보

- ENV: 환경변수

- Cmd: 해당 이미지로 컨테이너를 만들었을 때 자동으로 실행되는 명령어들

- WorkingDir: 컨테이너 생성 시의 시작 경로

#### 이미지 히스토리 조회

```
docker image history [options] 이미지명[:태그]
```

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/2a1ca901-720e-4ae4-9b88-4845e9e6bb06)

- 위쪽부터 최신 내용

- 이미지를 구성하는 layer의 수행 명령, 크기 등 조회가 가능

- 즉, 해당 이미지의 변경 사항들을 추적할 수 있는 명령

- 반드시 layer가 생성되었다고 해서 사이즈가 있어야 하는 것은 아님 → 0B여도 layer가 생성될 수도 있음

    - 사이즈 존재 → layer 생성 (O)

        - 사이즈가 존재하면 layer는 생성된 것

    - 사이즈 존재하지 않음 → layer생성되지 않음(X)

        - 이것은 틀린 전제

        - 사이즈가 존재하지 않더라도 layer가 생성될 수 있음

- Image ≠ layer

    - 이미지는 여러 개의 layer들로 이루어진 경우도 있기 때문에 layer가 image와 같다고 할 수 없음

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/f69f4712-bf10-4416-ae30-196560bc6483)
    

#### 이미지 태그 설정하기

```
docker tag 원본이미지명[:태그] 참조이미지명[:태그]
```

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/e3751276-f621-4a43-b25f-e3ce5bd73315)

- tag를 달리해서 이미지를 새로 만들 수 있음

- 이때 inspect를 이용해 이미지의 세부 정보를 조회해 보면 같은 layer를 공유한다는 것을 알 수 있음 → layer의 hash값 동일, image id도 동일

- 즉 태그를 달리하여 이미지를 생성했더라도 그 layer들이 다시 만들어지는 것 아님


#### docker 로그인

```
docker login
```

- 이미지를 docker hub에 push하기 위해서는 로그인이 필요

- 로그인을 하면 .docker 폴더의 config.json파일에 암호가 저장됨

- 평문으로 저장되지는 않지만 bash64인코딩을 통해 암호 키값이 저장 → base64는 디코딩하여 평문으로 만든 후 해킹할 수 있음

- 따라서, 로그인 기능을 필요하지 않는 작업에서는 항상 logout상태를 유지하도록 함

#### docker 로그아웃

```
docker logout
```
- 로그아웃을 하면 .docker폴더안의 config.json파일의 auth 값 삭제

- docker info에서 사용자명 제거

- 필요한 경우를 제외하고는 로그아웃 상태 유지 → 보안을 위해서


#### 이미지 삭제

```
docker image rm [options] {이미지명[:태그] | 이미지 id} #최신
docker rmi [options] {이미지명[:태그] | 이미지 id} #예전
```

- docker 이미지 삭제 시 latest버전을 제외하고는 반드시 태그 명을 명시해야 함

- 이미지 ID를 이용한 이미지 삭제 시, 그 이미지 ID가 서로 공유되고 있다면 (이미지 ID가 다른 태그를 참조한다면) 오류 발생

- 태그가 지정된 모든 관련 이미지를 삭제하려면 -f 옵션 이용

    - 같은 이미지 ID가진 관련 이미지들이 삭제됨

    ```
    docker image rm -f 이미지ID
    ```
    

#### 모든 docker 이미지 삭제

```sql
docker image prune
```

[ 원래 이미지 목록 ]

![image](https://github.com/dareunk/dareunk.github.io/assets/83913407/741c4d6e-e0f5-4deb-952c-f8173f9d7e05)

- docker image prune: 다운로드 한 이미지 중 하나 이상의 컨테이너가 연결되지 않은 모든 이미지를 제거, 이때 dangling 이미지를 모두 삭제

    - dangling 이미지

        - 태그가 붙지 않은 이미지

        - 어떤 컨테이너도 참조하지 않는 이미지

    - ex) 원래 이미지 목록에서 보면 모든 이미지가 태그가 붙어있기 때문에 삭제되지 않음

- docker image prune -a : 하나 이상의 컨테이너가 연결되지 않은 이미지를 제거 → 참조하는 이미지 빼고 다 삭제

    - ex) container가 참조하는 이미지는 busybox밖에 없으므로 httpd:2.0 이미지 삭제
    

