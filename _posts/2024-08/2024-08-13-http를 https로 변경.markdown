---
layout: post
title:  "TIL(20240813) [AWS ACM(SSL인증서발급) http->https]"
date:  2024-08-13
categories: TIL AWS HTTPS
---

----------------------------------------------------------------------------


아래 블로그를 참고하여 진행하였습니다.
이전과정까진 진행완료한 상태라 ACM 인증서 요청 부터 작성하게 되었습니다. 

[EC2 HTTPS로 연결하기](https://woojin.tistory.com/m/93)

### ✨ 들어가기전
- 특정 IP에 부여된 naver.com, google.com 과 같은 주소들을 도메인이라고 합니다. https 연결을 위해서는, 도메인을 가지고 있어야 합니다.
- 도메인이 없을 경우 인증서요청이 불가능합니다. 저희는 "가비아"에서 도메인을 구매하였습니다. 


## 💡 1. ACM 인증서 요청

AWS 접속 후 검색창에 ACM이라고 치면 아래 Ceriticate manager가 나오며 클릭한 후 들어가면 인증서 요청이라는 버튼 클릭 
![image](https://github.com/user-attachments/assets/c60fe074-1dd4-43bd-9db2-b70d26cea04e)

![image](https://github.com/user-attachments/assets/a464a2e3-edca-4c1d-849e-408f5fce1f48)

![image](https://github.com/user-attachments/assets/7595b6a2-6fc6-45b4-aedc-bba4a27b9c67)

** 혹시 동일한 도메인으로 여러사이트를 보호하고 싶으신 경우에는 와일드카드 (*)를 사용해서
ex) *.challengersbod.store 
라고 입력하시면 됩니다.

![image](https://github.com/user-attachments/assets/a6d47634-b61c-4605-9666-6ed027c1b8cc)

![image](https://github.com/user-attachments/assets/5d57779a-90b5-40e5-8a3e-33282c22655a)

요청!

## 💡 2. 발급받은 인증서 Route53 레코드 생성

발급받은 인증서 안으로 들어가게 되면

![image](https://github.com/user-attachments/assets/9c6ba3f8-13a4-4948-b0ec-6928fe987758)

현재 저는 발급받은 상태라 성공이라고 뜨지만 아니라면 대기? 아니면 검증중이라고 뜰 것 같습니다. 

발급전이면 도메인 앞에 체크박스가 있는데, 체크박스를 클릭 후 
레코드 생성 클릭

## 💡 3. EC2 보안그룹 규칙 추가

해당 인스턴스의 보안그룹으로 들어와서 인바운드 규칙을 추가

![image](https://github.com/user-attachments/assets/4e8b5b50-9cc7-4a88-961c-33e7b60e590f)

![image](https://github.com/user-attachments/assets/39af6953-2595-4324-8eb8-4ed3843b423d)

https 443 ip4 ip6 ,
http 8080(백엔드), 80(프론트) ip4/ip6 추가해주었습니다.
-> HTTP, HTTPS 모든 요청을 열어준다는 의미

아직 AWS에 대해서 시행착오를 겪는 중이라 부정확할 수 있습니다. 현재 docker compose로 CI/CD 구축한 상태입니다.
docker.yaml 파일에 depends on 백엔드가 되어있는 상태입니다.
그래서 해당 설정을 저렇게 해준 상태입니다.

## 💡 4. EC2 대상그룹 생성
-> 인스턴스의 가용영역 확인/보안그룹 확인 필요

![image](https://github.com/user-attachments/assets/b9b49137-9f88-40fa-b879-5d01e55b4170)

대상그룹 생성 클릭

![image](https://github.com/user-attachments/assets/bcce5358-de69-4000-81fb-6ce31a3dc37c)


![image](https://github.com/user-attachments/assets/7e802538-d0b6-4c12-a4ed-4ae08b71de30)

대상이름을 맞게 작성해주고 저희는 백엔드를 8080포트로 설정해놓았습니다. 그리고 VPC를 설정해주세요.(해당 인스턴스와 동일하게 설정하셔야 합니다.) 그리고 다음버튼 클릭!

![image](https://github.com/user-attachments/assets/73cf4165-e1ba-428f-adb8-697af8dd79ba)

아래 보류중인것으로 포함 클릭 

저희는 대상 그룹을 프론트도 추가하였고, 포트는 80으로 설정하였습니다.

## 💡 4. 로드밸런서 생성

ec2의 로드밸런싱 아래 로드밸런서를 클릭합니다.

![image](https://github.com/user-attachments/assets/97c682cd-581c-4a99-b9d1-33ae36e51933)

![image](https://github.com/user-attachments/assets/f2d83b4e-169d-4a1e-ba39-613e34edb81c)

![image](https://github.com/user-attachments/assets/3f972f4d-cd07-44f4-b989-3506e1890b3d)

로드밸런서 이름을 작성해주시고

![image](https://github.com/user-attachments/assets/3dc3deca-91a3-48ef-96cc-2199db580447)

해당 인스턴스의 VPC선택 후 해당 인스턴스의 가용영역,서브넷을 선택해줍니다.(네트워크 매핑) 인스턴스의 세부정보에서 가용영역을 확인할 수 있으며 2개이상 체크를 해야합니다. 


해당 인스턴스의 보안그룹을 설정해주시고 
8080에 해당하는 대상그룹을 설정해주세요.

----이부분에서는 정확한 방법인지는 모르겠지만 현재 저희 프로젝트 상태에서 맞춰 진행한 부분이니 참고 부탁드립니다.

백엔드->
![image](https://github.com/user-attachments/assets/e849ad37-4950-4f8e-bc26-6dedeaf13722)
challegers-lb-target(백엔드)

프론트->
![image](https://github.com/user-attachments/assets/3e2eb804-64c1-442d-8269-4134e896b3ff)
challegers-front-lb-target(프론트)

![image](https://github.com/user-attachments/assets/fb192a92-9f58-4e85-9c02-315f61d2976b)

그리고 아래 ACM에서 발급받은 인증서를 선택해주고 로드밸런서를 생성합니다. 

생성한 로드밸런서 안으로 들어가 리스너 및 규칙을 보면 아래와 같이 등록되어있는 것을 볼 수 있습니다.

![image](https://github.com/user-attachments/assets/47099e9a-a0db-4f8d-adfb-71460e4ae266)

## 💡 5. 도메인 레코드 생성
route53으로 이동.
A레코드를 만들기 위한 과정입니다.

![image](https://github.com/user-attachments/assets/81877afc-a304-473b-a6d4-28dd0dfad362)

해당 호스팅 영역으로 들어가서

![image](https://github.com/user-attachments/assets/0bcd0b5e-678b-4865-955a-1ffbb21047ee)

![image](https://github.com/user-attachments/assets/875ab7a5-73f5-4580-be98-6914f4c9f1c9)

별칭으로 바꿔주고 아래와 같이 설정 아까 생성했던 로드밸런서를 체크해주고 레코드 생성 하시면 됩니다!

![image](https://github.com/user-attachments/assets/c6e3f2d7-f656-49fd-9b1e-68d7dfa1b1ce)

A가 생성되었다면 완료!

```
vue.js로 구현하셨다면 axios.js 설정에 백엔드 서버를 https와 해당도메인 주소 :8080을 붙여 수정해주셔야 합니다.
ex) https://challengersbod.store:8080
그리고 백엔드 webconfig 관련해서 allowedOrigin도 https, 도메인주소로 변경해주시면 됩니다.(CORS설정)
ex) https://challegersbod.store
```
---------------------------------------------------------

개념정리가 필요할 것 같다.
산넘어 산.. 프로젝트가 끝나면 정말 CS공부와 이때까지 개념이 정립되지 못한 부분을 확실하게 짚어가야할 듯 하다..