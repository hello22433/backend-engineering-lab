# ADR-0001: 백엔드 학습 기술 스택 선정

- 상태: Amended by [ADR-0002](0002-build-system.md) for build tools; 나머지 항목 유지
- 작성일: 2026-11-19

## 의사결정 기준
1. 현재 Java/Spring 실무와 연결되는가?
2. CS 원리와 시스템 설계를 깊게 배울 수 있는가?
3. 이직 시 설명 가능한 실무 역량으로 이어지는가?
4. 향후 다른 프로젝트에서 재사용할 수 있는가?
5. 운영 복잡성과 학습 비용이 적절한가?

## 결정
| 영역 | 선택 | 근거 | 비교·유보 |
|---|---|---|---|
| 언어 | Java 21 | JVM, 메모리 모델, 동시성, 현재 실무 연결 | Go/Kotlin은 후속 비교 |
| 웹 | Spring Boot (호환 버전 고정) | Servlet, DI, 트랜잭션, 보안 생태계 | Quarkus/Micronaut는 필요 시 |
| 빌드 | 학습: JDK CLI → Maven/Gradle/Bazel 비교; 개인 실전: Gradle Kotlin DSL 우선 | 빌드 원리 학습·멀티모듈 자산화 | 회사 실무는 Maven 유지; [ADR-0002](0002-build-system.md)에서 결정 변경 근거 기록 |
| DB | MySQL 8.4 | SQL, InnoDB 잠금·격리·실행 계획, 실무 연계 | PostgreSQL 후속 비교 |
| 데이터 접근 | JDBC → JPA/Hibernate | 추상화 전후 비용·동작 비교 | MyBatis/jOOQ |
| 테스트 | JUnit, Mockito, Testcontainers | 단위 및 실제 인프라 통합 검증 | 인메모리 DB만으로 대체하지 않음 |
| 캐시 | 로컬 캐시 → Redis | 캐시 일관성·TTL·장애 학습 | 초기부터 Redis 강제하지 않음 |
| 메시징 | RabbitMQ → Kafka | 작업 큐와 이벤트 로그의 차이 학습 | 요구사항 없이 동시 도입 금지 |
| 인프라 | Linux, Docker Compose | 프로세스·네트워크·재현 가능한 환경 | Kubernetes 후순위 |
| CI | GitHub Actions | 저장소와 자동 테스트 연계 | Jenkins 비교 |
| 관측성 | Micrometer → Prometheus/Grafana | 메트릭 기반 병목·장애 분석 | 도입 전 측정 목표 정의 |

## 단계별 도입 원칙
- 1단계: Java, HTTP/TCP, Linux, Maven, JUnit, Git.
- 2단계: Spring MVC/Security, MySQL, JDBC/JPA.
- 3단계: Redis, 트랜잭션, 멱등성.
- 4단계: RabbitMQ/Kafka, 장애 복구.
- 5단계: 관측성, CI/CD, 배포.
- 정확한 라이브러리 버전과 호환성은 각 모듈 착수 시 공식 문서로 재확인한다.

## 트레이드오프
- Java/Spring 집중은 단기 깊이를 높이지만 언어 다양성은 늦춘다.
- MySQL 집중은 실무 전이성이 높지만 PostgreSQL의 차이를 별도로 배워야 한다.
- 학습용 직접 구현은 이해에 유리하지만 운영용 재사용은 검증된 구현을 우선한다.
- 많은 도구를 한 번에 도입하면 장애 원인 분리와 학습 집중도가 떨어진다.

## 공식 레퍼런스
- Java 21: https://docs.oracle.com/en/java/javase/21/
- Java Memory Model: https://docs.oracle.com/javase/specs/jls/se21/html/jls-17.html
- Spring Framework: https://docs.spring.io/spring-framework/reference/
- Spring Boot: https://docs.spring.io/spring-boot/reference/
- MySQL InnoDB: https://dev.mysql.com/doc/refman/8.4/en/innodb-storage-engine.html
- PostgreSQL: https://www.postgresql.org/docs/
- JUnit: https://junit.org/junit5/docs/current/user-guide/
- Testcontainers: https://java.testcontainers.org/
- Redis: https://redis.io/docs/latest/
- RabbitMQ: https://www.rabbitmq.com/docs
- Kafka: https://kafka.apache.org/documentation/
- Docker Compose: https://docs.docker.com/compose/
- GitHub Actions: https://docs.github.com/en/actions
- OWASP Cheat Sheets: https://cheatsheetseries.owasp.org/

> 위 링크는 검토할 공식 1차 자료이며, 실제 벤치마크나 시장 점유율의 증거를 뜻하지 않는다. 각 모듈에서 구체적 절과 실험 결과를 추가한다.
