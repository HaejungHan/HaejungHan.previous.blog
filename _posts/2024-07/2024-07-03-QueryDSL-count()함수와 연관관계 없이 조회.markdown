---
layout: post
title:  "TIL(20240703) [QueryDSL-count()함수, 연관관계 없는 엔티티 조회?]"
date:  2024-07-03
categories: TIL QueryDSL
---

---------------------------------------------------------------------


- **팔로워 TOP 10 목록 조회기능 추가**
    - 팔로워를 가장 많이 보유한 상위 10명의 프로필 정보 목록을 조회합니다.
    - 정렬 없이 10명의 프로필 정보가 모두 나오게 합니다.
    - 프로필 정보와 함께 몇명의 팔로워를 가지고 있는지 출력해줍니다.


위 해당 기능을 구현하기 위해서 QueryDSL로 구현과정에서
생긴 에러들에 대해서 알아보려고 한다. 


# 📌 QueryDSL - count()함수

위 기능을 구현하기 위해서는 총 3가지가 필요하다.<br>
첫번째 userResponseDto로 반환하기 위해 응답정보 넣어주기<br>
두번째 follow의 가장 많은 toUser 10개 - follower가 가장 많은 사람을 의미함<br>
세번째 toUser들의 id를 통해 각 유저들의 like엔티티에 있는 postlike, commentlike count가져오기(like엔티티는 연관관계 설정 무) <br>

```java

@Transactional(readOnly = true)
  public List<UserResponseDto> findTop10Followers() {
    QFollow follow = QFollow.follow;
    QUser user = QUser.user;
    QLike like = QLike.like;

    List<UserResponseDto> topFollowers = jpaQueryFactory
        .select(Projections.constructor(UserResponseDto.class,
            user.userId,
            user.userName,
            user.userEmail,
            user.description,
            like.userId.eq(user.userSeq).and(like.contentType.eq(LikeEnum.POST)).count().as("likedPostCount"),
            like.userId.eq(user.userSeq).and(like.contentType.eq(LikeEnum.COMMENT)).count().as("likedCommentCount"),
            follow.toUser.count().as("followCount")))
        .from(follow)
        .join(follow.toUser, user)
        .leftJoin(like).on(like.userId.eq(user.userSeq))
        .groupBy(user.userId, user.userName, user.userEmail, user.description)
        .orderBy(follow.toUser.count().desc())
        .limit(10)
        .fetch();
    return topFollowers;
  }


```

작성하고 postman에서 send를 요청하는 순간


```java

com.querydsl.core.types.ExpressionException: No constructor found for class com.sparta.heartvera.domain.user.dto.UserResponseDto with parameters: [class java.lang.String, class java.lang.String, class java.lang.String, class java.lang.String, class java.lang.Long, class java.lang.Long, class java.lang.Long]	at com.querydsl.core.util.ConstructorUtils.getConstructorParameters(ConstructorUtils.java:121)

```

다음과 같은 에러가 발생하였다.
대충 매개변수 타입이 잘못되어있다 라는 의미로 생각되서
Dto부분에서 int 타입으로 되어있는 필드들은 Long타입으로 바꾸어줄지 아니면 Query문에서 intValue()로 바꾸어줄지 고민했다.. 

QueryDSL에서 count()함수는 결과를 'Long'(Wrapper)타입으로 반환하기 때문에 int타입으로 반환해야 한다면 count().intValue() 를 넣어줘야 한다. 

개인적으로 그냥 구지 저것 때문에 wrapper클래스로 바꾸어 메모리를 비효율적으로 사용하지 말자(?)라는 생각이었다.


# 📌 QueryDSL - 연관관계가 없는 Like엔티티

그리고 또 직면한 문제.. userResponseDto의 응답정보가 


```java

public UserResponseDto(User user, int likedPostCount, int likedCommentCount) {
    this.userId = user.getUserId();
    this.userName = user.getUserName();
    this.userEmail = user.getUserEmail();
    this.description = user.getDescription();
    this.likedPostCount = likedPostCount;
    this.likedCommentCount = likedCommentCount;
  }

```

