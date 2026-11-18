# ADR-0002 — 빌드 시스템: 바닥부터 비교한 뒤 Gradle Kotlin DSL 채택

- 작성: 2026-11-19
- 상태: Accepted for learning; Gradle is provisional for production modules
- 선행: [ADR-0001](0001-tech-stack.md)
- 변경 배경: Maven을 실무 연계성 때문에 선택했으나, 학습 목표가 빌드 원리 이해 및 멀티모듈 자산화로 확장됨.

## 결정 요약
1. **학습용 최소 Java 프로그램**: JDK 도구 `javac`, `java`, `jar`, `javap`로 직접 컴파일·실행·패키징.
2. **비교 실험**: 동일 소스를 Maven, Gradle Kotlin DSL, Bazel로 빌드하여 차이를 측정.
3. **실전용 라이브러리/애플리케이션**: Gradle Kotlin DSL을 우선 후보로 채택. 실제 구현 시작 시 지원 버전·플러그인 호환성 확인.
4. **회사 실무**: Maven을 계속 사용하고 Maven의 라이프사이클과 의존성 해결을 깊게 이해.
5. **Bazel**: 대규모 코드베이스·빌드 재현성 문제를 학습하는 비교 대상이지 초기 기본 도구는 아님.

## 비교
| 항목 | JDK CLI | Maven | Gradle | Bazel |
|---|---|---|---|---|
| 핵심 추상화 | 컴파일러·런타임·아카이브 | 선언적 POM, phase/goal | task graph, plugins, configurations | target, action graph, hermeticity |
| 외부 의존성 | 수동 다운로드/classpath | 저장소·전이 의존성 | 저장소·전이 의존성 | 규칙·외부 저장소 설정 |
| 학습 가치 | 컴파일/실행의 실체 | 표준 빌드 생명주기 | 증분·캐시·멀티프로젝트 | 재현성·원격 캐시·규모 |
| 단점 | 규모가 커지면 수동 작업 증가 | 맞춤 빌드 로직은 플러그인 중심 | 유연성만큼 설정 복잡도 | 소규모에 설정 부담 |
| 선택 | Module 00 필수 | 비교 필수 | 개인 실전 기본 후보 | 선택 비교 |

## 중요한 정확성
- Spring Boot는 빌드 도구의 '상위 계층'이라기보다 애플리케이션 프레임워크다. 빌드 도구는 소스를 컴파일하고 패키징하는 별도 책임을 맡는다.
- `javac`는 바이트코드를 생성하고 `java`는 JVM을 구동한다. 클래스 로딩·JIT 등은 JVM 런타임의 책임이다.
- Gradle이 **항상** Maven보다 빠른 것은 아니다. cold/warm build, 캐시 적중, 테스트 규모, 머신 환경을 분리해 비교한다.
- Bazel은 Google의 내부 빌드 시스템과 역사적 연관이 있지만, 'Google의 모든 서비스가 Bazel을 사용한다'는 식으로 일반화하지 않는다.
- Netflix의 공개 빌드 사례 등은 특정 팀/시기의 사례이며 기업 전체 표준으로 단정하지 않는다.

## 실험 설계
동일한 Java 소스·JDK·의존성·테스트·머신에서 아래를 비교한다.

1. **기능 동등성**: 빌드 산출물 실행, 테스트 통과, 의존성 버전 확인.
2. **Clean build**: 캐시/산출물을 제거한 뒤 5회 반복.
3. **No-op build**: 소스 변경 없이 5회 반복.
4. **Incremental build**: Java 파일 한 개 수정 후 5회 반복.
5. **멀티모듈**: 공통 라이브러리 + API + 테스트 모듈에서 변경 영향 관찰.
6. **재현성**: 깨끗한 환경에서 동일 버전으로 빌드, 산출물 해시 비교 시 타임스탬프 등 비결정적 메타데이터 통제.
7. **개발자 경험**: 설정량, 디버깅 난도, IDE 지원, 의존성 충돌 해결.
8. 결과는 실행 환경(JDK/OS/CPU/RAM/빌드 도구 버전), 명령, 반복별 시간, 중앙값, 제한점을 포함해 `benchmarks/build-tools/`에 기록한다.

## 전환/재검토 조건
- Gradle 설정 유지비가 실제로 과도하거나 조직·배포 요구에 Maven이 유리하면 Maven 채택을 재검토.
- 모듈이 커지고 빌드 재현성·원격 캐시·다중 언어 빌드가 중요해질 때 Bazel 실험 확대.
- 결정 변경 시 기존 ADR을 삭제하지 않고 새 ADR로 대체한다.

## 공식 1차 자료
- Java JDK tools: https://docs.oracle.com/en/java/javase/21/docs/specs/man/
- JVM specification: https://docs.oracle.com/javase/specs/jvms/se21/html/
- Maven lifecycle: https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html
- Maven dependency mechanism: https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html
- Gradle multi-project: https://docs.gradle.org/current/userguide/multi_project_builds.html
- Gradle build cache: https://docs.gradle.org/current/userguide/build_cache.html
- Gradle Kotlin DSL: https://docs.gradle.org/current/userguide/kotlin_dsl.html
- Bazel introduction: https://bazel.build/about/intro
- Bazel Java tutorial: https://bazel.build/start/java
