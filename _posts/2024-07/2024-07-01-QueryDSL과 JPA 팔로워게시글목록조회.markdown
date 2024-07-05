---
layout: post
title:  "TIL(20240701) [팔로워 게시글 목록 조회 + QueryDSL + Repository작업]"
date:  2024-07-01
categories: TIL QueryDSL JPA
---

---------------------------------------------------------------------


# 📌 팔로워 게시글 목록 조회 기능을 QueryDSL로 구현해보기

- **팔로워 게시글 목록 조회기능 추가**
    - 자신이 팔로우 하고 있는 유저들의 게시글만 목록으로 조회 할 수 있습니다.
    - 응답정보는 기존 게시글 목록 조회기능 응답정보와 동일합니다.
    - 기본 정렬은 **생성일자 기준으로 최신순**으로 정렬합니다.
    - 페이지네이션
    - 페이지네이션하여 각 페이지 당 게시물 데이터가 5개씩 나오게 합니다.

- **팔로워 게시글 목록 조회 기능에 정렬 기준 추가**
    - 팔로워 게시글 목록 조회 기능에 **작성자명 기준** 정렬기능을 추가합니다.
    - 응답정보는 기존 게시글 목록 조회기능 응답정보와 동일합니다.
    - 페이지네이션
    - 페이지네이션하여 각 페이지 당 게시물 데이터가 5개씩 나오게 합니다.


응답정보는 아래 Dto에서 확인할 수 있다.

```java

@Getter
public class PublicPostResponseDto {
    String title;
    String content;
    String userName;
    LocalDateTime createdAt;
    LocalDateTime modifiedAt;
    int likeCount;

    public PublicPostResponseDto(PublicPost post, int likeCount) {
        this.title = post.getTitle();
        this.content = post.getContent();
        this.userName = post.getUser().getUserName();
        this.createdAt = post.getCreatedAt();
        this.modifiedAt = post.getModifiedAt();
        this.likeCount = likeCount;
    }

    public PublicPostResponseDto(PublicPost post) {
        this.title = post.getTitle();
        this.content = post.getContent();
        this.userName = post.getUser().getUserName();
        this.createdAt = post.getCreatedAt();
        this.modifiedAt = post.getModifiedAt();
        this.likeCount = 0;
    }

}

```

Controller 쪽에는 API를 생성일자 정렬과 작성자명 정렬 2개를 만들어주었다.

```java

@Operation(summary = "팔로우한 사람들의 공개글 전체 조회(생성일자 정렬)", description = "내가 팔로우한 사람들의 글을 전체 조회(생성일자 기준, 한페이지당 5개씩 조회)")
  @GetMapping("/following/created")
  public ResponseEntity<List<PublicPostResponseDto>> getFollowedPublicPostsOrderByCreatedAt(
      @AuthenticationPrincipal UserDetailsImpl userDetails, @RequestParam("page") int page,
      @RequestParam(value = "size", defaultValue = "5") int pageSize) {
    return ResponseEntity.status(HttpStatus.OK)
        .body(publicPostService.getFollowedPublicPostsOrderByCreatedAt(
            userDetails.getUser().getUserSeq(), page - 1, pageSize));
  }

  @Operation(summary = "팔로우한 사람들의 공개글 전체 조회(작성자명 정렬)", description = "내가 팔로우한 사람들의 글을 전체 조회합니다.(작성자명 기준, 한페이지당 5개씩 조회)")
  @GetMapping("/following/username")
  public ResponseEntity<List<PublicPostResponseDto>> getFollowedPublicPostsOrderByUsername(
      @AuthenticationPrincipal UserDetailsImpl userDetails, @RequestParam("page") int page,
      @RequestParam(value = "size", defaultValue = "5") int pageSize) {
    return ResponseEntity.status(HttpStatus.OK)
        .body(publicPostService.getFollowedPublicPostsOrderByUsername(
            userDetails.getUser().getUserSeq(), page - 1, pageSize));
  }


```

메서드명이 엄청나게 길어졌다.. orderby를 통해 어떤 정렬기준인지 알 수 있도록 구분을 해놓았다.

service 부분

