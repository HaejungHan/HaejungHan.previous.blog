---
layout: post
title:  "TIL(20240808) [최종프로젝트 - 트러블슈팅1]"
date:  2024-08-08
categories: TIL redis
---

----------------------------------------------------------------------------


## 🔒 'wsl' 명령을 찾을 수 없는 에러

도커데스크탑 실행했을 때

```
wsl --distribution Ubuntu
<3>WSL (10) ERROR: UtilTranslatePathList:2866: Failed to translate C:Windows\system32\
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.153.1-microsoft-standard-WSL2 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This message is shown once a day. To disable it please create the
/home/user/.hushlogin file.
user@DESKTOP-IIC3B9K:/mnt/c/Windows/system32$ wsl --distribution docker-desktop
Command 'wsl' not found, but can be installed with:
sudo apt install wsl
```

'wsl' 명령을 찾을 수 없는 에러

간단하게 cmd 창에서 sudo apt install wsl 해주면 다시 실행되었다.

## 🔒 Unable to connect to Redis

레디스 연결이 안되는 문제

레디스 서버 실행 

```
redis-server
docker ps -a
docker start 컨테이너ID
docker exec -it 6196285ad780 redis-cli
```

## 🔒 이메일 인증 구현 시 TLS연결문제

```
530-5.7.0 Must issue a STARTTLS command first. For more information, go to
```

블로그에서 여기저기 찾아보니 
spring.mail.properties.mail.smtp.starttls.enable=true 설정에 관한 문제였다...

```java
private Properties getMailProperties() {
	Properties properties = new Properties();
	properties.setProperty("mail.smtp.starttls.enable", "true"); // 이부분....😂
	properties.setProperty("mail.smtp.starttls.required", "true");
	properties.setProperty("mail.smtp.auth", "true");
	properties.setProperty("mail.smtp.ssl.trust", host);
	properties.setProperty("mail.debug", "true");
	return properties;
  }
```

그러면 starttls.enable 설정을 내가 잘못해주고 있다는 건데.. 어디서 잘못된걸까.. 자세히 들여봤다. 
이전까지 "spring.mail.smtp.starttls.enable" 이러고 있었다..
spring만 빼주니까.. 작동한다...
반드시 **mail.smtp.starttls.enable** 이 한줄이 적혀져야 저런 문제가 발생하지 않는다.

아래 두 번째 블로그에서는 spring.이 들어가도 정상적으로 작동한다고 하는데.. 그 어떤 부분에서 원인이 된건지.. 아직 찾지 못했다. 더 들여다 봐야겠다..ㅠ_ㅠ 

[mail.smtp.starttls.enable문제](https://m.blog.naver.com/samwalto/220198555776)

[Must inssue a STARTTLS command first. 오류 해결](https://velog.io/@hyemin0111/Spring-Boot-JavaMailSender-535-5.7.0)


레디스에 이메일 인증코드가 정상적으로 들어왔는지 확인하자

![image](https://github.com/user-attachments/assets/30c23c5b-f47f-47c8-9bdc-616c605a215f)


[이메일 인증구현 영상](https://www.veed.io/view/435bbf4f-f323-4691-aaa3-68f31436144e?panel=share)

[Gmail SMTP를 활용한 이메일 인증 기능 개발 with Redis](https://hareandrabbit.tistory.com/entry/Spring-Gmail-SMTP%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-%EC%9D%B4%EB%A9%94%EC%9D%BC-%EC%9D%B8%EC%A6%9D-%EA%B8%B0%EB%8A%A5-%EA%B0%9C%EB%B0%9C-with-Redis)