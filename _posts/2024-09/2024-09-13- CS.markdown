---
layout: post
title:  "TIL(20240903) [새로운 프로젝트: mixin 회원가입]"
date:  2024-09-03
categories: TIL
---

----------------------------------------------------------------------------

다시 마음을 다 잡고 마음이 맞는 분들이랑 새로운 프로젝트를 기획하게 되었다.

프로젝트 이름은 **Mixin** 

대학생들이 자신의 관심사와 열정을 중심으로 모임을 만들어 활동할 수 있도록 지원하는 소모임과 같은 형태의 앱 어플리케이션이다.

이때까지 웹 어플리케이션만 만들다가 처음으로 앱을 만들어보는 기회가 생겨서 기대된다.

일단 내가 맡은 부분은 "유저" 부분인데, 처음으로 Spring security를 도전해보게 될 것 같다. 과제로만 했었는데 프로젝트를 직접 맡게 되어서 걱정이 되긴 하다만 열심히 해보려고 한다.


## 🚩 SignupRequestDto 엑세스 레벨을 둔다는 것은?

아직 내 실력으로 무에서 유를 창조하는 건 힘들다.
다른 프로젝트를 참고하던 도중에 SignupRequestDto에 `@AllArgsConstructor(access = AccessLevel.PRIVATE) `어노테이션을 사용한 부분이었다. 

### 💡 해당 어노테이션을 사용하면 좋은 이점은?

- 1. `SignupRequestDto`는 회원가입 요청 시 사용자로 부터 입력된 데이터를 서버로 전달하기 위해 사용되는 객체이다. 이 객체를 불변으로 만드는 것은 전달된 데이터가 이후 변경되지 않도록 보장함으로써 데이터의 신뢰성을 높일 수 있게 된다. -> final 사용시 해당

-  2. `SignupRequestDto`는 특정한 메서드를 통해서만 생성되도록 제한할 수 있다. 이렇게 하면 객체의 생성 방식을 통제할 수 있고, 잘못된 데이터가 입력되지 않도록 안정성을 보장할 수 있다. -> 외부에서 객체 생성 방식을 제어함 

따라서 `@AllArgsConstructor(access = AccessLevel.PRIVATE)` 어노테이션을 사용하게 되면 데이터 전달의 신뢰성과 안정성을 보장할 수 있지만 외부에서 객체 생성을 막기위해 정적 팩토리 메서드를 사용하여야 한다는 것 

```java
@Getter
@AllArgsConstructor(access = AccessLevel.PRIVATE)
public class UserDTO {
    private String username;
    private String email;

    // 정적 팩토리 메서드
    public static UserDTO of(String username, String email) {
        return new UserDTO(username, email);
    }
}
```

## 💡 정적 팩토리 메서드 of

- 클래스 외부에서 위와 같은 Dto 객체를 생성하려면 이 of 메서드를 사용해야 한다.
- of 메서드는 private 생성자를 호출하여 새로운 Dto 객체를 반환한다.