like의 엔티티가 현재 연관관계가 없기 때문에 dto생성자가 저렇게 나오게 되는데, 여기서 문제는 like엔티티에 연관관계를 만들어주면 되지만 과제 제출기한 및 도전정신(연관관계가 없다면 어떤일이 생기는가?)으로 인해서 이전에 like엔티티 자체에서 likecount를 조회하는 형식으로 해서 구현하였기에 follower top10 프로필 정보를 조회할 경우 상당히 복잡해져 버리는 것을 알 수 있었다. -> 그래서 연관관계가 필요하다...

🤦‍♀️TMI) 무엇이 상당히 복잡해지는가? 내가 처음 생각했던 건 이랬다. like엔티티에서 contentType이 post이고 follow to_user의 아이디와 동일한 갯수를 가져오면 구할 수 있겟지 생각했는데, 큰 착오였다. like엔티티에 있는 userid는 follow 입장에서 보면 fromUser와 같은 존재였기 때문에 저 카운트를 불러온다고 해서 toUser가 작성한 postLikecount를 가져오는게 아닌 like한 사람의 count를 가져오는 꼴이다.. 그래서 like엔티티에 있는 contentType은 post이고 contentid를 가져와서 post에서 다시 조회를 해야하는 상황이 발생하는 것이었다.. 결국에는 user,like,follow,post/comment가 다 묶여버리는 상황......내가 너무 복잡하게 생각한건지.. 다른 방법이 있는건지 더 고민을 해봐야 할 부분인 것 같다. 

필드를   private int likedPostCount; private int likedCommentCount; 두가지 가지기 때문에 userResponseDto로 반환할 경우 해당 필드에 대한 정보를 넣어줘야 해서.. 요구사항에 프로필 정보에 대해서 likeCount를 넣어야 구지 넣어야 하는가 라는 생각도 들었다.. 고민끝에 UserTop10ResponseDto를 만들어서 반환해주는 것으로 결정했다.

그래서 과감히 like조회하는 부분을 빼버렸다.. 빼버리는 순간까지도 likecount까지 조회하는 서비스가 있을까.. 라는 고민을 했다.

```java
@Transactional(readOnly = true)
  public List<UserTop10ResponseDto> findTop10Followers() {
    QFollow follow = QFollow.follow;
    QUser user = QUser.user;
    QLike like = QLike.like;

    List<UserTop10ResponseDto> topFollowers = jpaQueryFactory
        .select(Projections.constructor(UserTop10ResponseDto.class,
            user.userId,
            user.userName,
            user.userEmail,
            user.description,
            follow.toUser.count().intValue().as("followCount")))
        .from(follow)
        .leftJoin(follow.toUser, user)
        .groupBy(user.userId, user.userName, user.userEmail, user.description)
        .limit(10)
        .fetch();
    return topFollowers;
  }

```

orderby 정렬이 있었지만 요구사항에는 "정렬없이"라고 되어있어서 해당 코드도 뺐다. 그리고 뒤늦게 발견한 사실은 like랑 저렇게 묶어두게 되면 groupby때문에 like랑 엮여서
count가 중복되어서 나타난다는 사실도 알게되었다. 후후...

jpa메서드로 간편하게 해당 부분을 구현할 수도 있겠지만 QueryDSL로 직관적으로 어디서 데이터를 가져올건지 어떻게 가져와서 어떻게 가공해서 반환할건지에 대해 알 수 있어서 너무 좋았다. 그럼 동적쿼리를 작성하기 위해선 어떻게 할까?

 ** QueryDsl에 fetch 함수 중 fetchCount, fetchResult는 Deprecated 된 함수 지원이 중단된 함수는 사용하지 않는 것이 좋으며 fetchOne, fetchFirst 함수를 활용하여 해볼 것!