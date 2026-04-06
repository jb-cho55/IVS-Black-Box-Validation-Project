# Project Overview

## 1. Project Introduction

본 프로젝트는 **IVS 제어기**를 대상으로 CAN 기반 입력/출력 요구사항과 UDS 요청/응답, 그리고 고장 상태 관리 로직을 검증한 **Requirement-Based Black Box Testing 프로젝트**이다.

공식 프로젝트명은 **자동화 테스팅을 통한 Black Box Testing 및 검증의 이해**이며, 프로젝트에서는 실제 차량 제어기 검증과 유사한 구조를 바탕으로 IVS의 입력 해석, 응답 메시지 송신, Fault 상태 전이 및 삭제 조건을 검증했다.

---

## 2. Project Background

차량 제어기 검증에서는 단순히 입력에 대한 출력만 확인하는 것이 아니라,  
요구사항에 정의된 조건에 따라 정상적으로 동작하는지, 고장이 올바르게 검출되고 복구되는지, 그리고 상태 전이가 일관되게 관리되는지를 함께 검증해야 한다.

본 프로젝트는 이러한 관점에서 IVS 제어기를 대상으로 다음과 같은 항목을 검증하는 것을 목표로 했다.

- CAN 입력 메시지 해석
- UDS 요청/응답 처리
- Fault Detection / Recovery
- Deleted / Fixed / Faulted 상태 관리
- IGN Off → On 반복에 따른 고장 삭제 조건

---

## 3. Project Goal

본 프로젝트의 목표는 다음과 같다.

- IVS 제어기의 요구사항을 테스트 관점에서 해석한다.
- CANoe 기반의 검증 환경을 직접 구축한다.
- 수동 검증과 자동화 검증이 가능한 테스트 구조를 만든다.
- 정적 테스팅과 동적 테스팅을 통해 결함을 식별한다.
- 결함의 원인을 분석하고 개선 방향을 제시한다.

---

## 4. Validation Target

검증 대상은 **IVS 제어기**이며, 주변 ECU로부터 입력을 받아 상태를 판단하고,  
필요한 응답 메시지 및 Fault 상태를 관리하는 동작을 중심으로 검증했다.

### Input ECU

- **BMS**
  - Batt_Percent
  - Batt_Chg_Sts
  - Batt_Voltage

- **ENG**
  - Ignition_sts
  - Engine_sts

- **STR**
  - Steering_angle

- **BRK**
  - Brake_press
  - ACCEL_press
  - Vehicle_Speed

### UDS Request / Response

- 소프트웨어 버전 요청
- 전체 고장 상태 요청
- 전체 고장 삭제 요청

---

## 5. Validation Scope

### 5.1 CAN Input Validation
IVS가 각 ECU에서 전달받는 입력 메시지를 요구사항에 맞게 해석하는지 검증했다.

### 5.2 UDS Communication Validation
UDS 요청 메시지에 대해 IVS가 적절한 응답을 송신하는지 검증했다.

### 5.3 Fault Management Validation
Fault가 요구사항에 따라 검출, 복구, 삭제되는지 확인했다.

주요 검증 항목은 다음과 같다.

- Fault Detection
- Fault Recovery
- Deleted(0) / Fixed(1) / Faulted(2) 상태 관리
- 동일 요청에 대한 응답 조건
- IGN Off → On 50회 전환 시 고장 삭제 조건
- 경계값 및 타이밍 조건

---

## 6. Project Execution Flow

본 프로젝트는 아래와 같은 흐름으로 진행했다.

1. 요구사항 분석
2. CANoe 네트워크 환경 구축
3. CANdb 작성
4. Panel / Trace 기반 수동 검증 환경 구성
5. 자동화 테스트 환경 및 모듈 생성
6. 테스트 케이스 설계
7. 정적 테스팅 수행
8. 동적 테스팅 수행
9. 결함 분석 및 개선 방향 정리

---

## 7. Test Approach

본 프로젝트에서는 **정적 테스팅**과 **동적 테스팅**을 함께 수행했다.

### Static Testing
요구사항 문서, 메시지 정의, Signal Description 간의 불일치를 검토하여  
명세 단계에서 발생할 수 있는 결함을 식별했다.

예시:
- Signal length와 상태 정의 간 충돌
- 값 표현 방식 불일치
- Signal 명과 Description 불일치

### Dynamic Testing
실제 입력 조건을 구성해 IVS의 동작을 검증하고,  
Fault 검출/회복, 상태 전이, 경계값, 타이밍 동작을 확인했다.

예시:
- Batt Percent 경계값 검증
- Batt Voltage 검출/회복 검증
- IGN 50 Cycle 동작 검증
- Pre-condition 충족 여부에 따른 Fault 검출 검증

---

## 8. Key Outcomes

본 프로젝트를 통해 다음과 같은 결과를 얻을 수 있었다.

- IVS 제어기 검증을 위한 CANoe 기반 테스트 환경 구축
- CANdb 및 Panel / Trace 기반 수동 검증 환경 구성
- 반복 검증을 위한 자동화 테스트 환경 구성
- 요구사항 기반 테스트 케이스 설계 및 재사용 구조 적용
- 정적/동적 테스팅을 통한 다양한 결함 식별
- 결함 원인 분석 및 수정 방향 도출

---

## 9. Meaning of the Project

이 프로젝트는 단순히 테스트를 수행한 프로젝트가 아니라,  
**요구사항을 해석하고, 검증 환경을 구성하고, 테스트를 설계하고, 실제 동작을 검증한 뒤, 결함의 원인을 분석하는 전체 흐름을 경험한 프로젝트**라는 점에서 의미가 있다.

특히 차량 제어기 검증에서 중요한 다음 요소들을 직접 경험할 수 있었다.

- 요구사항 기반 검증
- 상태 전이 기반 Fault 관리 이해
- 경계값 및 타이밍 검증의 중요성
- 수동 검증과 자동화 검증의 병행
- 결함 검출 이후 원인 추론과 개선 방향 제시
