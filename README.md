
### Code Study는 개발자들의 지식 공유와 면접 준비를 돕는 AI 기반 CS 학습·리뷰 플랫폼입니다. 

**기간 : 2025/08/18 ~ 진행 중**  
**URL : https://codestudy.org/** 

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

마크다운 기반 **CS 지식 저장소/위키 플랫폼**입니다.

문서를 GitHub 저장소에 업로드하면 **S3 + Lambda + CloudFront** 아키텍처를 통해 자동으로 HTML로 변환·배포됩니다. (Lambda -> SQS 변경 예정)

추후 코드 실행(Runner API)와 AI 추천 기능을 연계하여, 학습자 중심의 실습형 CS 학습 환경을 제공합니다.

## 🚀 주요 기능

- **Markdown → HTML 변환**
    - GitHub `main` 브랜치에 새로운 `.md` 파일이 머지되면 S3 업로드 이벤트가 트리거
    - Lambda가 실행되어 Markdown을 파싱해 문자열로 변환 후 VectorDB 적재
    - 파싱된 문자열 HTML로 변환 후 S3/CloudFront에 배포
- **JSON → DB 적재**
    - `.json` 파일 업로드 시 DB에 자동 적재하여 메타데이터/설정 관리
- **CDN 캐싱**
    - CloudFront 캐싱을 활용해 빠른 문서 제공
    - 캐시 무효화/버전 관리 전략을 통한 최신 문서 반영
- **추천 글 기능 (추가 예정)**
    - 학습자가 읽은 문서와 관련된 다른 문서를 추천
    - AI 기반 연계 학습 경로 탐색 고려 중
- **Runner API (추가 예정)**
    - 코드 실행 기능을 제공하여 실습형 학습 지원
    - 예: 알고리즘 문제 풀이, 간단한 코드 검증

---

## 🌱 철학과 비전

이 서비스는 단순한 위키가 아니라, **개발자 "스타터팩"** 으로 받아들여졌으면 합니다.

기술 면접을 준비하다 보면 질문이 크게 두 가지로 나뉩니다

1. **일반적인 질문**
    - 예: *"JPA 영속성 컨텍스트에 대해 설명해주세요."*, *"@Transactional의 동작 방식을 설명해주세요."*
    - 이런 질문은 마치 **정보처리기사**나 **운전면허 시험**처럼, 문제은행 기반의 정형화된 패턴이 있습니다.
2. **포트폴리오 기반 심화 질문**
    - 예: *"포트폴리오에서 JWT 인증/인가 시스템을 구축했다고 하셨는데, 어떤 고민이 있었나요?"*
    - 이는 개인의 경험과 설계 의도에 기반한 확장형 질문입니다.

**일반적인 질문 영역**은 AI를 활용해 "문제은행 + 자동 답변" 시스템으로 더 효율적으로 학습할 수 있다고 생각했습니다.

따라서 본 프로젝트는 **문제은행 방식으로 질문을 만들고, RAG 기반으로 문서의 어느 부분(문단 단위)에 답이 있는지 추적**하는 기능을 구현하고 있습니다.

추가적으로, 가능하다면 **보이스 인터페이스(음성 질의·응답)** 까지 확장해 더욱 자연스러운 학습 경험을 제공할 계획입니다.


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
