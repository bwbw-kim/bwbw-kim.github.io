---
title: Spring JPA 사용하는 Service에서 Transactional 사용하기 (간단한 코드)
date: 2024-12-03
categories:
  - Spring & JAVA
tags:
  - jpa
  - transactional
render_with_liquid: true
mermaid: true
---
#### Transactional
---
사실 사내 SW Q&A 플랫폼을 개발하면서 중간에 에러가 나는 상황이 발생해서 디비에 절반의 쿼리만 적용이 되어도 문제가 없는 상황이 많아서였을까 transaction에 대한 고민을 많이 해보지 못했던거 같다. 하지만 점점 사람이 많은 시스템을 운영하거나 돈이 관련된 서비스를 운영하게 되면 이건 정말 무조건! 필요하다!

우선 transaction은 2개 이상의 쿼리를 Atomic하게 운영하기 위해 사용된다.

예를 들어서 우리 서비스의 예를 들면 추천을 하면 평판이 올라간다. 이런 비즈니스 로직은 `추천을 한다` 와 `평판이 올라간다` 이 2개가 하나처럼 진행이 되어야한다. 이럴때 2개의 로직을 하나의 트랙잭션안에 넣으므로서 atomic 하게 사용 할 수 있다

#### 가볍게 코드를 살펴보자
---
트랜잭션의 종류는 다양하다 그 종류는 다음 정리때 아주 자세하게 알아보는것으로 하고 오늘은 가볍게 코드를 살펴보고자 한다!

위의 `추천을 한다` 와 `평판이 올라간다` 를 하나로 묶어보면 아래와 같이 할 수 있다!

```java
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class PostService {

    private final PostRepository postRepository;
    private final UserRepository userRepository;

    public PostService(PostRepository postRepository, UserRepository userRepository) {
        this.postRepository = postRepository;
        this.userRepository = userRepository;
    }

    @Transactional
    public void increaseLikesAndReputation(Long postId, Long userId) {
        Post post = postRepository.findById(postId)
                .orElseThrow(() -> new IllegalArgumentException("Post not found"));
        post.seVotes(post.getVotes() + 1);
        postRepository.save(post);

        User user = userRepository.findById(userId)
                .orElseThrow(() -> new IllegalArgumentException("User not found")); // 만약 여기에서 에러가 났다면 post의 vote 도 올라가지 않는다!!!
        user.setReputation(user.getReputation() + 10);
        userRepository.save(user);
    }
}
```

이렇게 중요한 개념을 많이 고민하지 않았다니!

앞으로는 이 부분도 깊게 고민하면서 개발을 해나가야겠다!