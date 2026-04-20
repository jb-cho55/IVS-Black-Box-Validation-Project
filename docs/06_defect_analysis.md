# Defect Analysis

## 1. Overview

본 문서는 IVS 제어기 Black Box Testing 프로젝트에서 식별한 결함을 통합 정리한 문서이다.

결함은 크게 다음 두 범주로 나누어 분석했다.

- **정적 테스팅 결함**: 요구사항 문서, 메시지 정의, Signal Description, 값 표현 방식 검토 과정에서 발견한 결함
- **동적 테스팅 결함**: 실제 입력 조건과 테스트 실행 결과를 비교하는 과정에서 발견한 결함

본 프로젝트에서는 총 **15건의 결함**을 정리했다.

- **정적 테스팅 결함**: 4건
- **동적 테스팅 결함**: 11건

---

## 2. Defect Classification

| Overall Defect No. | Category | Section Defect No. | Title | Reference Page |
|---|---|---|---|---|
| 1 | Static | Static Defect 1 | Ignition_sts Signal Length Mismatch | p.7 |
| 2 | Static | Static Defect 2 | Value Representation Mismatch (Brake_press) | p.10 |
| 3 | Static | Static Defect 3 | Value Representation Mismatch (ACCEL_press) | p.11 |
| 4 | Static | Static Defect 4 | Brake_Err_sts Description Mismatch | p.19 |
| 5 | Dynamic | Dynamic Defect 1 | Batt Percent 15% Boundary Miss-Detection | p.27 |
| 6 | Dynamic | Dynamic Defect 2 | IGN 50 Cycle Off-by-One Issue | p.27 ~ p.28 |
| 7 | Dynamic | Dynamic Defect 3 | Batt Percent 80% Equality Omission | p.28 |
| 8 | Dynamic | Dynamic Defect 4 | Battery Charging Fault False Detection Under Invalid Pre-condition | p.28 |
| 9 | Dynamic | Dynamic Defect 5 | Battery Voltage High Fault Detection Under Invalid Pre-condition | p.29 |
| 10 | Dynamic | Dynamic Defect 6 | Battery Voltage Recovery Threshold Mismatch | p.29 |
| 11 | Dynamic | Dynamic Defect 7 | Ignition Fault Level 2 Miss-Detection | p.30 |
| 12 | Dynamic | Dynamic Defect 8 | Engine Fault Level 2 Miss-Detection | p.31 |
| 13 | Dynamic | Dynamic Defect 9 | Brake / Accel / Vehicle Independent Fault Miss-Detection | p.32 |
| 14 | Dynamic | Dynamic Defect 10 | Steering Fault False Detection Under Invalid Condition | p.33 |
| 15 | Dynamic | Dynamic Defect 11 | Steering Fault Timing Error | p.33 |

---

## 3. Static Defect Summary

## 3.1 Static Defect 1 - Ignition_sts Signal Length Mismatch

- **Overall Defect No.**: 1
- **Reference Page**: p.7
- **Issue**: `Ignition_sts`는 `Off / On / Error`의 3가지 상태를 표현해야 하지만, Signal length가 1bit로 정의되어 있었다.
- **Impact**: 상태 수 대비 bit length가 부족하여 Ignition Error 관련 요구사항과 충돌할 가능성이 있다.
- **Suggested Fix**: Signal length를 최소 2bit로 수정

---

## 3.2 Static Defect 2 - Value Representation Mismatch (Brake_press)

- **Overall Defect No.**: 2
- **Reference Page**: p.10
- **Issue**: Error 값 `127`과 `0x7f`의 표현이 일관되지 않았다.
- **Impact**: 요구사항 해석, 테스트 기대값 정의, Error 상태 검증 시 혼동 가능성이 있다.
- **Suggested Fix**: `0x7f` 기준으로 표현 통일

---

## 3.3 Static Defect 3 - Value Representation Mismatch (ACCEL_press)

- **Overall Defect No.**: 3
- **Reference Page**: p.11
- **Issue**: ACCEL_press 항목에서도 동일하게 `127`과 `0x7f` 표현 불일치가 확인되었다.
- **Impact**: 동일한 값 표현 오류가 반복되어 문서/정의 일관성이 떨어진다.
- **Suggested Fix**: `0x7f` 기준으로 표현 통일

---

## 3.4 Static Defect 4 - Brake_Err_sts Description Mismatch

- **Overall Defect No.**: 4
- **Reference Page**: p.19
- **Issue**: `Brake_Err_sts` Signal의 Description이 `Accel Press State`로 기재되어 있었다.
- **Impact**: 테스트 설계, 구현 검토, 결함 분석 과정에서 Signal 해석 혼동이 발생할 수 있다.
- **Suggested Fix**: Description을 `Brake press state`로 수정

---

## 4. Dynamic Defect Summary

