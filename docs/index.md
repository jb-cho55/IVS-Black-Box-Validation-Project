# Documentation Index

## Overview

본 디렉토리는 IVS 제어기 Black Box Testing 프로젝트의 상세 문서를 정리한 공간이다.  
프로젝트 개요, 요구사항 요약, 테스트 환경, 정적/동적 테스팅 결과, 결함 분석 내용을 문서별로 분리해 정리했다.

README가 프로젝트의 전체 요약과 대표 결과를 보여주는 페이지라면,  
`docs/`는 각 주제를 더 자세하게 설명하는 상세 문서 모음 역할을 한다.

---

## Document List

### 1. Project Overview
프로젝트의 목적, 배경, 검증 대상, 전체 수행 흐름을 정리한 문서

- [01_project_overview.md](./01_project_overview.md)

### 2. Requirement Summary
요구사항을 테스트 관점에서 재정리한 문서  
입력 ECU, UDS 요청/응답, Fault 상태 관리, 경계값 및 타이밍 포인트를 포함

- [02_requirement_summary.md](./02_requirement_summary.md)

### 3. Test Environment
CANoe network node 구성, CANdb 작성, Panel / Trace 기반 수동 검증 환경, 자동화 테스트 환경을 정리한 문서

- [03_test_environment.md](./03_test_environment.md)

### 4. Static Testing
정적 테스팅을 통해 발견한 결함과 근거 페이지, 영향, 수정 방향을 정리한 문서

- [04_static_testing.md](./04_static_testing.md)

### 5. Dynamic Testing
동적 테스팅을 통해 확인한 결함, 테스트 조건, 기대 결과, 실제 결과, 분석 내용을 정리한 문서

- [05_dynamic_testing.md](./05_dynamic_testing.md)

### 6. Defect Analysis
정적/동적 결함 전체를 통합 관점에서 분류하고 분석한 문서

- [06_defect_analysis.md](./06_defect_analysis.md)

---

## Recommended Reading Order

처음 프로젝트를 보는 경우 아래 순서로 읽는 것을 권장한다.

1. [Project Overview](./01_project_overview.md)
2. [Requirement Summary](./02_requirement_summary.md)
3. [Test Environment](./03_test_environment.md)
4. [Static Testing](./04_static_testing.md)
5. [Dynamic Testing](./05_dynamic_testing.md)
6. [Defect Analysis](./06_defect_analysis.md)

---

## Document Relationship

각 문서는 다음과 같은 관계를 가진다.

- **Project Overview**  
  프로젝트 전체 목적과 흐름 설명

- **Requirement Summary**  
  테스트의 기준이 되는 요구사항 정리

- **Test Environment**  
  요구사항을 실제로 검증하기 위한 환경 설명

- **Static Testing / Dynamic Testing**  
  요구사항 기반 검증 수행 결과 정리

- **Defect Analysis**  
  전체 결함을 유형별, 원인별로 통합 분석

---

## Related Resources

상세 보고서와 발표자료는 아래 경로에서 확인할 수 있다.

- `../reports/result_report.pdf`
- `../reports/defect_analysis.pdf`
- `../reports/presentation.pptx`

대표 이미지 및 문서 내 사용한 캡처 이미지는 아래 경로에 정리했다.

- `../assets/images/`

---

## Notes

- 정적 테스팅 문서는 원문 기준 페이지와 근거 이미지를 함께 표기해 추적성을 높였다.
- 동적 테스팅 문서는 요구사항 페이지와 실제 결과 이미지를 함께 배치해 요구사항과 결과를 직접 비교할 수 있도록 구성했다.
- 결함 분석 문서는 전체 결함을 정적/동적 관점에서 통합 정리한 요약 문서이다.
