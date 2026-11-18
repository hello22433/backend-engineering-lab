# Module 00 — Java 실행 원리와 빌드 시스템

- 상태: Not started
- 기간: 2~3주 예상 (주 6~8시간; 속도에 따라 조정)
- 원칙: **실행 원리를 직접 확인 → 자동화 도구의 필요성을 경험 → 실험으로 선택**
- 선수 지식: Java 기본 문법, CLI 기초
- 의사결정: [ADR-0002](../decisions/0002-build-system.md)

## 학습 질문
- 소스 코드, 바이트코드, 기계어의 차이는 무엇인가?
- `javac`, `java`, `javap`, `jar`는 각각 무슨 일을 하는가?
- JVM의 클래스 로더, 검증, 인터프리터, JIT는 어떻게 연결되는가?
- classpath와 module path는 무엇이 다른가?
- 외부 JAR를 추가하면 어떤 일이 생기는가? 전이 의존성은 누가 관리하는가?
- Maven의 lifecycle/phase/goal과 Gradle의 task graph는 어떻게 다른가?
- incremental build, build cache, reproducible build의 정의와 한계는?
- Bazel의 target/action graph 및 hermetic build는 어떤 문제를 해결하는가?

## 0. 준비
- [ ] JDK 21 설치 확인: `java -version`, `javac -version`
- [ ] Git, 터미널(Windows PowerShell 또는 WSL2) 확인
- [ ] IDE 없이 터미널로 컴파일/실행할 수 있는지 확인
- [ ] OS/JDK/CPU/RAM/도구 버전 기록
- [ ] 회사 코드·계정·비밀키를 개인 저장소에 사용하지 않기

## 1. 수동 컴파일과 실행 (학습용)
- [ ] `Hello.java` 작성
- [ ] `javac -d out Hello.java` 후 생성된 `.class` 확인
- [ ] `java -cp out Hello` 실행
- [ ] `javap -c -p -classpath out Hello`로 바이트코드 분석
- [ ] 패키지 2개, 클래스 3개로 확장하고 컴파일 순서 및 classpath 확인
- [ ] `jar --create --file app.jar -C out .`로 아카이브 생성
- [ ] `jar --list --file app.jar`로 내용 확인
- [ ] 실행 가능한 JAR manifest의 Main-Class를 학습하고 실행
- [ ] 의도적으로 ClassNotFoundException/NoClassDefFoundError 발생시켜 원인 기록

### 산출물
`learning/00-java-build/manual/` 및 `docs/notes/00-jvm-and-classpath.md`

## 2. JVM과 OS 관찰
- [ ] 실행 중인 Java 프로세스 PID 관찰
- [ ] 스레드 목록 및 스택 추적 관찰 (`jcmd`, `jstack` 등 사용 가능 범위)
- [ ] `javap -v`로 constant pool 관찰
- [ ] 클래스 로딩 로그 옵션 실습
- [ ] JVM heap/stack/metaspace를 개념적으로 구분
- [ ] 인터프리터와 JIT의 역할을 문서화
- [ ] OS 프로세스/스레드와 Java 스레드 관계 정리

### 범위 제한
JVM이나 컴파일러 전체를 직접 만들지 않는다. 바이트코드 관찰과 작은 실험으로 원리를 검증한다.

## 3. Maven으로 동일 프로젝트 빌드
- [ ] 최소 `pom.xml` 구성
- [ ] `mvn compile`, `mvn test`, `mvn package`, `mvn verify` 실행
- [ ] lifecycle phase와 plugin goal 구분
- [ ] 외부 의존성 하나 추가, 전이 의존성 확인
- [ ] `mvn dependency:tree`로 충돌/버전 해석 확인
- [ ] Maven Wrapper 도입 및 버전 고정

## 4. Gradle Kotlin DSL로 동일 프로젝트 빌드
- [ ] `settings.gradle.kts`, `build.gradle.kts` 구성
- [ ] Gradle Wrapper 사용 및 버전 고정
- [ ] `./gradlew tasks`, `test`, `build`, `dependencies` 실행 (Windows는 `gradlew.bat`)
- [ ] task dependency graph, configuration, plugin 역할 정리
- [ ] no-op / incremental / cache 실험
- [ ] 작은 멀티프로젝트 구성 (`core`, `app`)

## 5. Bazel 비교 (시간 초과 시 후순위)
- [ ] `MODULE.bazel`, `BUILD.bazel`의 역할 조사
- [ ] Java target을 정의하고 빌드·테스트
- [ ] action graph, sandboxing, hermeticity 설명
- [ ] Gradle/Maven 대비 설정·실행 경험 기록

## 6. 실험 및 문서
- [ ] 동일 조건의 clean/no-op/incremental 빌드 비교
- [ ] 반복 측정 및 중앙값 기록 (환경·명령·버전 명시)
- [ ] 결과를 일반적 우열로 과장하지 않기
- [ ] 설계 비교표 및 개인 프로젝트 선택 이유 기록
- [ ] AI 없이 5분 설명 + 의존성 오류 하나 직접 해결

## 완료 기준
- [ ] 수동 컴파일·실행·JAR 패키징 가능
- [ ] 바이트코드·클래스 로딩·classpath를 설명 가능
- [ ] Maven/Gradle의 책임과 차이를 설명 가능
- [ ] 빌드 도구 비교 결과가 재현 가능한 형태로 기록됨
- [ ] Gradle 기본 선택의 근거와 포기한 대안 설명 가능

## 공식 자료
- https://docs.oracle.com/en/java/javase/21/docs/specs/man/
- https://docs.oracle.com/javase/specs/jvms/se21/html/
- https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html
- https://docs.gradle.org/current/userguide/userguide.html
- https://bazel.build/about/intro
