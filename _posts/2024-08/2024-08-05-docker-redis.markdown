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

