# Backend Engineering Lab

Java/Spring 기반 백엔드 엔지니어링 학습 및 재사용 가능한 기술 자산 구축 프로젝트.

## 목표
1. CS 원리와 공식 표준을 이해한다.
2. 핵심 동작을 학습용으로 직접 구현한다.
3. 검증된 프레임워크를 이용해 실전용 모듈을 만든다.
4. 테스트·장애·보안·성능 검증 결과를 기록한다.
5. 설계 판단을 ADR로 축적하고 다른 프로젝트에서 재사용한다.

## 문서
- [기술 스택 선정 근거](docs/decisions/0001-tech-stack.md)
- [학습 및 모듈 개발 로드맵](docs/plans/roadmap.md)

## 예정 구조
```text
learning/       # 원리 이해를 위한 학습용 구현 (운영 사용 금지)
libraries/      # 검증된 재사용 라이브러리
templates/      # 프로젝트 시작용 템플릿
applications/   # 통합 실습 애플리케이션
docs/decisions/ # Architecture Decision Records
docs/plans/     # 계획과 진행 상황
benchmarks/     # 성능 및 장애 검증 결과
```

## 학습 원칙
- 직접 설계하고 AI로 구현·리뷰를 보조한다.
- 학습용 코드와 운영용 코드를 구분한다.
- 기술은 필요성과 대안·트레이드오프를 검토한 뒤 도입한다.
- 회사 소스코드, 고객 정보, 비밀키 등 기밀 자료는 업로드하지 않는다.

현재 단계: **계획 및 기술 선정 근거 문서화**. 구현은 아직 시작하지 않았다.
