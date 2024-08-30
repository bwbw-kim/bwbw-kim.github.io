---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

## 😊 자기소개 

---

저는 삼성 전자 DS 소프트웨어 연구소의 소프트웨어 엔지니어링 Lab에서 근무하고 있습니다.

- 회사 내 소프트웨어에 날개를 달아주는 서비스를 개발하는 것이 제 업무입니다.
- GitHub와 Bitbucket의 PR 코드 리뷰 자동화 서비스 개발을 통해 개발 생산성을 향상시켰습니다.
- 부서 내에서 사용 가능하도록 ReviewSystem 공용 API 개발을 담당했습니다.
- Java 단위 테스트 강화를 위한 서비스 개발에 참여하여 소프트웨어 품질 향상에 기여했습니다.
- 현재는 사내 SW Q&A 플랫폼 (DSDN, 회원수 5만명 이상)의 개발 및 운영을 책임지고 있습니다.

---

## ⭐ 진행한 프로젝트 소개 

---

#### 사내 SW Q&A 플랫폼 개발 및 운영 (2022-10 ~ 현재)
현재는 회원수 50000명 이상의 사내 Q&A 플랫폼 개발 및 운영을 책임지고 있습니다.

- 사내 SW Q&A 플랫폼 개발 및 업데이트 (1000+ commit)
- 사내 SW Q&A 플랫폼 개발 및 첫번째 버전 1.0 release
- 댓글속 댓글 기능 추가
- 알림 기능 추가
- 일별 인기게시물, 인기태그 기능 추가
- 게시판 Grid View UI 기능 추가
- 확장성을 고려한 게시판 추가 및 삭제 Admin 기능 추가
- User 성취감 고취를 위한 평판 시스템, Custom Badge 기능 개발 및 유지 보수
- Web 성능 개선을 위한 병렬 처리 개선
- Mysql indexi 설정을 통한 개선
- 검색 기능 Elastic Search 사용하여 고도화
- WebSocket을 사용하여 실시간 채팅 시스템 개발
- Ehcache를 사용하여 성능 개선 (5초 → 1초 이내)
- SSE를 사용하여 브라우저 알림 기능 추가
- 사내 SSO 서비스와 연결
- 중국,인도 법인 오픈을 위한 번역기능 (전체 번역, 일부 번역, 원문 돌아가기) 추가

기술 스택: Springboot, Javascript, Mysql, Elastic Search

---

#### Java Unit Test 지원 (2022-05 ~ 2022-10)
JAVA 프로젝트 대상 Unit Test 개발 및 지원에 참여하였습니다.

- 사내 20여개 Java 프로젝트 대상으로 Sample Junit TC 개발 및 배포
- 오픈소스 활용 사내 Java 프로젝트 대상 Test Case와 Teset대상 Method 추출 및 Elastic Search 활용 TC 추천 Web 서비스 개발
- Pull Request 발생시 Unit Test 자동 실행 CI 환경 구축과 Coverage Report 발행 시스템 개발

기술 스택: JAVA, Junit, Mockito, Springboot, Elastic Search, Jenkins, Python

---

#### Review System 공용 API 개발 (2022-01 ~ 2022-05)
Review System (Github, Bitbucket, Crucible 등)의 Event와 RestApi 형식 통일을 위한 공용 API 개발에 참여했습니다.

- Review System의 Event 통일화 및 Event Subscription 서비스 개발
- Review System의 Comments, Approve 등의 통일화된 공용 API개발

기술 스택: springboot, MongoDB, Django

---

#### 코드리뷰 자동화 시스템 (2021-02 ~ 2022-01)
사내 코딩 스타일 통일화를 위해 PR을 자동으로 리뷰해주는 코드리뷰 자동화 시스템 개발에 참여하였습니다.

- Jenkins의 shell script와 groovy script를 활용하여 Github과 Bitbucket의 Pull Requset 이벤트가 발생 시 자동으로 코드 리뷰를 수행할 수 있도록 환경 구축
- 코드리뷰 자동화 시스템의 안정된 운영을 위한 모니터링 시스템 구축 및 웹 대시보드를 개발
- 코딩스타일 체커 오픈소스인 Cpplint와 Cppcheck의 False Positive를 개선
- Jenkins와 Docker를 활용한 시스템 Deploy 환경 개선
- 안정된 시스템 운영을 위한 Python Unittest 개발 및 CI환경 구축
- Pull Request 대상 파일 코딩스타일 자동 교정 도구 Web 애플리케이션 개발

기술 스택: Jenkins, groovy script, Django, Github RestApi, MongoDB, Python, C++

---
