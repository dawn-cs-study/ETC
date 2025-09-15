
### Code Study는 개발자들의 지식 공유와 면접 준비를 돕는 AI 기반 CS 학습·리뷰 플랫폼입니다.
- **URL : https://codestudy.org/**
- [content-repo](https://github.com/dawn-cs-study/cs-study-content)
- [lambda-repo](https://github.com/dawn-cs-study/cs-study-lambda)
- [ai-repo](https://github.com/dawn-cs-study/cs-study-ai)
- [api-repo](https://github.com/dawn-cs-study/cs-study-api)


### 구성원
| <a href="https://github.com/masiljangajji"><img src="https://github.com/masiljangajji.png" width="100px"><br>이승재</a> 
|-----|



---

### 기술 스택

개발도구

- Intellij IDEA - Ultimate

개발

- Java 24
- Spring Boot 3.5.5
- Spring Data JPA
- Spring Retry
- Spring AI
- FlexMark (Markdown)
- Gradle

데이터베이스

- PostgreSQL(PGVector)

AWS

- S3
- CloudFront
- RDS

CI/CD

- GitHub Actions

테스트

- JMH

품질 관리

- CodeRabbit AI

---

## **아키텍처**

<img width="977" height="553" alt="스크린샷 2025-09-16 06 41 23" src="https://github.com/user-attachments/assets/814a5732-030d-4242-8738-37bbf5cfc6d4" />


---

## 기여 내용

- **S3 Event 기반 자동 문서 처리 파이프라인**
    - **S3 Event → Lambda 트리거** 구조 구현, 새로운 파일 업로드 시 Lambda 자동 실행
    - **Markdown 파일을 불러와 텍스트로 변환** 후 **PGVector(PostgreSQL)** 에 임베딩
    - 변환된 문서 **HTML 빌드 및 CDN 서빙**
- **Lambda 최적화 및 비동기 처리 성능 개선**
    - **Virtual Thread** 기반 비동기 처리로 성능 최적화
    - **동기 처리, 고정 크기 ExecutorService(10/100/200), Virtual Thread(JDK 21/24)** **JMH 기반 성능 테스트**
    - Lambda 전체 재시도 특성을 고려 **실패 이벤트 개별 수집 후 보상**
    - [**동기에서 비동기로, 그리고 최적화까지 - Lambda 성능 실험기**](https://masiljangajji-coding.tistory.com/101)
- **파일 업로드 메모리 최적화**
    - **PDF 업로드 시 메모리 직적재 대신 임시 파일에 저장 후 스트리밍 방식 처리**하여 **메모리 사용량 최적화**
        - **Artillery 기반 부하 테스트**로 파일 업로드 처리량 및 안정성 검증
- **AI 검색 증강(RAG) 챗봇 구현 (Spring AI 기반)**
    - **GPT 모델 + PGVector 결합**으로 맥락 기반 **질의응답(FAQ/추천) 기능 제공**
- **CodeRabbit AI 기반 코드리뷰 도입**
    - 동료 리뷰가 없을 때 발생할 수 있는 **품질 저하 및 자기 확증 편향** 문제 완화