## 4.1 Dynamic Defect 1 - Batt Percent 15% Boundary Miss-Detection

- **Overall Defect No.**: 5
- **Reference Page**: p.27
- **Condition**: IGN = 1, ENG = 1, Batt Percent = 15
- **Expected**: Fault Level 2 검출
- **Actual**: 15%에서만 미검출, 15% 초과에서는 정상 검출
- **Analysis**: 경계값 비교에서 `<=`가 아닌 `<`로 처리된 가능성
- **Suggested Fix**: `Batt Percent ≤ 15%` 조건 반영

---

## 4.2 Dynamic Defect 2 - IGN 50 Cycle Off-by-One Issue

- **Overall Defect No.**: 6
- **Reference Page**: p.27 ~ p.28
- **Condition**: IGN Off → On 반복
- **Expected**: 50회에서 Function 동작
- **Actual**: 49회에서 동작
- **Analysis**: 카운트 기준이 1 부족하게 설정된 off-by-one 문제
- **Suggested Fix**: Count 기준을 50으로 수정

---

## 4.3 Dynamic Defect 3 - Batt Percent 80% Equality Omission

- **Overall Defect No.**: 7
- **Reference Page**: p.28
- **Condition**: IGN = 1, Engine = 1, Batt Percent = 80, Chg_sts = 1
- **Expected**: Fault Level 2 검출
- **Actual**: 80%에서만 미검출, 80% 초과는 정상 검출
- **Analysis**: `>= 80`이 아니라 `> 80`로 구현된 가능성
- **Suggested Fix**: `80% 이상` 조건 정확히 반영

---

## 4.4 Dynamic Defect 4 - Battery Charging Fault False Detection Under Invalid Pre-condition

- **Overall Defect No.**: 8
- **Reference Page**: p.28
- **Condition**: IGN = 1, Engine = 0, Batt Percent > 80, Chg_sts = 1
- **Expected**: 미검출
- **Actual**: Fault Level 2 검출
- **Analysis**: `Engine == 1` Pre-condition 미반영 가능성
- **Suggested Fix**: 검출 조건을 `IGN == 1 && Engine == 1 && Batt Percent > 80 && Chg_sts == 1`로 제한

---

## 4.5 Dynamic Defect 5 - Battery Voltage High Fault Detection Under Invalid Pre-condition

- **Overall Defect No.**: 9
- **Reference Page**: p.29
- **Condition**: IGN ≠ 1, Engine ≠ 1, Batt Voltage > 20V
- **Expected**: 미검출
- **Actual**: Fault Level 2 검출
- **Analysis**: `IGN == 1 && Engine == 1` 조건 미반영
- **Suggested Fix**: Batt Voltage High 검출에 Pre-condition 반영

---

## 4.6 Dynamic Defect 6 - Battery Voltage Recovery Threshold Mismatch

- **Overall Defect No.**: 10
- **Reference Page**: p.29
- **Condition**: IGN = 1, Engine = 1, Batt Voltage recovery
- **Expected**: `Batt Voltage ≤ 15V`에서 Fault Level 1 회복
- **Actual**: `Batt Voltage < 0.015V`에서만 회복
- **Analysis**: Recovery 기준값 또는 스케일 해석 오류
- **Suggested Fix**: `Batt Voltage ≤ 15V`로 회복 조건 수정

---

## 4.7 Dynamic Defect 7 - Ignition Fault Level 2 Miss-Detection

- **Overall Defect No.**: 11
- **Reference Page**: p.30
- **Condition**: `Ignition_sts = 3`, `40 ≤ Batt Percent ≤ 80`, `Engine_sts = 1`
- **Expected**: Fault Level 2 검출
- **Actual**: 5100ms까지 측정했으나 미검출
- **Analysis**: 정적 결함(Overall Defect 1)과 연결 가능성이 있었으나, length 수정 후에도 미검출되어 로직 결함으로 판단
- **Suggested Fix**: Ignition Fault 검출 로직 재검토

---

## 4.8 Dynamic Defect 8 - Engine Fault Level 2 Miss-Detection

- **Overall Defect No.**: 12
- **Reference Page**: p.31
- **Condition**: `Engine_sts = 2`, `40 ≤ Batt Percent ≤ 80`, `IGN = 1`
- **Expected**: Fault Level 2 검출
- **Actual**: 5100ms까지 미검출, `Engine_sts = 3`에서도 미검출
- **Analysis**: Engine 상태값 매핑 또는 검출 로직 문제 가능성
- **Suggested Fix**: Engine Fault 검출 조건 재검토

---

## 4.9 Dynamic Defect 9 - Brake / Accel / Vehicle Independent Fault Miss-Detection

