---
layout: post
title:  "TIL(20240629) [QueryDSL연산자와 QueryDSL로 구현한 메서드 사용을 위한 Repository작업]"
date:  2024-06-29
categories: TIL QueryDSL JPA
---

---------------------------------------------------------------------

# 📌 QueryDSL 연산자 설명해보기

```
.select() -> 반환할 필드
.from() -> 엔티티 지정
.where() -> 조건 in(포함된 것들), eq(동일한 것들)
.orderBy() -> 정렬조건 
.offset() -> page * amount 해서 시작위치를 계산함 
.limit() -> 한 페이지에서 가져올 데이터 수 지정
.fetch() -> 반환
```

**Projections.constructor(DtoClass,entity);**
- QueryDSL을 사용하여 데이터베이스에서 조회한 결과를 DTO로 변환하여 사용할 수 있다.


##  **🆕 프로필에 내가 좋아요한 게시글 수/댓글 수 응답필드 추가하기**
    - 프로필 조회응답에 필드 추가
    - 프로필 조회시 응답필드에 내가 좋아요한 게시글 수 필드를 추가합니다.
    - 프로필 조회시 응답필드에 내가 좋아요한 댓글 수 필드를 추가합니다.

프로필 조회시 게시글 수, 댓글 수가 함께 보여야 하므로
userResponseDto의 응답필드 LikePostCount, LikeCommentCount 추가하고 생성자 한개 더 만들어주기

```java

@Getter
public class UserResponseDto {

  private String userId;
  private String userName;
  private String userEmail;
  private String description;
  private int likedPostCount;
  private int likedCommentCount;

  public UserResponseDto(User user) {
    this.userId = user.getUserId();
    this.userName = user.getUserName();
    this.userEmail = user.getUserEmail();
    this.description = user.getDescription();
  }

  public UserResponseDto(User user, int likedPostCount, int likedCommentCount) {
    this.userId = user.getUserId();
    this.userName = user.getUserName();
    this.userEmail = user.getUserEmail();
    this.description = user.getDescription();
    this.likedPostCount = likedPostCount;
    this.likedCommentCount = likedCommentCount;
  }
}

```

userService쪽으로 가서 count를 위한 QueryDSL로 구현된? 메서드를 만들어줘야 한다. 

```java

 // 사용자 프로필 조회
    public UserResponseDto getUser(Long userSeq) {
        User user = findByUserSeq(userSeq);
        int likedPostCount = likeRepository.getLikedPostCount(userSeq);
        int likedCommentCount = likeRepository.getLikedCommentCount(userSeq);
        return new UserResponseDto(user, likedPostCount, likedCommentCount);
    }

```

요로코롬 더해주고 LikeRespository 작업이 필요하다.
LikeRepository에서 추가로 LikeCustomRepository-LikeCustomeRepositoryImpl 파일을 만들어 주었다. 

LikeCustomRepository는 interface로 LikeRepository에서 extends하고 있다. LikeCustomRepositoryImpl은 LikeCustomRepository의 실제 구현체이므로 implements 해주고 있다. 


```java

@Repository
@RequiredArgsConstructor
public class LikeCustomRepositoryImpl implements LikeCustomRepository{

 private final JPAQueryFactory jpaQueryFactory;

  public int getLikedPostCount(Long userId) {
    QLike like = QLike.like;
    return (int) jpaQueryFactory
        .selectFrom(QLike.like)
        .where(QLike.like.userId.eq(userId).and(QLike.like.contentType.eq(LikeEnum.POST)))
        .fetchCount();
  }

  public int getLikedCommentCount(Long userId) {
    QLike like = QLike.like;
    return (int) jpaQueryFactory
        .selectFrom(QLike.like)
        .where(QLike.like.userId.eq(userId).and(QLike.like.contentType.eq(LikeEnum.COMMENT)))
        .fetchCount();
  }

}
```

```java

public interface LikeCustomRepository {

  int getLikesCount(Long contentId, LikeEnum contentType);
  List<Long> getLikedPostIds(Long userId);
  List<Long> getLikedPublicPostIds(Long userId);
  int getLikedPostCount(Long userId);
  int getLikedCommentCount(Long userId);
}

```

진행과정을 설명하자면

1. Dto에 응답필드를 추가하고
2. Repository 작업을 하고 (2개 추가 구현체와 interface)
3. QueryDSL로 구현한 메서드를 넣어주고
4. userService에서 해당 메서드를 추가만 해주면 끝!

```java
public int getLikedPostCount(Long userId) {
    QLike like = QLike.like;
    return (int) jpaQueryFactory
        .selectFrom(QLike.like)
        .where(QLike.like.userId.eq(userId).and(QLike.like.contentType.eq(LikeEnum.POST)))
        .fetchCount();
  }

```

해당 메서드 대해서 설명을 해보자면
QueryDSL 사용시 Qclass로 entity에 접근하기에(우회한다?라는 느낌) like변수를 만들어주고 .fetchCount();의 반환 타입이 'long'이기에 내가 응답필드에 추가한 타입은 'int'이므로 형변환을 해준다.

```java

// like entity 테이블에서 
.selectFrom(QLike.like)
// userId가 같고
.where(QLike.like.userId.eq(userId)
// "그리고" contentType이 Post와 같은것의
.and(QLike.like.contentType.eq(LikeEnum.POST)))
.fetchCount(); // 개수를 가져오너라... 

```

### 💡 eq와 in의 차이점

- eq 연산자: 특정 필드의 값이 주어진 값과 **정확히 일치**하는 경우를 검색한다. 
- in 연산자: 특정 필드의 값이 주어진 값들 중 **하나와 일치**하는 경우를 검색한다. 예를 들어, userId.in(userIdList)는 userId가 userIdList 안에 포함된 값들 중 하나와 일치하는 경우를 찾는다.
