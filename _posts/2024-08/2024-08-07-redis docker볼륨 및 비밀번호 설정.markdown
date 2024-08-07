---
layout: post
title:  "TIL(20240807) [redis docker볼륨 및 비밀번호 설정]"
date:  2024-08-07
categories: TIL redis
---

----------------------------------------------------------------------------

[도커 포트포워딩](https://tttsss77.tistory.com/155)

## 💡 redis docker볼륨 및 비밀번호 설정
먼저, 볼륨 및 비밀번호 설정해야하는 이유는 레디스에 저장했던 데이터가 다 사라졌기 때문이다.
그렇기 때문에 docker의 볼륨을 설정해놓으면 데이터를 유지하는 데 사용되기에 컨테이너가 제거되더라도 유지되도록 redis 데이터를 저장할 볼륨을 생성해야한다. 

비밀번호는 관리차원에서 만들어주도록 한다.

docker에다가 redis 실행하는 게 익숙하지 않기 때문에 시행착오를 많이 겪은 것 같다.

해당 cmd 창을 열어서 .. 시행착오의 흔적을 통해 정리하려고 한다.

![image](https://github.com/user-attachments/assets/a7a5f989-6bd2-4b3f-a096-b1cffd36125b)

일단 어떻게든 컨테이너 만들어볼려고 계속 시도했던 흔적들..

```
docker ps -a \전체 컨테이너 조회
docker stop <container_id_or_name> \ 컨테이너 실행종료
docker rm <container_id_or_name> \ 컨테이너 제거
docker rmi $(docker images -q) \ 이미지도 제거
```

다시 컨테이너 조회해보니 아무것도 없었다.

도커 볼륨 생성 redis!

```
docker volume create redis_data \ 도커 볼륨생성

docker run -d \
  --name redis \
  -p 6379:6379 \
  -v redis_data:/data \
  redis:latest \
  redis-server --requirepass password(password지정하세용) 
```

![image](https://github.com/user-attachments/assets/40acfce9-e648-49f8-ada1-9dcd57bb67d5)

redis로 들어가자

```
docker exec -it redis redis-cli
```

비밀번호 설정했다면

```
AUTH (지정한 password)
```

![image](https://github.com/user-attachments/assets/841e8b33-975a-46ec-bd14-d165bdd692f6)

ping 하면 pong...ㅎㅎㅎㅎㅎㅎ;;

스프링이랑 연결!

![image](https://github.com/user-attachments/assets/7874510c-79c4-43da-bfb3-48e2d4f40118)

위와 같이 뜬다면

```
redis-server -> notfound로 뜬다면 아래 명령어 실행

sudo apt update
sudo apt install redis-server
```

![image](https://github.com/user-attachments/assets/244ba9b5-1202-4d48-8b62-b551f01b1fc5)

패스워드 넣고!

![image](https://github.com/user-attachments/assets/dfc467d4-7669-4086-9d51-9f8eecf9875d)

계속하길 원해? continue? 하면 y


```
redis-server --version \설치되어있는지 확인하자
redis-server
```

![image](https://github.com/user-attachments/assets/6f858b52-f030-4be3-9234-b496972ec45b)


다시 레디스로 들어가자

데이터 확인

![image](https://github.com/user-attachments/assets/f26aba70-56e3-4023-8b20-b62207f128b0)

도커에서 레디스 껐다가 실행해도 데이터가 남아있는 것을 확인! 

넣었던 더미데이터 삭제하기

![image](https://github.com/user-attachments/assets/022490d4-4f54-44e8-a01e-a36a86105977)

## 💡 apache jmeter 성능테스트

[apache jmeter 성능테스트](https://blog.naver.com/wisestone2007/222160380337)

포인트 랭킹조회를 QueryDSl과 redis로 구현해서 성능테스트 해보았다.


더미데이터 유저 500명 넣음

![image](https://github.com/user-attachments/assets/6620f9e9-1692-4217-9100-639ca8bb59e5)

QueryDSL로 구현한 랭킹조회 api호출
![image](https://github.com/user-attachments/assets/d1337713-1760-4914-bbeb-a524903208a1)

redis로 구현한 랭킹조회 api호출
![image](https://github.com/user-attachments/assets/59ac20a4-e73b-40fc-8418-6d8cedf42f16)

포스트맨으로 1번 조회 요청을 했을때랑 2번 조회를 했을 때랑
속도가 너무 달라서 테스트에 신뢰성이 떨어졌다..

querydsl
![image](https://github.com/user-attachments/assets/44fae4db-104d-441e-bafd-2c17b5b1d70b)
redis
![image](https://github.com/user-attachments/assets/0ba15074-5634-4441-b42c-d01df8f516f8)

처음에 저것만 보고 와 완전 반 이상을 줄였다 했는데..
jmeter로 하니까 또 저렇게 데이터가 나오니.. 음..
어쨋든 500ms는 줄인거 같아서 좋은데.. 극명하게 저렇게 차이났던건 뭐지..ㅠ_ㅠ?.. 아직 부족한가부다.. 