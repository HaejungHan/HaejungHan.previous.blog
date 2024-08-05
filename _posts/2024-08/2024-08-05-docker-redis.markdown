---
layout: post
title:  "TIL(20240805) [docker-redis]"
date:  2024-08-05
categories: TIL 쿼리최적화
---

----------------------------------------------------------------------------

## 💡 Docker를 활용해서 Redis 시작하기

1. 먼저 도커 시작하기

![image](https://github.com/user-attachments/assets/240e554b-0dab-4b0c-8feb-d4247690890f)


명령어에 익숙해지기 위해서 기록해두기

```
1. 도커 시작하기
   # systemctl start docker 
2. 리부팅시에도 자동 시작하게 설정
   # systemctl enable docker
3. 도커 상태 보기
   # systemctl status docker
4. 도커 버전 확인 
    # docker version
```

[Docker Redis Introduction](http://redisgate.kr/redis/education/docker_intro.php)

[[리눅스] Docker로 redis설치하기](https://velog.io/@junminlover/%EB%A6%AC%EB%88%85%EC%8A%A4-Docker%EB%A1%9C-redis%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)



2. 도커 이미지 다운로드

![image](https://github.com/user-attachments/assets/fa7a9fbc-560f-454d-94b5-587de89a7a24)

```
$ docker pull redis
$ (sudo) docker images
```

3. 도커 컨테이너 생성 및 도커 컨테이너 상태 확인

![image](https://github.com/user-attachments/assets/f60d887c-d33a-4ce6-9b8e-2b47e28a45ae)


```
$ (sudo) docker run -p 6379:6379 --name docker_redis redis
$ (sudo) docker ps -a
```

```
* Redis version=7.4.0, bits=64, commit=00000000, modified=0, pid=1, just started
* Running mode=standalone, port=6379.
```

대략적으로 서버가 방금 시작했다.. 이런 메세지😁

4. 도커 컨테이너 실행

![image](https://github.com/user-attachments/assets/936e876f-13bd-44f7-974b-04ca31585b8f)

```
$ (sudo) docker ps -a
$ (sudo) docker start docker_redis
$ (sudo) docker ps -a
```

5. 서비스 등록
- 참고) redis docker container의 실행을 서비스로 만들어서 등록해놓으면, 매번 리눅스 서버를 재시작할 때마다 docker container가 자동으로 실행되기 때문에 편리하다.

1) /etc/systemd/system/redis.service 파일을 생성

```
$ sudo nano /etc/systemd/system/redis.service
```
위 명령어를 입력하고

```
[Unit]
Description=Service to run docker container for redis

[Service]
Type=simple
ExecStart=sudo docker start docker_redis
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

서비스 정의 ctrl+o 다음 파일이름 확인 후 enter, 다음 빠져나오기 ctrl+x

```
$ systemctl start docker
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
$ systemctl status docker
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
```
위처럼 명령어가 안먹히는 상황

찾아보니 

 systemctl start docker 해당명령어 사용 불가 
 나의 경우 SysV init  인듯 명령어가 다르다

- systemd(systemctl)를 사용할 수 없거나 Docker가 다른 초기화 시스템(예: SysV init 또는 Upstart)에 의해 관리되는 환경에서 작업하는 경우 시스템에 적합한 명령을 사용해야 한다. 
 

 ```
 1. 도커 시작하기
   # sudo service docker start
2. 리부팅시에도 자동 시작하게 설정
   # sudo update-rc.d docker defaults
3. 도커 상태 보기
   # sudo service docker status
 ```

6. Docker의 redis-cli로 접속하기

```
$ docker exec -it docker_redis(name) redis-cli -p 6379
```


** 자주 사용하는 명령어

```
- 레디스 서버 실행하기

docker run --name myredis -d -p 6379:6379 redis

- Docker의 redis-cli로 접속하기

docker run -it --link myredis:redis --rm redis redis-cli -h redis -p 6379
```

## 💡 포인트 랭킹 조회 기능구현하기

![image](https://github.com/user-attachments/assets/7b984c3f-8968-48d9-b84e-1a10a093da4b)

왜 typedTuples size =0 일까... 후..

알고보니 dto에 값을 잘못 넣어주고 있었다.

멍충..

postman 조회

![image](https://github.com/user-attachments/assets/127decc5-d0f9-4bd2-b3f0-c44672f0e074)

Redis cli 에서 데이터 확인시
```
127.0.0.1:6379> ZRANGE ranking 0 -1 WITHSCORES
1) "\xec\x88\x9c\xec\x9d\xb4\xec\x9e\x85\xeb\x8b\x88\xeb\x8b\xb9"
2) "500"
3) "\xec\xb2\xa0\xec\x88\x98\xec\x9d\xb4\xec\x97\x90\xec\x9a\xa9"
4) "500"
5) "\xec\x98\x81\xed\x9d\xac\xec\x9e\x85\xeb\x8b\x88\xeb\x8b\xa4"
6) "3000"
7) "adminNick"
8) "10000"
```
\xec\x88\x9c\xec\x9d\xb4\xec\x9e\x85\xeb\x8b\x88\xeb\x8b\xb9 이게 뭐냐구우..........?

- 유니코드 문자를 16진수로 표현한 것. Redis CLI는 일반적으로 인쇄할 수 없는 문자 또는 유니코드 문자를 이 인코딩된 형식으로 표시된다. CLI를 통해 볼 때 Redis가 인코딩된 형식으로 문자열 값을 저장하는 것은 일반적이며, 이는 주로 기본 ASCII 세트에 포함되지 않은 문자를 표시하기 위한 것이라고 한다. 

😅 이거 반환값을 아이디(영어)로 바꾸자고 해야할지 고민된다..팀원들이랑 이 부분에 대해서 얘기를 해봐야겠다. 다른 방법이 있으려나..? 포스트맨 조회시에는 닉네임이 잘 받아져서 나오긴 한다만..

## ⭐redis - 시간복잡도 확인하기



## 💡 Docker 컨테이너에서 Redis CLI 관련 명령어

1) 순위 데이터 확인

```
ZRANGE ranking 0 -1 WITHSCORES
```

2) Redis 데이터베이스의 모든 키를 확인

```
KEYS *
```

3) 특정 유저의 ranking 확인

```
ZSCORE ranking user1(username)
```

종료는 exit 또는 ctrl+D



커밋하고 푸시하는 과정에 갑자기.. 에러
뭐지.. 의존성 추가도 해놓고 다했는데 왜왜왜.. push만 하면 되는데 왜왜왜... 잘되었는데 왜 갑자기..ㅠ_ㅠ
```
Error processing condition on org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration.propertySourcesPlaceholderConfigurer
```

인텔리제이 껏다가 키니까 된당.....😂