- **Overall Defect No.**: 13
- **Reference Page**: p.32
- **Condition**: `Brake Press = 0x7F` 또는 `Accel_press = 0x7F` 또는 `Vehicle_Speed = 255`
- **Expected**: 각 조건에서 독립적으로 Fault Level 2 검출
- **Actual**: 독립적으로 검출되지 않음
- **Analysis**: 개별 입력 조건의 독립 검출 경로가 제대로 구현되지 않은 가능성
- **Suggested Fix**: Brake / Accel / Vehicle 조건을 독립적으로 검출 로직에 반영

---

## 4.10 Dynamic Defect 10 - Steering Fault False Detection Under Invalid Condition

- **Overall Defect No.**: 14
- **Reference Page**: p.33
- **Condition**: `(Steering_Angle > 720 || Steering_Angle < -720) && Engine == 1 && Ignition == 0`
- **Expected**: 미검출
- **Actual**: Fault Level 2 검출
- **Analysis**: Ignition 관련 Pre-condition 미반영 가능성
- **Suggested Fix**: 요구사항 p.33에 맞게 미검출되도록 로직 수정

---

## 4.11 Dynamic Defect 11 - Steering Fault Timing Error

- **Overall Defect No.**: 15
- **Reference Page**: p.33
- **Condition**: Steering_Err_sts 관련 검출 시점 확인
- **Expected**: `50 ± 10ms`에서 검출
- **Actual**: `1000 ± 10ms` 수준에서 검출
- **Analysis**: 검출은 발생했지만 요구사항 대비 타이밍 오차가 크게 발생
- **Suggested Fix**: 검출 시점을 `50 ± 10ms` 수준으로 수정

---

## 5. Defect Pattern Analysis

본 프로젝트에서 확인한 결함은 다음 패턴으로 분류할 수 있다.

### 5.1 Specification Definition Issues
- Signal length 부족
- 값 표현 불일치
- Signal 명과 Description 불일치

### 5.2 Boundary Condition Issues
- 15% 경계값 미검출
- 80% equality 조건 누락

### 5.3 Pre-condition Issues
- Battery Charging Fault 오검출
- Battery Voltage Fault 오검출
- Steering Fault 오검출

### 5.4 State / Logic Issues
- Ignition Fault 미검출
- Engine Fault 미검출
- Brake / Accel / Vehicle 조건 독립 검출 실패

### 5.5 Timing / Counter Issues
- IGN 50 Cycle off-by-one
- Steering Fault timing error

### 5.6 Threshold / Scaling Issues
- Battery Voltage Recovery 기준 불일치

---

## 6. Root Cause Perspective

결함들을 종합하면, 주요 원인은 아래와 같이 정리할 수 있다.

- 요구사항 해석과 정의 간 불일치
- 경계값 처리(`>`, `>=`, `<`, `<=`) 오류
- Pre-condition 반영 누락
- 상태값 매핑 또는 Fault 검출 로직 오류
- 타이밍 및 카운트 기준 설정 오류
- 단위/스케일 해석 오류

---

## 7. Why These Defects Matter

본 프로젝트의 결함은 단순 오작동 수준이 아니라, 차량 제어기 검증 관점에서 중요한 의미를 가진다.

- 잘못된 Fault 검출은 정상 상태를 고장으로 판단하게 만들 수 있다.
- 미검출은 실제 고장을 놓칠 수 있다.
- 경계값 오류는 특정 조건에서만 불안정한 동작을 만들 수 있다.
- 타이밍 오류는 상태 전이 및 제어 응답 신뢰성에 영향을 줄 수 있다.
- 문서 정의 불일치는 테스트 정확도와 유지보수성 저하로 이어질 수 있다.

---

## 8. Key Takeaways

이 프로젝트를 통해 다음과 같은 점을 확인할 수 있었다.

- 요구사항 기반 검증에서는 정적 결함과 동적 결함을 함께 봐야 한다.
- 경계값, Pre-condition, 타이밍, 상태 전이는 모두 독립적인 검증 포인트가 된다.
- 결함 분석은 단순 PASS/FAIL 정리가 아니라, 왜 실패했는지 논리적으로 연결하는 과정이 중요하다.
- 테스트 엔지니어는 실행 결과뿐 아니라 정의 단계의 불일치까지 식별할 수 있어야 한다.

---

## 9. Summary

본 프로젝트에서는 총 15건의 결함을 식별했다.

- **정적 결함 4건**
  - Signal definition / value representation / description consistency 문제

- **동적 결함 11건**
  - 경계값 처리 오류
  - Pre-condition 미반영
  - 검출/회복 로직 결함
  - 타이밍 및 카운트 오류

이 결함들은 IVS 제어기의 요구사항 기반 검증에서 중요한 개선 포인트를 드러냈으며,  
정적/동적 테스트를 병행해야 실제 품질 이슈를 효과적으로 식별할 수 있음을 보여준다.
