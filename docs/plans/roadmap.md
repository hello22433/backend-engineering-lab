# Backend Engineering Lab — 학습 및 개발 로드맵

- 작성일: 2026-11-19
- 상태: 계획 (코드 구현 전)
- 시간 가정: 주당 6~8시간. 기간은 추정이며 진도에 따라 조정한다.

## 최종 목표
Java/Spring 백엔드의 원리, 설계, 테스트, 운영 역량을 체계적으로 축적하고 재사용 가능한 개인 기술 자산으로 만든다. 단순한 모듈 개수보다 설계 설명·장애 분석·검증 능력을 우선한다.

## 모듈 순서
| 단계 | 모듈 | 학습 핵심 | 산출물 |
|---|---|---|---|
| 00 | Java 실행·빌드 원리 | javac, JVM, classpath, Maven/Gradle/Bazel 비교 | [Module 00 상세 계획](module-00-java-build-fundamentals.md) |
| 01 | HTTP Server | TCP, HTTP/1.1, 소켓, Servlet | 학습용 HTTP 서버, Spring API 템플릿 |
| 02 | Authentication | 쿠키, 세션, 암호 저장, CSRF | Spring Security 인증 예제 |
| 03 | Authorization | RBAC, 최소 권한, 접근 제어 | 권한 정책 예제 |
| 04 | Database | SQL, JDBC, 인덱스, ORM | 데이터 접근 예제 |
| 05 | Transaction | ACID, 격리 수준, 잠금, 데드락 | 정합성 검증 실험 |
| 06 | Cache | TTL, 캐시 일관성, 장애 | 캐시 적용 패턴 |
| 07 | Idempotency | 중복 요청, 경쟁 상태 | 멱등성 처리 모듈 |
| 08 | Messaging | ACK, 재시도, DLQ, 파티션, 오프셋 | RabbitMQ/Kafka 예제 |
| 09 | Observability | 로그, 메트릭, 트레이스 | 관측성 템플릿 |
| 10 | Deployment | Linux, Docker, CI/CD | 배포 자동화 예제 |

## 진행 순서 변경
먼저 [Module 00](module-00-java-build-fundamentals.md)에서 JDK 도구와 빌드 시스템을 학습한 후 HTTP Server로 진행한다. 빌드 도구 결정은 [ADR-0002](../decisions/0002-build-system.md)를 따른다.

## 모듈별 표준 절차
1. **PLAN**: 문제, 범위, 성공·실패 조건 정의.
2. **RESEARCH**: CS 원리와 공식 레퍼런스 조사; 출처 기록.
3. **DESIGN**: 설계안, 대안, 트레이드오프, 위협/장애 시나리오 작성.
4. **LEARNING**: 원리 확인을 위한 최소 기능 직접 구현. 운영 사용 금지.
5. **PRODUCTION**: 검증된 프레임워크로 재사용 가능한 구성 작성.
6. **VERIFY**: 단위·통합·동시성·보안·성능 테스트(해당 시).
7. **REVIEW**: AI 리뷰 + 스스로 설계 설명 및 요구사항 변경 실습.
8. **RELEASE**: README, ADR, 테스트 결과, 버전·예제 기록.

## AI 사용 원칙
- 설계와 실패 조건은 먼저 직접 생각한다.
- 학습용 핵심 코드는 직접 작성하고 AI에는 힌트·리뷰를 우선 요청한다.
- 반복 코드 및 실전 구현에는 AI를 적극 활용하되 검증은 직접 책임진다.
- 공식 문서·실험으로 AI의 기술적 주장을 확인한다.

## 첫 스프린트: HTTP Server (예상 2주)
### 1주 차
- HTTP 요청/응답, TCP 연결, 소켓 학습
- Java ServerSocket 기반 최소 HTTP 요청 처리 구현
- GET, 요청 라인/헤더 파싱, 응답 작성
- 잘못된 요청, 타임아웃, 연결 종료 동작 테스트

### 2주 차
- Servlet, DispatcherServlet 요청 흐름 조사
- Spring MVC 구현과 학습용 서버 비교
- JUnit 테스트, README, 설계 결정 및 제한 사항 기록
- HTTP/1.1 전체 호환이나 운영 투입을 목표로 하지 않음

## 완료 기준 (Definition of Done)
- CS 원리 및 선택 이유를 AI 도움 없이 설명할 수 있다.
- 최소 핵심 동작을 직접 구현했다.
- 공식 레퍼런스와 실제 구현을 비교했다.
- 자동화 테스트 및 주요 실패 시나리오를 검증했다.
- 실전용 코드 또는 템플릿의 재사용 예제가 있다.
- 문서, ADR, 보안·운영 제한 사항이 기록되어 있다.

## 취업 대비
- 자료구조·알고리즘과 코딩테스트를 주당 1~2시간 병행한다.
- 회사의 소스·비밀·고객 데이터는 복사하지 않고 일반화된 개인 예제로 학습한다.

## 기록 위치
- `docs/decisions/`: 기술 결정(ADR)
- `docs/plans/`: 로드맵·스프린트
- `learning/`: 원리 학습용 코드
- `libraries/`: 실전용 재사용 라이브러리
- `applications/`: 통합 예제
- `benchmarks/`: 검증 결과