```java

// 팔로워한 사람들의 비익명게시물 목록 조회(생성일자 기준 정렬)
  @Transactional(readOnly = true)
  public List<PublicPostResponseDto> getFollowedPublicPostsOrderByCreatedAt(Long userId, int page, int pageSize) {

    List<Long> followedUserIds = followRepository.findFollowedUserIds(userId);
    List<PublicPost> publicPostList = publicPostRepository.findByPublicPostIdOrderByCreatedAt(page, pageSize, followedUserIds);

    if(publicPostList.isEmpty()) {
      throw new CustomException(ErrorCode.EMPTY_FOLLOW);
    }

    List<PublicPostResponseDto> publicPostResponseDtos = new ArrayList<>();
    for(PublicPost publicPost : publicPostList) {
        int likedCount = likeRepository.getLikesCount(publicPost.getId(), LikeEnum.PUBPOST);
        PublicPostResponseDto responseDto = new PublicPostResponseDto(publicPost, likedCount);
        publicPostResponseDtos.add(responseDto);
    }

    return publicPostResponseDtos;
  }

  // 팔로워한 사람들의 비익명게시물 목록 조회(작성자명 기준 오름차순 정렬)
  @Transactional(readOnly = true)
  public List<PublicPostResponseDto> getFollowedPublicPostsOrderByUsername(Long userId, int page, int pageSize) {
    Pageable pageable = PageRequest.of(page, pageSize);

    List<Long> followedUserIds = followRepository.findFollowedUserIds(userId);

    List<PublicPost> publicPostList = publicPostRepository.findByPublicPostIdOrderByUsername(page, pageSize, followedUserIds);

    if(publicPostList.isEmpty()) {
      throw new CustomException(ErrorCode.EMPTY_FOLLOW);
    }

    List<PublicPostResponseDto> publicPostResponseDtos = new ArrayList<>();
    for(PublicPost publicPost : publicPostList) {
      int likedCount = likeRepository.getLikesCount(publicPost.getId(), LikeEnum.PUBPOST);
      PublicPostResponseDto responseDto = new PublicPostResponseDto(publicPost, likedCount);
      publicPostResponseDtos.add(responseDto);
    }

    return publicPostResponseDtos;
  }

```

```java
List<Long> followedUserIds = followRepository.findFollowedUserIds(userId);
List<PublicPost> publicPostList = publicPostRepository.findByPublicPostIdOrderByCreatedAt(page, pageSize, followedUserIds); 
```

위 두 구문의 쿼리문은 QueryDSL로 구현해보았는데,
해당 쿼리문을 Service에서 구현할 경우 엄청나게 길어지는 코드를 확인할 수 있었다. 그래서 관심사 분리가 필요하다고 생각했고 repository에서 해당 로직을 처리하도록 하기 위해 

PublicPostCustomRepository(interface)-PublicPostCustomRepositoryImpl(실제 구현체)를 생성해주었다.


```java

public interface PublicPostCustomRepository {

  List<PublicPost> findByPublicPostIdOrderByCreatedAt(int page, int pageSize, List<Long> followedUserIds);
  List<PublicPost> findByPublicPostIdOrderByUsername(int page, int pageSize, List<Long> followedUserIds);
}

```

```java

// interface PublicPostCustomRepository 실제 구현체 Impl 

 public List<PublicPost> findByPublicPostIdOrderByCreatedAt(int page, int pageSize, List<Long> followedUserIds) {
    Pageable pageable = PageRequest.of(page, pageSize);
    QPublicPost qPublicPost = QPublicPost.publicPost;

      List<PublicPost> publicPostList = jpaQueryFactory
          .selectFrom(publicPost)
          .where(publicPost.user.userSeq.in(followedUserIds))
          .orderBy(publicPost.createdAt.desc())
          .offset(pageable.getOffset())
          .limit(pageable.getPageSize())
          .fetch();
      return publicPostList;

  }
  public List<PublicPost> findByPublicPostIdOrderByUsername(int page, int pageSize, List<Long> followedUserIds) {
    Pageable pageable = PageRequest.of(page, pageSize);
    QPublicPost qPublicPost = QPublicPost.publicPost;

      List<PublicPost> publicPostList = jpaQueryFactory
          .selectFrom(publicPost)
          .where(publicPost.user.userSeq.in(followedUserIds))
          .orderBy(user.userName.asc())
          .offset(pageable.getOffset())
          .limit(pageable.getPageSize())
          .fetch();
      return publicPostList;

  }

```

jpa와 QueryDSL 사용하여 구현해보니 장단점이 존재하는 것 같다. 간단히 내가 느낀점을 말해보자면

- JPA 장점 <br>
    1. 간단한 CRUD작업을 빠르게 처리할 수 있다.
    2. 개발자가 직접 JPQL문을 따로 작성하지 않아도 JPQL문을 자동화하여 처리한다. 
    3. 개발자가 직접 객체를 다루듯이 데이터를 다룰 수 있다.

- JPA 단점<br>
    1. 정렬이나 요구사항이 많아질 경우 쿼리문이 길어지기 때문에 가독성이 떨어지기도 하고 제한적이다. 그리고 잘못된 쿼리작성 시 오류를 찾기가 어렵다.

- QueryDSL 장점<br>
    1. JPQL과 같이 문자열이 아닌 코드이기에 컴파일 시점에서 오타 등 빠르게 찾아낸다. 
    2. 복잡한 동적쿼리를 작성할 수 있다. 연관관계가 없는 엔티티도 join을 통해서 만들어낼 수 있다.
    3. 직관적으로 SQL문처럼 데이터를 어떻게 가져올건지에 대해서 명확히 눈에 보이기 때문에 가독성이 좋지만, 문제는 진짜 코드 길이가 어마무시하게 길어진다.

상황에 따라서 두 가지를 적용하여 사용하면 좋을 것 같다. 
근데 QueryDSL 의 Q클래스 사용은 참.. 뭔가 살짝 번거롭긴 하다... 

