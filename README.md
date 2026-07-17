# Automotive NMOS High-Temperature Degradation Analysis and Process Optimization

<p align="center">
  <b>
    Synopsys Sentaurus TCAD를 활용한 차량용 NMOS의<br>
    423 K 고온 구동 열화 분석 및 공정 파라미터 최적화
  </b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Tool-Synopsys%20Sentaurus%20TCAD-00599C">
  <img src="https://img.shields.io/badge/Device-NMOS-2E8B57">
  <img src="https://img.shields.io/badge/Temperature-423%20K-E67E22">
  <img src="https://img.shields.io/badge/Project-Team%20Project-6C757D">
  <img src="https://img.shields.io/badge/Role-Team%20Leader-8E44AD">
</p>

---

## Project Summary

차량용 반도체는 고집적화와 함께 엔진룸 및 실외 환경에서 발생할 수 있는 고온 조건에서도 안정적인 전기적 특성을 유지해야 합니다.

본 프로젝트에서는 **Synopsys Sentaurus TCAD**를 활용하여 NMOS의 온도 상승과 채널 길이 감소에 따른 열화 특성을 분석했습니다. 이후 **Halo 도핑, LDD 도핑, P-well 농도**를 단계적으로 최적화하여 423 K 환경에서 발생하는 Short-Channel Effect와 off-state leakage를 완화했습니다.

최종적으로 423 K, Gate Length 0.12 μm 조건에서 다음 결과를 얻었습니다.

| Metric | Baseline | Optimized | Improvement |
| :--- | ---: | ---: | :--- |
| Subthreshold Swing | 1015.2 mV/dec | **198.6 mV/dec** | **80.4% 감소** |
| Ioff | 2.81 × 10⁻⁵ A/μm | **1.58 × 10⁻¹² A/μm** | **약 7 orders 감소** |
| Ion/Ioff | 23 | **1.39 × 10⁸** | **약 6 × 10⁶배 향상** |

---

## Contents

1. [Project Information](#1-project-information)
2. [My Contribution](#2-my-contribution)
3. [Problem Definition](#3-problem-definition)
4. [Simulation Environment](#4-simulation-environment)
5. [Optimization Strategy](#5-optimization-strategy)
6. [Halo Doping Optimization](#6-halo-doping-optimization)
7. [LDD Optimization](#7-ldd-optimization)
8. [P-well Refinement](#8-p-well-refinement)
9. [Final Results](#9-final-results)
10. [Technical Interpretation](#10-technical-interpretation)
11. [Implementation Highlights](#11-implementation-highlights)
12. [Documentation](#12-documentation)
13. [Source Code](#13-source-code)
14. [Scope and Limitations](#14-scope-and-limitations)

---

# 1. Project Information

| Item | Description |
| :--- | :--- |
| Course | 반도체집적공정 |
| Term | 2026-1 |
| Project Type | Team Project |
| Team Size | 4 members |
| Team Leader | 송민호 |
| Device | Planar NMOS |
| Simulation Tool | Synopsys Sentaurus Workbench T-2022.03 |
| TCAD Modules | Sprocess, Sdevice, Svisual |
| Temperature Conditions | 300 K, 423 K |
| Optimization Baseline | 423 K, Gate Length 0.12 μm |
| Optimization Variables | Halo Dose, Halo Energy, LDD Dose, LDD Energy, P-well Concentration |
| Evaluation Metrics | Ion, Ioff, Ion/Ioff Ratio, Subthreshold Swing |

---

# 2. My Contribution

## Team Leadership

팀장으로서 프로젝트의 전체 방향을 설정하고, 팀원별 역할을 분배하여 시뮬레이션·데이터 분석·보고서·발표자료 작업이 유기적으로 진행되도록 관리했습니다.

- 프로젝트 주제 및 분석 방향 구체화
- 팀원별 역할과 작업 범위 배분
- 단계별 시뮬레이션 진행 상황 관리
- 팀원별 결과 통합 및 최종 결과 검토
- 보고서와 발표자료의 전체 논리 흐름 조정

## TCAD Code Implementation

전체 Sprocess, Sdevice, Svisual workflow 중 약 절반의 코드 구현과 수정을 담당했습니다.

- 423 K 고온 구동 조건 파라미터화
- 공정 변수의 Sentaurus Workbench parameter 연동
- Halo 및 LDD implantation 조건 구성
- Id–Vg sweep 및 전기적 특성 추출 workflow 구성
- Ion, Ioff, SS, Vtgm, gm 자동 추출 코드 적용

## Halo Optimization — Primary Responsibility

**Halo 도핑 최적화 과정 전체를 단독으로 수행했습니다.**

- Halo Dose의 광범위한 1차 sweep 수행
- 정밀 탐색 구간 선정
- Halo Energy의 세부 sweep 수행
- Ion, Ioff, Ion/Ioff, SS 비교
- 과도한 Halo 조건에서 발생하는 Ion 저하 분석
- 최적 Halo 조건 선정
- Halo 도핑의 punch-through 억제 효과에 대한 물리적 해석

선정한 최종 Halo 조건은 다음과 같습니다.

| Parameter | Selected Value |
| :--- | ---: |
| Halo Dose | 1.5 × 10¹³ cm⁻² |
| Halo Energy | 26 keV |
| Ion/Ioff | 867 |
| Subthreshold Swing | 838.3 mV/dec |

## Documentation

- 최종 발표자료의 일부 슬라이드 구성
- Halo 최적화 결과 표와 그래프 정리
- 공정 변수별 성능 변화 및 물리적 의미 정리
- 최종 보고서와 PPT의 결과 검토

---

# 3. Problem Definition

## 3.1 Temperature-Induced Degradation

Gate Length가 0.25 μm인 기준 NMOS의 온도를 300 K에서 423 K로 상승시켰을 때 다음과 같은 열화가 나타났습니다.

| Metric | 300 K | 423 K | Change |
| :--- | ---: | ---: | :--- |
| Ioff | 1.29 × 10⁻¹⁴ A/μm | 1.07 × 10⁻¹⁰ A/μm | 약 4 orders 증가 |
| Subthreshold Swing | 75.9 mV/dec | 115.9 mV/dec | 40.0 mV/dec 증가 |

고온에서는 intrinsic carrier concentration이 증가하면서 off-state 누설전류가 커집니다. 동시에 문턱전압 이하 영역의 전류 기울기가 완만해져 게이트의 채널 제어력이 약화됩니다.

<p align="center">
  <img src="./온도상승에따른성능변화.png"
       width="82%"
       alt="NMOS degradation with increasing temperature"/>
</p>

---

## 3.2 Gate Length Scaling at 423 K

고온과 소자 미세화가 동시에 적용되는 가혹 조건을 설정하기 위해 423 K에서 Gate Length split을 수행했습니다.

Gate Length가 감소할수록 source와 drain의 공핍 영역이 채널 내부로 확장되면서 Short-Channel Effect가 심화되었습니다.

특히 Gate Length 0.12 μm에서는 다음과 같은 성능 저하가 나타났습니다.

| Metric | Baseline Result |
| :--- | ---: |
| Temperature | 423 K |
| Gate Length | 0.12 μm |
| Ion | 6.56 × 10⁻⁴ A/μm |
| Ioff | 2.81 × 10⁻⁵ A/μm |
| Ion/Ioff | 23 |
| Subthreshold Swing | 1015.2 mV/dec |

따라서 **423 K, Gate Length 0.12 μm** 조건을 이후 공정 최적화의 baseline으로 선정했습니다.

<p align="center">
  <img src="./베이스라인1.png"
       width="46%"
       alt="Baseline NMOS degradation"/>
  <img src="./베이스라인2.png"
       width="46%"
       alt="Ion/Ioff and subthreshold swing design map"/>
</p>

---

# 4. Simulation Environment

## 4.1 TCAD Workflow

```text
Sentaurus Workbench
        |
        v
Sprocess
  - NMOS process structure generation
  - Gate oxidation and patterning
  - LDD and Halo implantation
  - Source/Drain implantation
  - Annealing and contact formation
        |
        v
Sdevice
  - Temperature-dependent simulation
  - Drain-voltage ramp
  - Gate-voltage sweep
  - Carrier transport calculation
        |
        v
Svisual
  - Id–Vg visualization
  - Threshold voltage extraction
  - Ion and Ioff extraction
  - Subthreshold Swing extraction
  - Transconductance extraction
```

## 4.2 Device Physics Models

| Category | Model or Condition |
| :--- | :--- |
| Temperature | `Temperature = @Temp@` |
| Bulk Mobility | PhuMob |
| High-Field Mobility | HighFieldSaturation |
| Surface Mobility | Enormal |
| Recombination | SRH with DopingDependence |
| Effective Intrinsic Density | OldSlotboom |
| Gate Sweep | 0 V to 2.5 V |
| Drain Voltage | Parameterized through `@Vd@` |
| Subthreshold Analysis | Vd = 0.01 V |
| Main High-Temperature Condition | 423 K |

## 4.3 Workbench Parameters

| Parameter | Description |
| :--- | :--- |
| `Lg` | Gate Length |
| `NWell` | Boron-based p-type substrate/P-well concentration |
| `GOxTime` | Gate oxidation time |
| `LDD_Dose` | LDD implantation dose |
| `LDD_Energy` | LDD implantation energy |
| `Halo_Dose` | Halo implantation dose |
| `Halo_Energy` | Halo implantation energy |
| `RTA` | Rapid Thermal Annealing time |
| `Temp` | Device operating temperature |
| `Vd` | Drain voltage |

> **Parameter naming note:**  
> 기존 Workbench에서는 Boron 기반 p-type 농도 변수에 `NWell`이라는 이름을 사용했습니다. 물리적으로는 P-well 또는 p-type substrate 농도를 의미하므로, 향후 코드에서는 `PWell`로 변경하는 것이 더 명확합니다.

---

# 5. Optimization Strategy

본 프로젝트에서는 여러 변수를 동시에 변경하지 않고, 이전 단계의 선정 조건을 고정한 뒤 다음 변수를 탐색하는 **Sequential Split 방식**을 적용했습니다.

```text
High-Temperature Baseline Definition
                  |
                  v
          Halo Dose Sweep
                  |
                  v
         Halo Energy Sweep
                  |
                  v
           LDD Dose Sweep
                  |
                  v
          LDD Energy Sweep
                  |
                  v
     P-well Concentration Refinement
                  |
                  v
        Final Device Comparison
```

최적 조건은 특정 지표 하나를 최대화하는 방식이 아니라 다음 성능 사이의 균형을 기준으로 선정했습니다.

| Metric | Optimization Direction | Engineering Meaning |
| :--- | :---: | :--- |
| Subthreshold Swing | Minimize | 게이트 제어력 향상 |
| Ioff | Minimize | 고온 누설전류 억제 |
| Ion/Ioff | Maximize | 스위칭 안정성 향상 |
| Ion | Preserve | 구동 전류 저하 완화 |

---

# 6. Halo Doping Optimization

> 이 절의 Dose sweep, Energy sweep, 최적 조건 선정 및 물리적 해석은 송민호가 전담했습니다.

## 6.1 Engineering Purpose

Halo implantation은 source와 drain 인근 채널 영역의 국부 도핑 농도를 증가시켜 공핍층 확장을 억제하고 punch-through를 감소시키는 목적으로 적용했습니다.

그러나 Halo 조건이 과도하면 채널 내부의 전위 장벽이 높아져 Ion이 감소할 수 있습니다. 따라서 가장 낮은 Ioff 또는 가장 높은 Ion/Ioff만을 선택하지 않고, 누설전류 억제와 구동 전류 유지 사이의 trade-off를 함께 고려했습니다.

---

## 6.2 Two-Stage Halo Dose Optimization

Halo Dose는 한 번의 sweep으로 결정하지 않고, **광범위 탐색과 정밀 탐색의 두 단계**로 나누어 최적화했습니다.

### 6.2.1 Broad Dose Sweep

먼저 Halo Energy를 15 keV로 고정하고 Dose를 `5.0×10¹² ~ 1.0×10¹⁴ cm⁻²` 범위에서 넓게 sweep했습니다.

| Halo Dose | Ion | Ioff | Ion/Ioff | SS |
| ---: | ---: | ---: | ---: | ---: |
| 5.0 × 10¹² cm⁻² | 6.23 × 10⁻⁴ | 2.08 × 10⁻⁵ | 30 | 960.0 |
| 1.0 × 10¹³ cm⁻² | 5.69 × 10⁻⁴ | 1.40 × 10⁻⁵ | 41 | 938.1 |
| 2.0 × 10¹³ cm⁻² | 3.56 × 10⁻⁴ | 5.22 × 10⁻⁶ | 68 | 961.6 |
| 5.0 × 10¹³ cm⁻² | 6.42 × 10⁻⁶ | 3.03 × 10⁻⁷ | 21 | 1322.0 |
| 1.0 × 10¹⁴ cm⁻² | 1.75 × 10⁻⁷ | 2.76 × 10⁻⁸ | 6 | 2659.6 |

광범위 sweep 결과, `1.0×10¹³ ~ 2.0×10¹³ cm⁻²` 구간에서는 Dose 증가에 따라 Ioff가 감소하고 Ion/Ioff가 향상되는 경향이 나타났습니다.

반면 `5.0×10¹³ cm⁻²` 이상에서는 다음 문제가 나타났습니다.

- Ion의 급격한 감소
- Subthreshold Swing 증가
- 채널 전위 장벽 증가
- 구동 전류와 스위칭 특성의 동시 열화

따라서 `1.0×10¹³ ~ 2.0×10¹³ cm⁻²` 범위를 정밀 탐색 구간으로 선정했습니다.

<p align="center">
  <img src="./halo-dose-broad-sweep.png"
       width="90%"
       alt="Broad Halo Dose sweep"/>
</p>

<p align="center">
  <sub>1차 Halo Dose sweep을 통해 1.0×10¹³~2.0×10¹³ cm⁻²를 정밀 탐색 구간으로 선정했습니다.</sub>
</p>

---

### 6.2.2 Fine Dose Sweep

1차 탐색에서 선정한 `1.0×10¹³ ~ 2.0×10¹³ cm⁻²` 구간을 세분화하여 추가 sweep을 수행했습니다.

Ion loss는 Halo를 적용하지 않은 baseline의 Ion을 기준으로 다음 식을 사용하여 비교했습니다.

```text
Ion Loss (%) = (Ion,ref − Ion,target) / Ion,ref × 100
```

| Halo Dose | Ion | Ioff | Ion/Ioff | SS | Ion Loss |
| ---: | ---: | ---: | ---: | ---: | ---: |
| 1.0 × 10¹³ cm⁻² | 5.69 × 10⁻⁴ | 1.40 × 10⁻⁵ | 41 | 938.11 | 13.3% |
| 1.3 × 10¹³ cm⁻² | 5.21 × 10⁻⁴ | 1.07 × 10⁻⁵ | 49 | 947.55 | 20.6% |
| **1.5 × 10¹³ cm⁻²** | **4.80 × 10⁻⁴** | **8.86 × 10⁻⁶** | **54** | **954.93** | **26.8%** |
| 1.8 × 10¹³ cm⁻² | 4.08 × 10⁻⁴ | 6.50 × 10⁻⁶ | 63 | 961.14 | 37.8% |
| 2.0 × 10¹³ cm⁻² | 3.56 × 10⁻⁴ | 5.22 × 10⁻⁶ | 68 | 961.64 | 45.7% |

Dose가 증가할수록 Ioff는 감소하고 Ion/Ioff는 증가했지만, 동시에 Ion loss가 커지는 trade-off가 나타났습니다.

따라서 가장 높은 Ion/Ioff를 나타내는 조건을 그대로 선택하지 않고, 누설전류 억제 효과와 구동 전류 손실을 함께 고려하여 `1.5×10¹³ cm⁻²`를 Energy 최적화의 기준 Dose로 선정했습니다.

> **Selected Halo Dose: 1.5 × 10¹³ cm⁻²**

<p align="center">
  <img src="./halo-dose-fine-sweep.png"
       width="90%"
       alt="Fine Halo Dose sweep"/>
</p>

<p align="center">
  <sub>정밀 Dose sweep과 Ion loss 비교를 통해 1.5×10¹³ cm⁻²를 기준 Dose로 선정했습니다.</sub>
</p>

---

## 6.3 Two-Stage Halo Energy Optimization

Halo Dose를 `1.5×10¹³ cm⁻²`로 고정한 뒤, Energy 역시 광범위 탐색과 정밀 탐색의 두 단계로 나누어 분석했습니다.

### 6.3.1 Broad Energy Sweep

먼저 Energy를 5~25 keV 범위에서 탐색했습니다.

| Halo Energy | Ion | Ioff | Ion/Ioff | SS |
| ---: | ---: | ---: | ---: | ---: |
| 5 keV | 5.87 × 10⁻⁴ | 2.22 × 10⁻⁵ | 26 | 958.43 |
| 10 keV | 5.39 × 10⁻⁴ | 1.59 × 10⁻⁵ | 34 | 964.08 |
| 15 keV | 4.80 × 10⁻⁴ | 8.86 × 10⁻⁶ | 54 | 954.93 |
| 20 keV | 4.18 × 10⁻⁴ | 3.15 × 10⁻⁶ | 133 | 894.44 |
| **25 keV** | **3.61 × 10⁻⁴** | **6.08 × 10⁻⁷** | **594** | **839.68** |

20~25 keV 영역에서 Ioff와 SS가 큰 폭으로 개선되는 것을 확인했습니다.

이에 따라 25 keV 인근을 중심으로 Energy를 세분화하여 추가 탐색했습니다.

---

### 6.3.2 Fine Energy Sweep

정밀 Energy sweep은 다음 조건으로 수행했습니다.

```text
25 keV → 26 keV → 28 keV → 31 keV → 35 keV
```

| Halo Energy | Ion | Ioff | Ion/Ioff | SS |
| ---: | ---: | ---: | ---: | ---: |
| 25 keV | 3.61 × 10⁻⁴ | 6.08 × 10⁻⁷ | 594 | 839.7 |
| **26 keV** | **3.53 × 10⁻⁴** | **4.07 × 10⁻⁷** | **867** | **838.3** |
| 28 keV | 3.38 × 10⁻⁴ | 1.72 × 10⁻⁷ | 1,969 | 847.4 |
| 31 keV | 3.21 × 10⁻⁴ | 3.78 × 10⁻⁸ | 8,472 | 875.8 |
| 35 keV | 2.85 × 10⁻⁴ | 1.86 × 10⁻⁹ | 152,951 | 905.0 |

Energy가 증가할수록 Ioff는 감소하고 Ion/Ioff는 증가했지만, 31~35 keV에서는 Ion 감소와 SS 재악화가 나타났습니다.

35 keV는 가장 높은 Ion/Ioff를 나타냈지만, 낮은 Ion과 높은 SS 때문에 균형 잡힌 설계 조건으로 보기 어려웠습니다.

26 keV는 다음 특성을 동시에 나타냈습니다.

- 비교 조건 중 가장 낮은 Subthreshold Swing
- 25 keV보다 낮은 Ioff
- 25 keV보다 높은 Ion/Ioff
- 고에너지 조건보다 작은 Ion 감소
- SS와 leakage 사이의 안정적인 균형

따라서 Ion, Ioff, Ion/Ioff 및 SS를 함께 고려한 **multi-objective compromise point**로 26 keV를 선정했습니다.

<p align="center">
  <img src="./halo-energy-fine-sweep.png"
       width="90%"
       alt="Fine Halo Energy sweep"/>
</p>

<p align="center">
  <sub>가장 높은 Ion/Ioff가 아니라 종합적인 성능 균형을 기준으로 26 keV를 선정했습니다.</sub>
</p>

---

## 6.4 Final Halo Result

### Selected Halo Condition

| Parameter | Selected Value |
| :--- | ---: |
| Halo Dose | 1.5 × 10¹³ cm⁻² |
| Halo Energy | 26 keV |
| Ion | 3.53 × 10⁻⁴ A/μm |
| Ioff | 4.07 × 10⁻⁷ A/μm |
| Ion/Ioff | 867 |
| Subthreshold Swing | 838.3 mV/dec |

### Improvement from Baseline

| Metric | Baseline | Halo Optimized | Change |
| :--- | ---: | ---: | :--- |
| Ion/Ioff | 23 | 867 | Approximately 37.7-fold improvement |
| Subthreshold Swing | 1015.2 mV/dec | 838.3 mV/dec | 17.4% reduction |
| Ioff | 2.81 × 10⁻⁵ A/μm | 4.07 × 10⁻⁷ A/μm | Significant leakage reduction |

Halo 최적화만으로 Ion/Ioff가 약 37.7배 향상되었으며, 이후 LDD 최적화를 수행하기 위한 개선된 공정 기준을 확보했습니다.

<p align="center">
  <img src="./halo-final-comparison.png"
       width="90%"
       alt="Final Halo optimization comparison"/>
</p>

<p align="center">
  <sub>Halo 최적화 전후의 Id–Vg 특성과 Ion/Ioff–SS 변화를 비교했습니다.</sub>
</p>

---

## 6.5 Physical Profile Comparison

Halo implantation 전후의 소자 단면을 비교하여 source와 drain 인근 채널 영역에 국부적인 p-type 도핑 영역이 형성되는 것을 확인했습니다.

이 Halo 영역은 게이트 하부 공핍층의 source–drain 방향 확장을 제한하여 punch-through leakage를 완화하는 역할을 합니다.

<p align="center">
  <img src="./halo-profile-comparison.png"
       width="82%"
       alt="NMOS profile before and after Halo optimization"/>
</p>

<p align="center">
  <sub>Halo implantation 적용 전후의 NMOS 단면 비교</sub>
</p>

> The cross-sectional profiles are presented as a qualitative comparison.  
> The color-scale ranges should be matched for direct quantitative comparison.

---

# 7. LDD Optimization

## 7.1 Engineering Purpose

LDD 구조는 source/drain extension 부근의 전계 집중을 완화하고, 비교적 얕은 접합을 형성하여 Short-Channel Effect와 off-state leakage를 줄이는 데 사용했습니다.

최적 Halo 조건을 고정한 상태에서 LDD Dose와 Energy를 순차적으로 조절했습니다.

---

## 7.2 LDD Dose Sweep

LDD Energy를 5 keV로 고정하고 Dose를 sweep했습니다.

| LDD Dose | Ion | Ioff | Ion/Ioff | SS |
| ---: | ---: | ---: | ---: | ---: |
| 1.5 × 10¹⁴ cm⁻² | 3.15 × 10⁻⁴ | 6.42 × 10⁻¹¹ | 4.90 × 10⁶ | 215.3 |
| **2.0 × 10¹⁴ cm⁻²** | **3.25 × 10⁻⁴** | **6.33 × 10⁻¹¹** | **5.14 × 10⁶** | **204.5** |
| 2.5 × 10¹⁴ cm⁻² | 3.33 × 10⁻⁴ | 1.86 × 10⁻¹⁰ | 1.79 × 10⁶ | 242.0 |
| 3.0 × 10¹⁴ cm⁻² | 3.45 × 10⁻⁴ | 1.70 × 10⁻¹⁰ | 2.02 × 10⁶ | 224.1 |
| 3.5 × 10¹⁴ cm⁻² | 3.52 × 10⁻⁴ | 4.60 × 10⁻¹⁰ | 7.64 × 10⁵ | 273.4 |
| 4.0 × 10¹⁴ cm⁻² | 3.55 × 10⁻⁴ | 4.46 × 10⁻¹⁰ | 7.96 × 10⁵ | 265.2 |

`2.0×10¹⁴ cm⁻²` 조건에서 가장 낮은 SS와 가장 높은 Ion/Ioff가 동시에 나타나 최적 Dose로 선정했습니다.

---

## 7.3 LDD Energy Sweep

LDD Dose를 `2.0×10¹⁴ cm⁻²`로 고정하고 Energy를 3~7 keV 범위에서 조절했습니다.

| LDD Energy | Ion | Ioff | Ion/Ioff | SS |
| ---: | ---: | ---: | ---: | ---: |
| 3 keV | 3.24 × 10⁻⁴ | 2.04 × 10⁻¹⁰ | 1.59 × 10⁶ | 263.4 |
| 4 keV | 3.32 × 10⁻⁴ | 9.38 × 10⁻¹¹ | 3.54 × 10⁶ | 212.3 |
| **5 keV** | **3.25 × 10⁻⁴** | **6.33 × 10⁻¹¹** | **5.14 × 10⁶** | **204.5** |
| 6 keV | 3.34 × 10⁻⁴ | 1.94 × 10⁻¹⁰ | 1.72 × 10⁶ | 242.6 |
| 7 keV | 3.41 × 10⁻⁴ | 6.05 × 10⁻¹⁰ | 5.65 × 10⁵ | 309.2 |

5 keV 조건은 비교적 얕은 접합을 형성하면서 off-state leakage와 SS를 동시에 낮춰 최적 Energy로 선정했습니다.

### Selected LDD Condition

| Parameter | Selected Value |
| :--- | ---: |
| LDD Dose | 2.0 × 10¹⁴ cm⁻² |
| LDD Energy | 5 keV |
| Ion | 3.25 × 10⁻⁴ A/μm |
| Ioff | 6.33 × 10⁻¹¹ A/μm |
| Ion/Ioff | 5.14 × 10⁶ |
| Subthreshold Swing | 204.5 mV/dec |

---

# 8. P-well Refinement

Halo와 LDD 최적화 이후에도 남아 있는 source–drain punch-through leakage를 추가로 억제하기 위해 p-type 영역의 농도를 조절했습니다.

P-well 농도를 `7.0×10¹⁷ cm⁻³`으로 설정하여 채널 하부 공핍층의 확장을 억제하고 고온 off-state 누설전류를 추가로 낮췄습니다.

## Final Integrated Recipe

| Process Parameter | Final Value | Engineering Role |
| :--- | ---: | :--- |
| Gate Length | 0.12 μm | High-temperature short-channel baseline |
| Temperature | 423 K | Automotive high-temperature condition |
| Halo Dose | 1.5 × 10¹³ cm⁻² | Punch-through 및 공핍층 확장 억제 |
| Halo Energy | 26 keV | Halo 침투 깊이 조절 |
| LDD Dose | 2.0 × 10¹⁴ cm⁻² | Source/Drain 부근 전계 완화 |
| LDD Energy | 5 keV | Shallow junction 형성 |
| P-well Concentration | 7.0 × 10¹⁷ cm⁻³ | 고온 off-state leakage 추가 억제 |

<p align="center">
  <img src="./단면.png"
       width="72%"
       alt="Final NMOS cross-section"/>
</p>

<p align="center">
  <img src="./최종1.png"
       width="46%"
       alt="Final optimized NMOS result 1"/>
  <img src="./최종2.png"
       width="46%"
       alt="Final optimized NMOS result 2"/>
</p>

---

# 9. Final Results

## 9.1 Performance by Optimization Stage

| Optimization Stage | SS | Ioff | Ion/Ioff |
| :--- | ---: | ---: | ---: |
| Baseline | 1015.2 mV/dec | 2.81 × 10⁻⁵ A/μm | 23 |
| Halo Optimization | 838.3 mV/dec | 4.07 × 10⁻⁷ A/μm | 867 |
| LDD Optimization | 204.5 mV/dec | 6.33 × 10⁻¹¹ A/μm | 5.14 × 10⁶ |
| P-well Refinement | **198.6 mV/dec** | **1.58 × 10⁻¹² A/μm** | **1.39 × 10⁸** |

## 9.2 Improvement over Baseline

| Metric | Baseline | Final Device | Improvement |
| :--- | ---: | ---: | :--- |
| Subthreshold Swing | 1015.2 mV/dec | 198.6 mV/dec | 80.4% 감소 |
| Ioff | 2.81 × 10⁻⁵ A/μm | 1.58 × 10⁻¹² A/μm | 약 7 orders 감소 |
| Ion/Ioff | 23 | 1.39 × 10⁸ | 약 6 × 10⁶배 향상 |

## Optimization Trajectory

```text
Baseline
SS       = 1015.2 mV/dec
Ioff     = 2.81 × 10⁻⁵ A/μm
Ion/Ioff = 23
        |
        v
Halo Optimization
SS       = 838.3 mV/dec
Ioff     = 4.07 × 10⁻⁷ A/μm
Ion/Ioff = 867
        |
        v
LDD Optimization
SS       = 204.5 mV/dec
Ioff     = 6.33 × 10⁻¹¹ A/μm
Ion/Ioff = 5.14 × 10⁶
        |
        v
P-well Refinement
SS       = 198.6 mV/dec
Ioff     = 1.58 × 10⁻¹² A/μm
Ion/Ioff = 1.39 × 10⁸
```

---

# 10. Technical Interpretation

## 10.1 Halo Doping

Halo implantation은 게이트 하부와 source/drain 인근의 공핍층 확장을 억제하여 punch-through leakage를 줄였습니다.

다만 Halo Dose 또는 Energy가 과도하게 증가하면 채널 전위 장벽이 높아져 Ion이 감소했습니다. 따라서 가장 낮은 Ioff 또는 가장 높은 Ion/Ioff만을 선택하지 않고, SS와 구동 전류를 함께 고려해야 했습니다.

## 10.2 LDD Engineering

LDD implantation은 source/drain junction 인근의 전계 집중을 완화했습니다.

특히 5 keV의 저에너지 조건은 상대적으로 얕은 접합을 형성하여 Short-Channel Effect 억제에 유리했습니다. LDD 최적화 단계에서 SS와 Ioff가 가장 큰 폭으로 개선되었습니다.

## 10.3 P-well Concentration

P-well 농도 증가는 채널 하부 공핍층의 확장을 제한하고 source와 drain 사이의 punch-through 경로를 추가로 억제했습니다.

그 결과 최종 Ioff를 `1.58×10⁻¹² A/μm`까지 감소시킬 수 있었습니다.

## 10.4 Multi-Objective Trade-off

본 프로젝트에서는 Ion/Ioff 하나만을 최대화하는 조건이 반드시 가장 적절한 설계 조건은 아니라는 점을 확인했습니다.

실제 공정 조건을 선정할 때는 다음 지표를 함께 고려해야 합니다.

- Drive current
- Off-state leakage
- Subthreshold Swing
- Ion/Ioff ratio
- Junction depth
- Electric-field distribution
- Channel potential barrier

---

# 11. Implementation Highlights

## 11.1 Parameterized TCAD Workflow

Sentaurus Workbench parameter를 이용하여 다음 조건을 자동 sweep할 수 있도록 구성했습니다.

```text
Lg
Temp
Vd
Halo_Dose
Halo_Energy
LDD_Dose
LDD_Energy
NWell
GOxTime
RTA
```

## 11.2 Dual-Angle Halo Implantation

게이트 양쪽에 대칭적인 Halo profile을 형성하기 위해 rotation 0°와 180° 조건을 적용했습니다.

```tcl
implant BF2 dose=@Halo_Dose@ energy=@Halo_Energy@ tilt=30 rotation=0
implant BF2 dose=@Halo_Dose@ energy=@Halo_Energy@ tilt=30 rotation=180
```

## 11.3 Temperature Parameterization

동일한 소자 구조에서 온도만 변경하여 비교할 수 있도록 Sdevice의 구동 온도를 parameter로 구성했습니다.

```tcl
Physics {
   Temperature = @Temp@
   EffectiveIntrinsicDensity(OldSlotboom)
}
```

## 11.4 Automated Electrical Extraction

Svisual을 통해 Id–Vg curve에서 다음 전기적 특성을 자동 추출했습니다.

- Threshold Voltage, Vtgm
- On Current, Ion
- Off Current, Ioff
- Subthreshold Swing, SS
- Transconductance, gm

---

# 12. Documentation

| Document | Description | Link |
| :--- | :--- | :---: |
| Final Technical Report | 전체 시뮬레이션 조건, parameter split 및 결과 분석 | [PDF](./automotive-nmos-final-report.pdf) |
| Final Presentation | 프로젝트 배경, 최적화 과정 및 최종 결과 요약 | [PDF](./automotive-nmos-presentation.pdf) |

---

# 13. Source Code

## 13.1 Sprocess

<details>
<summary><strong>Open Sprocess Code</strong></summary>

```tcl
#============================================================
# Automotive NMOS Process Simulation
# Synopsys Sentaurus Sprocess
#============================================================

#-------------------------
# Mesh Definition
#-------------------------
line x location=0.0  spacing=0.01 tag=top
line x location=0.15 spacing=0.02
line x location=1.0  spacing=0.2  tag=bottom

line y location=0.0       spacing=0.1*@Lg@  tag=left
line y location=0.5*@Lg@  spacing=0.05*@Lg@
line y location=2.0*@Lg@  spacing=@Lg@      tag=right

region Silicon xlo=top xhi=bottom ylo=left yhi=right

#-------------------------
# Initial P-type Region
#-------------------------
# Legacy Workbench parameter name: NWell
# Physical meaning: Boron-based P-well/substrate concentration
init concentration=@NWell@ field=Boron slice.angle=180 !DelayFullD

# Use Charged-Fermi diffusion model
pdbSet Silicon Dopant DiffModel ChargedFermi

#-------------------------
# Gate Oxidation
#-------------------------
pdbSet Oxide Grid perp.add.dist 0.01e-4
diffuse time=@GOxTime@ temperature=950 O2

# Extract oxide thickness
set oxidelayer [lindex [layers y=0 Oxide] 1]

puts "DOE: tox [format %.4f \
  [expr [lindex $oxidelayer 1] - [lindex $oxidelayer 0]]]"

#-------------------------
# Poly-Silicon Gate
#-------------------------
deposit PolySilicon type=anisotropic thickness=0.1

mask name=poly left=-@Lg@/2 right=@Lg@/2

etch PolySilicon type=anisotropic thickness=0.12 mask=poly
etch Oxide       type=anisotropic thickness=0.02

#-------------------------
# LDD Implantation
#-------------------------
implant Arsenic dose=@LDD_Dose@ energy=@LDD_Energy@

#-------------------------
# Dual-Angle Halo Implantation
#-------------------------
implant BF2 dose=@Halo_Dose@ energy=@Halo_Energy@ \
  tilt=30 rotation=0

implant BF2 dose=@Halo_Dose@ energy=@Halo_Energy@ \
  tilt=30 rotation=180

#-------------------------
# Spacer Formation
#-------------------------
deposit Nitride type=isotropic thickness=0.3*@Lg@
etch Nitride type=anisotropic thickness=0.35*@Lg@

#-------------------------
# Source/Drain Implantation
#-------------------------
implant Phosphorus dose=1e15 energy=15

# Rapid Thermal Annealing
diffuse time=@RTA@<s> temperature=1000

#-------------------------
# Contact Formation
#-------------------------
deposit Aluminum type=anisotropic thickness=0.05

mask name=contact left=@Lg@*1.2
etch Aluminum type=anisotropic thickness=0.1 mask=contact

# Reflect the half structure
transform reflect left

#-------------------------
# Electrode Definition
#-------------------------
contact name=substrate bottom
contact name=source point y=-@Lg@*1.5 x=-0.010 replace
contact name=drain  point y= @Lg@*1.5 x=-0.010 replace
contact name=gate   point y=0          x=-0.050

#-------------------------
# Save Structure
#-------------------------
struct tdr=n@node@ !Gas !interfaces
```

</details>

---

## 13.2 Sdevice

<details>
<summary><strong>Open Sdevice Code</strong></summary>

```tcl
#============================================================
# Automotive NMOS Device Simulation
# Synopsys Sentaurus Sdevice
#============================================================

File {
   Grid    = "@tdr@"
   Plot    = "@tdrdat@"
   Current = "@plot@"
   Output  = "@log@"
}

Electrode {
   { Name="source"    Voltage=0.0 }
   { Name="drain"     Voltage=0.0 }
   { Name="gate"      Voltage=0.0 }
   { Name="substrate" Voltage=0.0 }
}

Physics {
   # Parameterized device operating temperature
   Temperature = @Temp@

   EffectiveIntrinsicDensity(
      OldSlotboom
   )
}

Physics(Material="Silicon") {
   Mobility(
      PhuMob
      HighFieldSaturation
      Enormal
   )

   Recombination(
      SRH(DopingDependence)
   )
}

Math {
   Extrapolate
   Iterations=20
   ExitOnFailure
}

Solve {
   # Initial Poisson solution
   Coupled(Iterations=100) {
      Poisson
   }

   # Initial carrier solution
   Coupled {
      Poisson
      Electron
      Hole
   }

   # Drain-voltage ramp
   Quasistationary(
      InitialStep=0.1
      Increment=1.5
      MinStep=1e-5
      MaxStep=1
      Goal {
         Name="drain"
         Voltage=@Vd@
      }
   ) {
      Coupled {
         Poisson
         Electron
         Hole
      }
   }

   # Gate-voltage sweep
   NewCurrentPrefix="IdVg_"

   Quasistationary(
      DoZero
      InitialStep=0.01
      Increment=1.5
      MinStep=1e-5
      MaxStep=0.05
      Goal {
         Name="gate"
         Voltage=2.5
      }
   ) {
      Coupled {
         Poisson
         Electron
         Hole
      }
   }
}
```

</details>

---

## 13.3 Svisual

<details>
<summary><strong>Open Svisual Code</strong></summary>

```tcl
#============================================================
# Id-Vg Visualization and Parameter Extraction
# Synopsys Sentaurus Visual
#============================================================

#setdep @node|sdevice@

set n @node|sdevice@

# Determine the Sdevice node status
set status @[gproject::GetNodeStatus @node|sdevice@]@

#-------------------------
# Plot Configuration
#-------------------------
if {[llength [list_plots Plot_1D]] == 0} {
   create_plot -1d -name Plot_1D
   select_plots Plot_1D

   set_plot_prop \
      -hide_title \
      -show_legend

   set_axis_prop \
      -title_font_size 16 \
      -scale_font_size 14

   set_axis_prop \
      -axis x \
      -title "Gate Voltage (V)" \
      -type linear

   set_axis_prop \
      -axis y \
      -title "Drain Current (A/<greek>m</greek>m)" \
      -type log

   set_legend_prop \
      -label_font_size 14 \
      -location bottom_right
}

#-------------------------
# Load and Create Id-Vg Curve
#-------------------------
load_file @[relpath IdVg_n@node|sdevice@_des.plt]@ \
   -name PLT($n)

create_curve \
   -name IdVg($n) \
   -dataset PLT($n) \
   -axisX "gate InnerVoltage" \
   -axisY "drain TotalCurrent"

#-------------------------
# Electrical Parameter Extraction
#-------------------------
if {$status == "done"} {
   load_library extract

   set Vgs [get_variable_data \
      "gate OuterVoltage" \
      -dataset PLT($n)]

   set Ids [get_variable_data \
      "drain TotalCurrent" \
      -dataset PLT($n)]

   ext::ExtractVtgm \
      out=Vtgm \
      name=Vtgm \
      v=$Vgs \
      i=$Ids

   ext::ExtractExtremum \
      out=Ion \
      name=Ion \
      x=$Vgs \
      y=$Ids \
      type=max

   ext::ExtractExtremum \
      out=Ioff \
      name=Ioff \
      x=$Vgs \
      y=$Ids \
      type=min

   ext::ExtractSS \
      out=SS \
      name=SS \
      v=$Vgs \
      i=$Ids \
      vo=[expr $Vtgm/3.0]

   ext::ExtractGm \
      out=gm \
      name=gm \
      v=$Vgs \
      i=$Ids
}

#-------------------------
# Curve Styling
#-------------------------
if {[info exists runVisualizerNodesTogether]} {
   set_curve_prop IdVg($n) \
      -label "IdVg $legend" \
      -color $color \
      -line_style $line \
      -line_width 3
} else {
   puts "To compare the curves, select multiple Sentaurus Visual nodes"
   puts "and run \"Run Selected Visualizer Nodes Together\"."
}
```

</details>

---

# 14. Scope and Limitations

- 본 프로젝트의 결과는 **Synopsys Sentaurus TCAD 시뮬레이션 결과**이며 실제 제작 소자의 측정 결과가 아닙니다.
- 423 K는 차량용 반도체의 고온 동작 환경을 모사하기 위해 설정한 조건입니다.
- 본 프로젝트는 실제 차량용 반도체 인증이나 신뢰성 시험을 수행한 것이 아닙니다.
- 최적화는 모든 공정 변수의 전역 탐색이 아닌 Sequential Split 방식으로 진행했습니다.
- 최종 결과는 사용한 물리 모델, mesh, bias 및 parameter extraction 기준에 따라 달라질 수 있습니다.
- 변수 간 상호작용을 정밀하게 분석하려면 full-factorial DOE 또는 response-surface analysis가 추가로 필요합니다.
- 기존 코드의 `NWell` parameter는 물리적으로 Boron 기반 P-well 농도를 의미하며, 향후 명칭 정리가 필요합니다.

---

# Key Takeaway

본 프로젝트를 통해 고온과 채널 미세화가 동시에 적용될 경우 NMOS의 off-state leakage와 Subthreshold Swing이 급격히 악화될 수 있음을 확인했습니다.

또한 Halo, LDD, P-well 공정 변수를 단계적으로 조절하며 각 공정이 전계 분포, 공핍층 확장, punch-through 및 구동 전류에 미치는 영향을 분석했습니다.

최종적으로 423 K, Gate Length 0.12 μm 조건에서:

- Subthreshold Swing 80.4% 감소
- Ioff 약 7 orders 감소
- Ion/Ioff 23에서 1.39 × 10⁸로 회복

이라는 결과를 얻었습니다.

특히 Halo 최적화를 통해, 단순히 누설전류를 최소화하거나 Ion/Ioff를 최대화하는 것보다 **구동 전류와 게이트 제어력까지 함께 고려한 multi-objective process optimization이 중요하다**는 점을 확인했습니다.

---

## References

1. 반도체집적공정 강의자료, 숭실대학교
2. Synopsys Sentaurus TCAD Manual and Example Projects
3. Project Final Technical Report
4. Project Final Presentation
