# 5G NR 장별 요약 노트

> 교재: Dahlman · Parkvall · Sköld, *5G NR: The Next Generation Wireless Access Technology* (Rel-15 기준)
> 정리: 장별 핵심 · 외울 숫자 · LTE 대비 변경점 · 책 이후 업데이트 · 연구 연결 · 원문 오류

## 목차

- [책 전체를 관통하는 세 가지 결정](#책-전체를-관통하는-세-가지-결정)
- [1장 What Is 5G?](#1장-what-is-5g)
- [2장 5G 표준화](#2장-5g-표준화)
- [3장 5G 스펙트럼](#3장-5g-스펙트럼)
- [4장 LTE 개요](#4장-lte-개요)
- [5장 NR 개요](#5장-nr-개요)
- [6장 무선 인터페이스 구조](#6장-무선-인터페이스-구조)
- [7장 전체 전송 구조](#7장-전체-전송-구조)
- [8장 채널 사운딩](#8장-채널-사운딩)
- [9장 전송채널 처리](#9장-전송채널-처리)
- [10장 물리계층 제어 시그널링](#10장-물리계층-제어-시그널링)
- [11장 다중안테나 전송](#11장-다중안테나-전송)
- [12장 빔 관리](#12장-빔-관리)
- [13장 재전송 프로토콜](#13장-재전송-프로토콜)
- [14장 스케줄링](#14장-스케줄링)
- [15장 상향 전력·타이밍 제어](#15장-상향-전력타이밍-제어)
- [16장 초기 접속](#16장-초기-접속)
- [17장 LTE/NR 연동과 공존](#17장-ltenr-연동과-공존)
- [18장 RF 특성](#18장-rf-특성)
- [19장 mmWave RF 기술](#19장-mmwave-rf-기술)
- [20장 첫 release 이후](#20장-첫-release-이후)
- [부록 A. 연구 연결 포인트 모음](#부록-a-연구-연결-포인트-모음)
- [부록 B. 원문 오류 목록](#부록-b-원문-오류-목록)

---

## 책 전체를 관통하는 세 가지 결정

1. **하위 호환성을 버리고 forward compatibility를 택함** — 고정 위치 신호를 최소화하고 예약 자원으로 미래 확장 여지를 남김.
2. **상시 신호 제거(ultra-lean)** — CRS를 없애고 용도별 신호(DMRS, CSI-RS, TRS)로 분리. 상시 신호는 SSB 하나.
3. **모든 계층의 설계 기준이 저지연** — 미니슬롯, front-loaded DMRS, 주파수 우선 매핑, RLC 연접·순서 보장 제거, MAC 헤더 재배치, 짧은 처리 시간.

### LTE 한계 → NR 대응 (5~19장을 읽는 기준표)

| LTE Rel-8 설계 | 그렇게 한 이유 | 생긴 한계 | NR의 대응 |
|---|---|---|---|
| 15 kHz 고정, CP 4.7 µs | 3 GHz 이하 매크로셀 | mmWave·저지연 부적합 | 가변 numerology (15~240 kHz) |
| 1 ms 서브프레임이 최소 스케줄링 단위 | MBB에 충분 | 지연 하한 | 슬롯 + 미니슬롯 |
| CRS 상시 전송 | 큰 셀, 다수 사용자 | 에너지 효율 상한, 빈 셀 간섭 | ultra-lean, DMRS는 데이터 있을 때만 |
| PDCCH 전 대역 | 주파수 다이버시티 | 모든 단말이 20 MHz 지원 필수 | CORESET, BWP |
| PCFICH로 제어 영역 가변 | — | 파이프라인 처리 어려움 | CORESET 크기 고정 |
| Turbo 부호 | 당시 기술 | 고부호율 복잡도 | 데이터 LDPC, 제어 Polar |
| 동기식 상향 HARQ, RTT 8 ms | — | 동적 TDD 불가, 지연 | 비동기 HARQ, 가변 K1 |
| 정적 TDD | 매크로 간섭 회피 | 트래픽 추종 불가 | 동적 TDD 기본 |
| 상향 DFT-s-OFDM만 | 낮은 PAPR | 상향 MIMO 수신 복잡 | CP-OFDM 기본 + DFT-s 보조 |
| 코드북 빔포밍(최대 4안테나) | — | — | 빔 관리 체계 신설 |

---

## 1장 What Is 5G?

### 세대별 흐름

| 세대 | 시기 | 대표 기술 | 바뀐 것 |
|---|---|---|---|
| 1G | 1980년경 | AMPS, NMT, TACS | 아날로그 음성, 대중화 시작 |
| 2G | 1990년대 초 | GSM, D-AMPS, PDC, IS-95 | 디지털화, 제한적 데이터. GSM 독식 |
| 3G | 2000년대 초 | WCDMA(HSPA), cdma2000, TD-SCDMA | 모바일 브로드밴드, **TDD 첫 등장** |
| 4G | 2009~ | LTE | OFDM, **FDD/TDD를 단일 RAT로 통합**, 세계 단일 기술로 수렴 |

- FDD = 페어 스펙트럼(상하향 다른 주파수), TDD = 비페어 스펙트럼(같은 주파수를 시간으로 분할).

### 3GPP
- 핵심 논리: **시장 규모가 단말 가격을 결정한다** (NMT → GSM → 3G 글로벌화).
- 1998년 지역 SDO들이 WCDMA 완성을 위해 3GPP 결성. IS-95 진영은 3GPP2(cdma2000) → 결국 3GPP만 남음.

### 핵심 주장: 5G는 RAT가 아니라 사용 사례로 정의된다
- **5G = LTE 진화 + NR**.
- NR이 필요한 이유: LTE로 못 맞추는 요구 + 10년간 축적된 신기술.
- **NR은 하위 호환성 제약이 없다** — numerology, 프레임 구조가 LTE와 달라지는 근본 이유.
- 일정: 2015 가을 워크숍 → 2016 봄 기술 작업 → 2017 말 첫 규격(NSA).

### 세 가지 사용 사례

| 분류 | 극단으로 미는 지표 | 덜 중요한 것 |
|---|---|---|
| eMBB | 전송률, 용량 | — |
| mMTC | 단말 수, 가격, 소비전력(배터리 수년) | 전송률 |
| URLLC | 지연, 신뢰도 | 대량 전송 |

- 이 분류는 **요구사항 정의용 단순화**. **신뢰도와 지연은 독립된 축**이다(책이 직접 짚음).

### 5GCN
- NR은 5GCN에 붙는 게 기본. EPC에 LTE와 묶여 붙는 방식이 **NSA**.
- 실제 Rel-15 동결: NSA 2017.12, SA 2018.06.

---

## 2장 5G 표준화

### 조직 지도

| 종류 | 성격 | 예시 | 역할 |
|---|---|---|---|
| SDO | 비영리 산업 단체 | 3GPP (ETSI, ARIB, TTC, ATIS, CCSA, TTA, TSDSI) | 상호운용 규격 작성 |
| 규제기관 | 정부 주도 | 각국, CEPT/ECC, CITEL, APT, ITU | 스펙트럼, 면허, 형식승인 |
| 산업 포럼 | 업계 단체 | GSMA, NGMN, 5G Americas | 요구사항 제시, 로비 |

- 3GPP 문서는 비회원도 전부 무료 열람 가능.
- ETSI는 EU 위임으로 CE 인증용 harmonized standard까지 작성.

### ITU-R
- **WRC**: 3~4년마다, Radio Regulations(구속력 있는 조약) 개정.
- **WP5D**: IMT 담당. 규격을 쓰지 않고 SDO 규격을 참조 목록(**RSPC**)으로 묶음.
- RSPC: IMT-2000 **M.1457**, IMT-Advanced **M.2012**, IMT-2020 (실제로는 **M.2150**, 2021.02).
- RIT = 단일 무선 인터페이스, SRIT = RIT 묶음.
- LTE-Advanced = Rel-10에 ITU 제출용으로 붙인 이름. Rel-13부터 LTE-Advanced Pro.

### IMT-2020 문서 체계

| 단계 | 문서 | 내용 |
|---|---|---|
| 비전 | **M.2083** | 사용 시나리오 3개, 역량 13개(핵심 8개) |
| 최소 요구사항 | **M.2410** | 표 2.1 |
| 평가 방법 | **M.2412** | 시험 환경 5개, 채널 모델 |
| 제출 양식 | **M.2411** | 템플릿 |
| 3GPP 대응 | **TR 38.913** | ITU보다 넓고 엄격할 수 있음 |

### 핵심 역량 8개 (M.2083 목표치)

| 역량 | 목표 | IMT-Adv 대비 |
|---|---|---|
| 최대 전송률 | 20 Gbit/s | 1 → 20배 |
| 체감 전송률 | 100 Mbit/s (실내 1 Gbit/s) | 10 → 10배 |
| 스펙트럼 효율 | 3배 | — |
| 이동성 | 500 km/h | 350 |
| 지연 | 1 ms | 10 → 1/10 |
| 연결 밀도 | 10⁶/km² | 10배 |
| 에너지 효율 | 100배 | — |
| 면적 용량 | 10 Mbit/s/m² | 100배 |

- 최대 전송률 = 최대 스펙트럼 효율 × 대역폭 → 사실상 스펙트럼 문제(학술적 지표).
- 면적 용량 = 스펙트럼 효율 × 대역폭 × TRP 밀도.
- 나머지 5개(유연성, 신뢰도, 복원력, 보안, 운용 수명)는 정량화가 어려워서 "핵심"에서 빠졌을 뿐.

### 최소 요구사항 (M.2410) — 비전과 달라진 점 위주

| 항목 | 값 |
|---|---|
| 최대 전송률 | DL 20 / UL 10 Gbit/s |
| 최대 스펙트럼 효율 | DL 30 / UL 10 bit/s/Hz |
| 체감 전송률 | DL 100 / UL 50 Mbit/s |
| 사용자 평면 지연 | **eMBB 4 ms / URLLC 1 ms** |
| 제어 평면 지연 | 20 ms (신규) |
| 연결 밀도 | 10⁶/km² |
| 에너지 효율 | 정성적(sleep 비율·지속) → inspection |
| 신뢰도 | **32바이트 L2 PDU, 1 ms 내, 1−10⁻⁵, Urban Macro 셀 경계** |
| 이동성 | 500 km/h |
| 단절 시간 | **0 ms** (신규) |
| 대역폭 | 최소 100 MHz, 고주파 최대 1 GHz |

- 평가 방법: **시뮬레이션**(스펙트럼 효율, 연결 밀도, 이동성, 신뢰도) / **분석**(전송률, 면적 용량, 지연, 단절 시간) / **검토**(대역폭, 에너지 효율, 서비스·대역 지원).
- 시험 환경 5개: Indoor Hotspot-eMBB, Dense Urban-eMBB, Rural-eMBB, Urban Macro-mMTC, Urban Macro-URLLC.

### 3GPP 구조와 일정
- 4단계(요구사항 → 아키텍처 → 상세 규격 → 시험) 반복. 규격 완료 후 상용까지 약 1년.
- RAN WG1 물리계층 / WG2 L2·L3 / WG3 RAN 인터페이스 / WG4 RF·RRM / WG5 적합성 / WG6 GSM·HSPA(이후 폐지).
- 규격 번호: 25 WCDMA, 45 GSM, 36 LTE, 37 multi-RAT, **38 NR**.
- 로밍 단말은 가장 엄격한 지역 요구를 만족해야 → 지역 옵션은 주로 기지국 쪽.
- NR: Rel-14 study → Rel-15 work item. 2018.02 ITU 제출(3GPP SRIT+RIT, 한국·중국 RIT).

---

## 3장 5G 스펙트럼

- 저주파 = 커버리지, 고주파 = 용량. 이전 세대 대역은 재배치(re-farming)로 재사용.
- ITU IMT 지정은 **세대 중립** → 기존 IMT 대역 전부 5G 후보.

### WRC 연혁

| 회의 | 지정 대역 | 의미 |
|---|---|---|
| WARC-92 | 1885–2025, 2110–2200 MHz | IMT-2000 코어 밴드 |
| WRC-2000 | 806–960, 1710–1885, 2500–2690 MHz | 2G 대역 편입 |
| WRC-07 | 450–470, 698–806, 2300–2400, 3400–3600 MHz | IMT-Adv 포함 |
| WRC-15 | 600/700 MHz, L-band, **3300–3700**, 4800–4990 MHz | 중대역 5G 근거 |
| WRC-19 (의제 1.13) | 24.25–27.5, 37–43.5, 45.5–47, 47.2–48.2, 66–71 GHz | mmWave 근거 |

### 저·중·고 대역

| 구분 | 범위 | 대표 밴드 | 채널 폭 | 역할 |
|---|---|---|---|---|
| 저 | < 2 GHz | n71, n28 | ≤ 20 MHz | 커버리지 |
| 중 | 3–6 GHz | **n77, n78**, n79 | ≤ 100 MHz | 균형 |
| 고 | > 24 GHz | **n257, n258**, n259 | ≤ 400 MHz | 핫스팟 |

- 3.3–5 GHz가 핵심인 이유: 넓은 블록 할당 가능 + Massive MIMO 배열을 현실적 크기로.
- 한국: 2018년 3.5 GHz(n78)·28 GHz(n257) 경매, 28 GHz는 이후 할당 취소.

### 3GPP 밴드 체계
- **FR1** < 6 GHz(이후 7.125 GHz), **FR2** 24.25–52.6 GHz(Rel-17에서 FR2-2 52.6–71 GHz 추가).
- FR을 나눈 이유는 물리계층이 아니라 **RF 요구·측정 방식(OTA)** 차이.
- 번호: 재배치 대역은 LTE 번호 + "n", FR1 신규 n65–n256, FR2 신규 n257–n512.
- SDL/SUL: SUL은 고주파 TDD의 상향 커버리지 보완.
- Rel-15: FR1 26개, FR2 3개 밴드.

### RF 노출
- 6–10 GHz에서 기준이 SAR → 전력 밀도로 바뀌며 단말 출력이 최대 10 dB 깎일 수 있다는 문제 제기. 이후 ICNIRP 2020, IEEE C95.1-2019에서 상당 부분 완화.

---

## 4장 LTE 개요

### Rel-8 수치
- 하향 150 Mbit/s(2레이어, 20 MHz), 상향 75 Mbit/s.
- HARQ RTT 8 ms, RAN 단방향 < 5 ms, 실망 종단 간 약 10 ms.
- 20 MHz에 부반송파 1200개(점유율 90%).

### Release 연혁

| Release | 완료 | 핵심 |
|---|---|---|
| 8 | 2008 | 기본 LTE, EPC |
| 10 (LTE-A) | 2010 | CA 5 CC/100 MHz, DL 8·UL 4레이어, 릴레이, CSI-RS |
| 11 | 2012 | CoMP, EPDCCH |
| 12 | 2014 | **DC**, small-cell on/off, eIMTA, **D2D**, Cat-0 |
| 13 (LTE-A Pro) | 2015 | **LAA**, CA 32 CC/640 MHz, FD-MIMO, Cat-M1, **NB-IoT** |
| 14 | 2017 | **V2V/V2X**, 상향 LAA |
| 15 | 2018 | **sTTI**, 드론 |

- 공통 패턴: **하위 호환성이 진화 방식을 결정** (CC는 Rel-8 구조, 릴레이는 일반 셀처럼 보임, sTTI는 제약 큼).

### 주요 기능
- **CA**: 32 CC로 이론상 약 25 Gbit/s → IMT-2020 최대 전송률 수치를 넘김(최대 전송률이 스펙트럼 의존 지표라는 증거).
- **LAA**: 비면허 5 GHz를 면허 앵커에 CA, LBT로 Wi-Fi 공존. 단독 운용은 MulteFire.
- **CSI-RS 분리**(Rel-10): NR 기준신호 철학의 출발점.
- **CoMP**: CS/CB, DPS, JT. Rel-11 이상적 백홀 → Rel-12 비이상 백홀.
- **DC**: 비이상 백홀 CA → **NR NSA(EN-DC)의 기반**.
- **Small-cell on/off**: CRS 상시 전송 문제의 땜질 → ultra-lean의 동기.
- **MTC**: Cat-0(1 Mbit/s) → Cat-M1(1.4 MHz) → NB-IoT(180 kHz, ≤250 kbit/s).

### V2X 계보
- D2D(Rel-12, 공공안전용) → LTE-V2X(Rel-14) → NR V2X(Rel-16).
- LTE 사이드링크(PC5): **브로드캐스트 전용, HARQ 피드백 없음**, 블라인드 재전송.
- 자원 할당: mode 3(기지국 지정) / mode 4(센싱 기반 SPS 자율) → NR에서 mode 1/2.

---

## 5장 NR 개요

### 설계 원칙 3 + 기술 2

| 구분 | 항목 | 해결하는 LTE 문제 | 수단 |
|---|---|---|---|
| 원칙 | 고주파·스펙트럼 유연성 | 3 GHz 이하 설계 | 1 GHz 이하~52.6 GHz |
| 원칙 | Ultra-lean | CRS 상시 전송 | CRS 제거, SSB 20 ms |
| 원칙 | Forward compatibility | 고정 위치 신호 | 고정 자원 최소화, 예약 자원 |
| 기술 | 저지연 | 1 ms TTI, 느린 처리 | front-loaded DMRS, 미니슬롯, 빠른 ACK |
| 기술 | 빔 중심 | 데이터 채널만 빔포밍 | 동기·제어까지 빔 스윕, 빔 복구 |

- Forward compatibility 3원칙(3GPP 합의): 유연하게 쓰거나 비울 수 있는 자원 극대화 / 상시 신호 최소화 / 기능을 설정 가능한 자원 안에 가두기.

### 저지연 메커니즘
- front-loaded DMRS·제어, 심볼 간 인터리빙 없음 → 버퍼링 없이 즉시 복호.
- HARQ-ACK 약 1슬롯, grant → PUSCH도 비슷.
- **Self-contained slot**: short PUCCH로 같은 슬롯 안 ACK (LTE 약 3 ms).
- MAC/RLC 헤더 재설계, configured grant, 선점 + CBG 재전송.

### 기타
- 부호: 데이터 LDPC, 제어 Polar, 초소형 제어 Reed-Muller.
- MIMO: DL 8레이어, UL 4레이어, 직교 DMRS 12개, PTRS 신규.
- 초기 접속: LTE 5 ms·반송파 중앙·100 kHz 래스터 → NR 20 ms·희소 동기 래스터(SSB가 중앙이 아닐 수 있음).
- LTE 공존 시나리오: 상하향 공존 / 상향만 공존(SUL).
- 2017.12 규격은 **NSA만**. SA·NR-NR DC는 2018.06.

---

## 6장 무선 인터페이스 구조

### 코어망
- 5GC 3대 특징: **SBA, 네트워크 슬라이싱, CUPS**.

| 기능 | 역할 | EPC 대응 |
|---|---|---|
| UPF | 사용자 평면 게이트웨이 | S/P-GW 사용자 평면 |
| AMF | NAS, 보안, idle 이동성 | MME |
| SMF | IP 할당, 세션 | MME 일부 + P-GW 제어 |

- 배치 옵션: **3(EN-DC)** EPC+eNB 앵커+gNB / 3a / 3x, **2(SA)** 5GC+gNB, 4·5·7(LTE를 5GC에).
- 3G→4G는 교차 연결 불가, 4G↔5G는 가능.

### RAN
- gNB는 논리 노드(물리 기지국과 다름).
- 인터페이스: NG-u(UPF), NG-c(AMF), **Xn**(gNB 간), **F1**(CU-DU), Uu(무선).
- **CU: RRC·PDCP·SDAP / DU: RLC·MAC·PHY**.

### QoS
- PDU 세션 → QoS flow(QFI) → DRB. **서비스를 아는 건 코어뿐**, RAN은 flow→베어러 매핑만.
- 상향 매핑: reflective(NR 신규) / RRC 명시.

### 사용자 평면 스택 — 바뀐 점

| 계층 | 기능 | LTE 대비 |
|---|---|---|
| SDAP | QoS flow → DRB | **신규** (NSA에서는 미사용) |
| PDCP | ROHC, 암호·무결성, 핸드오버 재전송·중복 제거, in-order(선택), split bearer, **복제** | in-order를 넘겨받음 |
| RLC | 분할, ARQ (TM/UM/AM) | **in-order·연접 제거** |
| MAC | 다중화, HARQ, 스케줄링 | **서브헤더를 SDU 바로 앞에** |
| PHY | 코딩, 변조, MIMO | — |

- 이유는 전부 저지연: grant 전에 PDU를 미리 조립.

### 채널 매핑
- 논리: BCCH, PCCH, CCCH, DCCH, DTCH (NSA는 BCCH·PCCH 없음).
- 전송: BCH, PCH, DL-SCH, UL-SCH, RACH.
- 물리: PBCH, PDSCH, PDCCH, PUSCH, PUCCH, PRACH. **PDCCH·PUCCH는 대응 전송채널 없음**.

### 스케줄링과 HARQ
- 하향: gNB가 포맷·다중화까지 결정 / 상향: gNB는 포맷만, 베어러 선택은 단말.
- Configured grant: Type 1(RRC 설정·활성) / Type 2(RRC 주기 + L1 활성).
- HARQ: 병렬 stop-and-wait, **상하향 비동기**, 최대 16 프로세스.
- 코드블록 최대 8448비트(부호율 1/4 미만 3840).
- HARQ + RLC 이중 구조: 빠른 RTT + 신뢰 전달.

### RRC 상태

| 상태 | RRC 컨텍스트 | 코어 | 이동성 |
|---|---|---|---|
| IDLE | 없음 | CN_IDLE | 셀 재선택 |
| **INACTIVE** (신규) | gNB·단말 유지 | CN_CONNECTED | 셀 재선택 |
| CONNECTED | 있음 | CN_CONNECTED | 네트워크 주도 HO |

- 추적: 셀 ⊂ RAN Area ⊂ Tracking Area.
- 연결 상태 HO: L3(RRC) 보고 → 네트워크 결정 → **경쟁 없는 RACH 2단계**로 동기.

---

## 7장 전체 전송 구조

### 파형
- 상향 기본 CP-OFDM, DFT-s-OFDM은 보조(단일 레이어).
- DFT-s 단점: MIMO 수신 복잡, 상하향 비대칭, 연속 할당만.
- NR V2X 사이드링크는 CP-OFDM.

### Numerology

| μ | SCS | CP | 슬롯 | 1 ms당 슬롯 | 용도 |
|---|---|---|---|---|---|
| 0 | 15 kHz | 4.7 µs | 1 ms | 1 | FR1, LTE 공존 |
| 1 | 30 kHz | 2.3 µs | 0.5 ms | 2 | FR1 |
| 2 | 60 kHz | 1.2 µs (확장 4.2) | 0.25 ms | 4 | **유일한 확장 CP** |
| 3 | 120 kHz | 0.59 µs | 0.125 ms | 8 | FR2 |
| 4 | 240 kHz | 0.29 µs | 0.0625 ms | 16 | **SSB 전용** |

- SCS ↑ → 도플러·위상잡음에 강함, CP ↓ → 지연 확산에 약함.
- 15 kHz 기준 = LTE·NB-IoT 공존(MTC 교체 주기 10년+).
- 2의 거듭제곱 스케일링 → 심볼 경계 정렬.
- Tc = 1/(480000·4096), Ts = 64·Tc.
- **저지연은 numerology가 아니라 전송 길이를 슬롯에서 분리해서 얻는다.**

### 시간 구조
- 프레임 10 ms = 서브프레임 10 = 서브프레임당 슬롯 2^μ. SFN 0~1023(10.24 s).
- 서브프레임 = numerology 무관 시간 기준, 슬롯 = 스케줄링 단위.
- 미니슬롯 이유: 저지연·선점 / 아날로그 빔 시간 다중화 / LBT 직후 전송.

### 주파수 구조
- DC 부반송파도 데이터에 사용.
- RB = 주파수 12 부반송파(1차원).
- **Point A** = CRB 0의 부반송파 0 (SIB1로 알림). PRB 시작은 numerology별.
- 최대 275 RB(3300 부반송파), 400 MHz 제한 → 15/30/60/120 kHz에서 50/100/200/400 MHz. 최소 11 RB(SSB는 20 RB).
- 래스터: ≤3 GHz 5 kHz, 3–24.25 GHz 15 kHz, 그 위 60 kHz. 별도 희소 동기 래스터.

### BWP
- 이유: 전체 대역 수신 불가 단말 지원 + 전력 절감.
- 서빙셀당 DL 4·UL 4 설정(SUL +4), **방향별 활성 1개**.
- DCI로 빠른 전환. 활성 BWP 밖 측정은 측정 갭 필요.
- CA로 대신하지 않는 이유: CC에는 RF 요구가 붙고 BWP에는 없음, HARQ는 CC 간 이동 불가.

### CA vs SUL

| | CA | SUL |
|---|---|---|
| 목적 | 최대 전송률 | 상향 커버리지, 지연 |
| 구조 | 상향마다 하향 → 셀 여러 개 | 하향 1 + 상향 2 = **셀 1개** |
| 동시 전송 | 가능 | **불가** (DCI SUL 지시자) |

- CA: 최대 16 CC, MAC CE 비트맵 활성화, PUCCH 그룹 2개(PCell, PUCCH SCell).
- SUL은 TDD 상향 대기 지연도 줄임.

### 듀플렉스와 슬롯 포맷
- 단일 프레임 구조로 TDD·FDD·반이중 FDD.
- **동적 TDD 기본**: 동적을 제한하는 게 정적에 동적을 덧붙이는 것보다 쉽다.
- 보호 구간 3요소: 전환 시간(약 20 µs), 타이밍 어드밴스(셀 반경), 원거리 기지국 간섭.
- 심볼 상태 D/U/F. 전송 방향 신호: ① DCI ② RRC 반정적(0.5–10 ms) ③ SFI.
- **SFI는 RRC의 D/U나 동적 스케줄링을 덮어쓸 수 없고 F만 좁힘.** RRC·SFI 모두 F → 예약 자원.

### 안테나 포트와 QCL

| 포트 | 상향 | 하향 |
|---|---|---|
| 0번대 | PUSCH/DMRS | — |
| 1000번대 | SRS, 프리코딩 PUSCH | PDSCH |
| 2000번대 | PUCCH | PDCCH |
| 3000번대 | — | CSI-RS |
| 4000번대 | PRACH | SSB |

- 포트 = 사실상 기준신호 하나. 모르는 프리코더는 채널의 일부.
- QCL Type A(도플러 이동·확산, 평균 지연, 지연 확산) / B(도플러 이동·확산) / C(도플러 이동, 평균 지연) / **D(공간 Rx)**.

---

## 8장 채널 사운딩

| 용도 | LTE | NR |
|---|---|---|
| 복조 | CRS | DMRS |
| CSI | CRS → CSI-RS | CSI-RS |
| 시간·주파수 추적 | CRS | **TRS** |
| 간섭 측정 | — | CSI-IM |
| 빔·이동성 | — | CSI-RS 또는 SSB |
| 상향 측정 | SRS | SRS |

### CSI-RS
- 최대 32포트, N포트 = RB·슬롯당 RE N개(TRS 예외). 단말별 설정이지만 공유 가능.
- CDM2(부반송파 2) / CDM4(2×2) / CDM8(2×4). 포트 번호는 CDM → 주파수 → 시간.
- 밀도 1, 1/2(4·8·12포트 불가), 3(TRS).
- 주기적(4~640슬롯) / 반지속(MAC CE) / 비주기(DCI) — **resource set의 속성**.

### CSI-IM, ZP-CSI-RS
- CSI-IM: RE 4개(2×2 또는 4×1), 자기 셀은 비우고 이웃 간섭 측정.
- ZP-CSI-RS = "PDSCH를 매핑하지 마라"는 레이트 매칭 정보. 실제로 비어 있다는 보장 없음.

### TRS
- 1포트 밀도 3 CSI-RS 4개 × 연속 2슬롯(또는 1슬롯 2개), 주기 10/20/40/80 ms.
- 심볼 간격 4 → 15 kHz에서 주파수 오차 추적 범위 **약 ±1.75 kHz**.
- 부반송파 간격 4 → 타이밍 오차 범위 **약 ±8.3 µs**.
- 참고: 3.5 GHz·250 km/h 도플러 ≈ 810 Hz.

### 공간 필터 F
- 같은 F로 같은 안테나에 매핑 = 같은 포트. PMI는 네트워크가 같은 F를 쓴다고 가정.

### 측정·보고 (CSI-ReportConfig)
- 보고량: CQI/PMI/RI, **L1-RSRP**(빔 관리), **None**.
- L1-RSRP = 필터 없음 / L3 RSRP = 필터 거친 RRC 보고(핸드오버용).

| 보고 유형 | 채널 | 활성화 |
|---|---|---|
| 주기적 | PUCCH | RRC |
| 반지속 | PUCCH 또는 PUSCH | MAC CE |
| 비주기 | **PUSCH만** | DCI 0-1 CSI request(최대 6비트, 63종) |

- 허용 조합: 보고가 느슨할수록 더 다양한 자원(주기적 보고 ↔ 주기적 자원만).

### SRS
- 최대 4포트, 낮은 CM, 마지막 6심볼 안 1·2·4심볼, comb-2/4.
- **Zadoff-Chu**: 진폭 일정 + 스펙트럼 평탄 → 순환 이동끼리 직교. 소수 길이 M이면 M−1개. 길이 ≥36은 확장 ZC, 짧으면 컴퓨터 탐색 수열.
- 다중 포트 = 위상 회전("cyclic shift"). 비주기 SRS는 DCI 0-1·1-1 2비트.

---

## 9장 전송채널 처리

| 단계 | 핵심 | 숫자 |
|---|---|---|
| TB CRC | 오류 검출 | TB > 3824 → 24비트, 아니면 16 |
| CB 분할 | CB마다 CRC 24 | BG1 8448, BG2 3840 |
| LDPC | quasi-cyclic, 체계적 | lifting size 51종 |
| 레이트 매칭 | circular buffer, RV0~3 | RV0·RV3 단독 복호 가능 |
| 인터리빙 | 행 수 = 변조 차수 | — |
| 스크램블링 | C-RNTI + 데이터 ID | — |
| 변조 | QPSK~256QAM | π/2-BPSK는 DFT-s만 |
| 레이어 매핑 | TB당 ≤4레이어 | 5~8레이어는 TB 2개(DL) |
| DFT 프리코딩 | 상향 선택 | 크기 = 2·3·5의 곱 |
| 자원 매핑 | **주파수 우선** | VRB → PRB |

### LDPC
- BG1 부호율 1/3~22/24, BG2 1/5~5/6, 최대 0.95.
- BG 선택(TS 38.212): **TBS ≤ 292, 또는 TBS ≤ 3824 & R ≤ 0.67, 또는 R ≤ 0.25 → BG2**, 나머지 BG1.
- TB CRC + CB CRC 이유: CBG 재전송, 성공 CB 복호 생략, 추가 보호.

### 레이트 매칭·HARQ
- 체계 비트 일부 펑처링 → circular buffer → RV별 시작점 → IR.
- 재전송마다 유효 부호율 ↓.
- Limited buffer: DL은 최대 TBS × 부호율 2/3까지만.

### 프리코딩·매핑
- DL 프리코딩 투명, 스케줄링 간 시간 상관 가정 금지, PRG(2/4 RB/전체) 안에서만 같은 프리코더.
- 주파수 우선 매핑 = 파이프라인 처리.
- VRB→PRB 인터리빙(DL, 할당 type 1만): 작은 할당은 다이버시티, **큰 할당은 CB 품질 평균화**. 상향은 슬롯 안 주파수 호핑.

### 하향 예약 자원
- LTE 반송파 참조(CRS) / CORESET 참조 / 비트맵(bitmap-1 RB, -2 심볼, -3 40슬롯 주기).
- 최대 8세트, DCI 지시자 2개. **상향에는 없음**.

### DMRS

| 구분 | 내용 |
|---|---|
| Mapping type A | 첫 DMRS 슬롯 기준 심볼 2 또는 3 |
| Mapping type B | 할당 첫 심볼 (PDSCH 2·4·7심볼) |
| 추가 DMRS | 최대 3개 (고속) |
| 슬롯 간 보간 | **불가** |
| 설정 type 1 | 단일 4포트 / 이중 8포트 |
| 설정 type 2 | 단일 6포트 / 이중 12포트 |
| 수열 | Gold 2³¹−1, **CRB 0부터 생성** (MU-MIMO 직교성) |
| DFT-s | ZC, TDM, type 1, 단일 레이어 |

- Mapping type ≠ 설정 type.

### PTRS
- 시간 촘촘(L심볼마다, MCS 연동), 주파수 성김(2·4 RB마다, 대역 연동). 위치는 C-RNTI로, 포트는 최저 DMRS 포트.

---

## 10장 물리계층 제어 시그널링

### 하향 제어
- PDCCH 하나. **PHICH 없음**(비동기 HARQ), **PCFICH 없음**(CORESET 고정).
- DCI + 24비트 CRC(RNTI 스크램블). **Polar**, 리스트 8 복호 시 실효 21비트. 최대 512비트 코딩, 정보 ≤140비트.
- QPSK, PDCCH 전용 DMRS(밀도 1/4, LTE 1/6).
- **REG = 1 RB × 1심볼, CCE = 6 REG**(데이터 RE 54개).
- 집성 레벨 1·2·4·8·**16**(신규).

### CORESET
- 최대 3심볼, 6 RB 배수, 슬롯 어디든. **CORESET 0은 MIB로**.
- 고정 크기 → 즉시 처리. 남는 자원은 예약 자원으로 재사용.
- CCE-REG: 비인터리빙(번들 6, 주파수 선택) / 인터리빙(번들 2·3 또는 6, 다이버시티, 고속 fallback).
- TCI는 CORESET마다. 미설정 시 SSB와 QCL.

### 블라인드 복호
- 단말 search space: C-RNTI 해시, 시간에 따라 변화. 공통 search space: SI/P/RA/TPC/INT/SFI-RNTI.
- 상한(15/30/60/120 kHz): 블라인드 복호 **44/36/22/20**, CCE 채널 추정 **56/56/48/32**, DCI 크기 **3+1**.

### DCI 포맷

| 포맷 | 용도 |
|---|---|
| 0-0 / 0-1 | 상향 grant |
| 1-0 / 1-1 | 하향 할당 |
| 2-0 | SFI |
| 2-1 | 선점 지시 |
| 2-2 | PUCCH/PUSCH 그룹 TPC |
| 2-3 | SRS TPC·요청 |

- Fallback 이유: RRC 재설정 직후 불확실 구간, 작은 패킷 오버헤드.
- 주요 필드: MCS 5, NDI 1, RV 2, HARQ 4, K1 3, PRI 3, TCI 3, BWP 2, CIF 3, CSI req 0~6, SRS req 2.

### 자원 지시
- 주파수: type 0(RBG 비트맵) / type 1(시작+길이) / 동적 전환. BWP 전환 시 필드 패딩·절단.
- 시간: RRC 표(≤16행) — K0 0~3, **K2 0~7**, SLIV, 매핑 type. 슬롯 집성 최대 8.
- TBS: MCS 표 → 공식(RE − DMRS − 오버헤드) × 레이어 × Qm × R → 양자화. 예약 MCS = 재전송용 변조만 지시.

### 상향 제어

| 포맷 | 길이 | 비트 | 구조 |
|---|---|---|---|
| 0 | 1~2 | ≤2 | 수열 선택(위상 회전 = 정보) |
| 1 | 4~14 | ≤2 | 수열 × 시간 OCC |
| 2 | 1~2 | >2 | **OFDM**, RM/Polar |
| 3 | 4~14 | >2 | DFT-s, 다중 RB, 최대 페이로드 |
| 4 | 4~14 | >2 | DFT-s, 1 RB, 확산 2/4 |

- PUCCH resource set ≤4: UCI 크기 → set, **PRI → set 안 자원**. Set 0 = 포맷 0·1.
- UCI on PUSCH: ACK ≤2비트 펑처링, 초과 시 상향 DAI로 레이트 매칭. ACK는 첫 DMRS 다음 심볼, beta offset.
- Rel-15는 PUSCH+PUCCH 동시 전송 불가.

---

## 11장 다중안테나 전송

- 고주파 손실의 일부는 "안테나 크기가 파장에 비례해 줄어든다"는 가정에서 옴. 전체 크기를 유지하고 소자를 늘리면 지향성 ∝ f² → LOS 링크 예산은 오히려 개선 가능(대기·회절 손실은 별개).

| | 아날로그 | 디지털 |
|---|---|---|
| 위치 | DAC 뒤 | DAC 앞 |
| 가능 | 위상만(빔포밍) | 일반 행렬 W |
| 다중화 | 한 번에 한 방향 → 시간 분할 | 주파수 분할 동시 다방향 |
| 사용처 | 고주파(전력 제한) | 저주파(대역 제한) |

- DMRS를 같이 프리코딩 → 채널 H·W, 프리코딩 투명.

### 하향 CSI
- RI → PMI → CQI 순으로 조건부. **코드북은 보고용, 실제 프리코더 제한 없음.**

| 종류 | 대상 | 구조 | 최대 레이어 | 크기 |
|---|---|---|---|---|
| Type I single-panel | SU-MIMO | W = W1·W2 (W1 광대역 빔, W2 서브밴드 선택 + 편파 위상) | 8 | 수십 비트 |
| Type I multi-panel | 패널 2~4 | 패널 간 위상 추가 | 4 | 수십 비트 |
| Type II | MU-MIMO | 빔 ≤4 선형 결합(진폭·위상) | 2 | 수백 비트, 저속 전용 |

### 상향 프리코딩
- 최대 4레이어, 코히어런스 능력 full / partial / none.

| | Codebook | Non-codebook |
|---|---|---|
| 전제 | 상호성 불성립 | 상호성 성립 |
| 흐름 | SRS → 네트워크가 랭크·TPMI 결정 | CSI-RS로 단말이 계산 → 레이어별 SRS → SRI로 선택 |
| SRI | SRS(패널) 1개 | 여러 SRS = 랭크 |

---

## 12장 빔 관리

- 목표: 빔 쌍 수립·유지. 최적 쌍이 반사 경로일 수도 있음.
- **빔 대응성**은 빠른 페이딩과 무관 → FDD에도 적용.

| 단계 | 용어 | 측정 | 고정 | 보고 |
|---|---|---|---|---|
| 초기 | P1 | SSB 스윕 | — | SSB 인덱스 ↔ RACH occasion |
| 송신 빔 조정 | **P2** | CSI-RS 스윕(repetition off) | 단말 수신 빔 | L1-RSRP ≤4 빔 |
| 수신 빔 조정 | **P3** | 반복 CSI-RS(repetition on) | 기지국 송신 빔 | None |
| 상향 | — | SRS | — | 대응성 있으면 생략 |

- L1-RSRP 보고: 최강 빔 절댓값 + 나머지 차분. CSI-RS는 1·2포트만.

### TCI
- RRC 최대 64 state.
- PDCCH: CORESET별 후보 → MAC CE로 1개.
- PDSCH: 스케줄링 오프셋 > N이면 MAC CE 8개 + DCI 3비트, ≤ N이면 CORESET TCI 따름.
- N = timeDurationForQCL (60 kHz 7/14/28심볼, 120 kHz 14/28심볼).

### 빔 실패 복구 (BFR)
- RLF와 별도인 이유: 좁은 빔은 자주 끊기지만 같은 셀 안에서 L1/L2로 빨리 복구 가능.
1. **검출**: PDCCH와 QCL된 기준신호의 가상 PDCCH BLER 기준, 연속 실패 횟수 초과.
2. **후보 탐색**: L1-RSRP 임계값.
3. **요청**: 후보 빔에 연결된 경쟁 없는 프리앰블 + 전력 램핑(빔별 고유 / 그룹 / 공통).
4. **응답**: 요청 4슬롯 후부터 후보 빔 QCL PDCCH를 창 안에서 대기.

---

## 13장 재전송 프로토콜

| 계층 | 속도 | 신뢰도 | 용도 |
|---|---|---|---|
| HARQ (MAC) | 매우 빠름 | 피드백 오류 0.1~1% 수준 잔여 | 일반 오류 정정 |
| RLC ARQ | 느림 | ~10⁻⁵ 이하 | HARQ 잔여 오류, TCP 보호 |
| PDCP | 이벤트성 | — | gNB 간 HO, 복제 |

- TCP 100 Mbit/s 이상 유지에는 손실 < 10⁻⁵ 필요.

### HARQ
- 반송파당 entity 1개, 비동기, ≤16 프로세스. 비동기 이유: 동적 TDD, 비면허.
- CBG 2/4/6/8, 구성은 초기 전송 때 고정. 같은 프로세스에서 새 TB와 섞기 불가.
- IR, 기본 RV 순서 **0 → 2 → 3 → 1**. IR 이득은 초기 부호율이 높을수록 큼.
- HARQ = 암묵적 링크 적응(처리량 유리, 속도 무관). 명시적 링크 적응이 필요한 이유는 지연.
- NDI 토글(1비트 SN). CBGTI 비트맵, CBGFI 1비트(**하향만**).

### ACK 타이밍과 처리 시간
- K1: DCI 3비트 → RRC 표 인덱스 (LTE는 고정 약 3 ms).

| N1 | 15 kHz | 30 kHz | 60 kHz | 120 kHz | LTE |
|---|---|---|---|---|---|
| 기본 (front-loaded) | 0.57 ms = 8심볼 | 0.36 ms = 10 | 0.30 ms = 17 | 0.18 ms = 20 | 2.3 ms |
| 추가 DMRS | 0.92 ms | 0.46 ms | 0.36 ms | 0.21 ms | — |
| 고속 능력 | 0.18~0.29 ms | 0.08~0.17 ms | — | — | — |

- 심볼 수가 SCS와 함께 늘어남 → µs 고정 성분 존재.

### HARQ-ACK 코드북

| | Semi-static (Type-1) | Dynamic (Type-2, 기본) |
|---|---|---|
| 크기 | 고정 (K1 집합 × CC·TB·CBG) | 스케줄된 것만 |
| 빈 자리 | NACK | — |
| DCI 유실 대책 | 불필요 | cDAI + tDAI (각 2비트, mod 4) |
| 단점 | 커질 수 있음 | 연속 4개 이상 유실 시 감지 불가 |

### RLC
- TM(BCCH·CCCH·PCCH) / UM(SN 6·12) / AM(SN 12·18).
- 헤더: SI, SO(16비트), Poll, D/C.
- 연접·in-order 제거 → 사전 조립, head-of-line blocking 제거.
- **RLC 재전송은 다른 반송파로 가능**(HARQ는 불가).
- t-Reassembly → 상태 보고, poll 비트, status prohibit 타이머, 재분할.

### PDCP
- ROHC(IPv4 40 B, IPv6 60 B → VoIP에서 헤더 약 60%).
- 암호·무결성, split bearer 라우팅·복제, CU-DU 흐름 제어.
- COUNT = HFN + SN으로 재전송·재정렬. **discard 타이머**.

---

## 14장 스케줄링

### 하향
- 스케줄러 알고리즘은 비표준. 입력: 채널, 버퍼, 우선순위.
- 다중 사용자 다이버시티 vs Massive MIMO의 channel hardening.
- 다른 numerology cross-carrier: 타이밍은 PDSCH numerology 기준.
- BWP 전환 약 1슬롯, 비활성 타이머 → 기본 BWP.
- **선점 지시**: DCI 2-1, 14비트 비트맵(1심볼×BWP 또는 2심볼×BWP 절반), 단말 동작은 비규정.

### 상향
- 전력 제한 → FDM 비중 큼. grant는 단말 단위, 데이터 없으면 미전송.
- **N2**(grant → PUSCH): 0.71/0.43/0.41/0.32 ms = **10/12/23/36심볼** (LTE 3 ms).

### LCP
1. 적격성: 허용 SCS, **최대 PUSCH 길이**, 허용 셀.
2. 분배: 우선순위 + PBR × BSD 토큰 버킷 → 잔여는 엄격 우선순위.
- 최대 길이 제한 = 급한 데이터가 긴 PUSCH에 묶이는 것 방지. 허용 셀 = 복제 사본 분리.

### SR / BSR / PHR

| 보고 | 경로 | 핵심 |
|---|---|---|
| SR | PUCCH | 논리채널별 다중 SR 설정, 주기 2심볼~80 ms, 없으면 RACH |
| BSR | MAC CE | LCG ≤8, 포맷 4종, 트리거: 고우선 도착·주기·패딩 |
| PHR | MAC CE | Type 1/2/3, **음수 가능**, 반송파 단위 |

### Grant 없는 전송
- DL SPS: CS-RNTI, HARQ ID 시간 공식.
- CG Type 1: RRC 전부, **SFN 기준 오프셋**.
- CG Type 2: PDCCH 활성 + **MAC CE 확인 응답**.

### DRX
- 주기, on-duration, 비활성 타이머, HARQ RTT·재전송 타이머, 짧은/긴 주기(VoIP 20 ms), ms 단위, DRX command MAC CE.

---

## 15장 상향 전력·타이밍 제어

### PUSCH 전력
**P = min{ P_CMAX,c , P₀(j) + α(j)·PL(q) + 10·log₁₀(2^μ·M_RB) + Δ_TF + f(l) }**

- α < 1: 이웃 간섭 ↓, 셀 가장자리 전송률 ↓.
- Δ_TF = 10·log₁₀((2^(1.25·BPRE) − 1)·β) → 섀넌의 80% 모델. 단일 레이어만, 부분 보상과 병용 금지.
- TPC 2비트: −1 / 0 / +1 / +3 dB.

### 빔 기반 (모두 SRI로 선택)

| 인덱스 | 내용 | 개수 |
|---|---|---|
| q | 경로손실 기준신호 | ≤4 |
| j | (P₀, α) 쌍 | j=0 Msg3(α=1), j=1 CG, j≥2 동적 |
| l | 폐루프 | ≤2 |

- PUCCH: α = 1, TPC는 하향 DCI.
- 다중 반송파: 합이 P_CMAX 초과 시 비례 축소. **EN-DC는 LTE 우선**.

### 타이밍 어드밴스
- 목표: 수신 시점 차이를 CP 안에. MAC CE, 초당 한두 번.
- TA 타이머 만료 → RACH부터.
- 스텝 = 16·64·Tc/2^μ → 15 kHz 약 0.52 µs(약 78 m), 120 kHz 약 65 ns(약 10 m).
- TAG: 수신 지점이 다른 반송파 그룹별 TA.

---

## 16장 초기 접속

### SSB
- 4심볼 × 240 부반송파. 심볼1 PSS(127), 심볼3 SSS(127) + 양옆 PBCH 48, 심볼 2·4 PBCH. PBCH RE 576.

| SCS | 대역폭 | 길이 | 범위 |
|---|---|---|---|
| 15 kHz | 3.6 MHz | ~285 µs | FR1 (<3 GHz) |
| 30 kHz | 7.2 MHz | ~143 µs | FR1 |
| 120 kHz | 28.8 MHz | ~36 µs | FR2 |
| 240 kHz | 57.6 MHz | ~18 µs | FR2 |

- **60 kHz SSB 없음**.
- 주기 5~160 ms, 초기 탐색은 ≤20 ms 가정(LTE 5 ms).
- SS burst set: 5 ms 안. 최대 SSB **4(<3 GHz) / 8(3~6 GHz) / 64(FR2)**.
- PCI = 3(PSS) × 336(SSS) = **1008** (LTE 504).

### PBCH

| 정보 | 비트 |
|---|---|
| SSB 시간 인덱스 | FR1 0 / FR2 3 |
| CellBarred | 2 |
| 첫 DMRS 위치 | 1 |
| SIB1 numerology | 1 |
| SIB1 설정 | 8 |
| CRB 오프셋 | FR1 5 / FR2 4 |
| 반 프레임 | 1 |
| SFN | 10 |
| CRC | 24 |

- SSB 인덱스 하위 비트는 PBCH DMRS 수열로 암묵 전달.
- CellBarred의 NSA 용도: NR 직접 접속 차단.

### 시스템 정보
- SIB1(RMSI): 160 ms, SI-RNTI, RACH 설정 포함. 나머지 SIB는 방송 또는 **on-demand**.

### 랜덤 액세스
- 4단계: 프리앰블 → RAR(ID, TA, Msg3 grant, TC-RNTI) → Msg3 → Msg4.
- 용도: 초기 접속, HO, 동기 재수립, SR 대체, BFR.
- RACH 주기 10~160 ms. ZC, 순환 이동 수는 zero-correlation zone(셀 크기).

| | Long | Short |
|---|---|---|
| 길이 | 839 | 139 |
| SCS | 1.25 / 5 kHz | 15·30 (FR1) / 60·120 (FR2) |
| 범위 | FR1만 | FR1, FR2 |
| 포맷 | 0~3 | A1~A3, B1~B4, C0, C2 |
| 폭 | 6 / 24 RB | 12 RB |

- SSB ↔ RACH occasion 연결: 주파수 → 슬롯 안 시간 → 슬롯 간.
- 전력 램핑, 경쟁 해소(C-RNTI PDCCH 또는 TC-RNTI + 식별자 일치 → ACK). 경쟁 없는 RA는 2단계에서 끝.
- SUL: RSRP < 임계값이면 SUL로 RA.

---

## 17장 LTE/NR 연동과 공존

### 이중 연결
- 마스터 = 제어 평면, 세컨더리 = 사용자 평면.
- 시나리오: LTE 매크로 + NR 소형셀 / 같은 사이트 같은 대역(LTE-NR CA 불가 → DC) / 같은 사이트 mmWave(견고성).
- 옵션 3(EPC, LTE 마스터), 4(5GC, NR 마스터), 7(5GC, LTE 마스터).
- **Single-TX**: 어려운 대역 조합에서 LTE·NR 상향을 시간 분할. LTE는 DL/UL reference configuration 차용, 단말별 시간 이동.

### 스펙트럼 공존
- 정적 분할보다 동적 공유(DSS).
- 상향: 스케줄링 협조로 충분. 하향: LTE 상시 신호 회피.

| LTE 신호 | 회피 |
|---|---|
| PSS/SSS | 예약 자원 비트맵 |
| PBCH | 예약 자원 비트맵 |
| CRS | 전용 CRS 레이트 매칭 (대역·위치, MBSFN, 포트 수, v-shift). **15 kHz만** |

---

## 18장 RF 특성

- FR1 450~6000 MHz, FR2 24.25~52.6 GHz. 채널 5~400 MHz, 점유율 최대 98%(LTE 90%).
- 보호 대역 GB = (BW − N_RB·12·Δf)/2 − Δf/2. 예: 100 MHz·30 kHz·273 RB → 한쪽 약 845 kHz.

| 범위 | SCS | 대역폭 | N_RB |
|---|---|---|---|
| FR1 | 15 | 5~50 MHz | 25~270 |
| FR1 | 30 | 5~100 MHz | 11~273 |
| FR1 | 60 | 10~100 MHz | 11~135 |
| FR2 | 60 | 50~200 MHz | 66~264 |
| FR2 | 120 | 50~400 MHz | 32~264 |

- TDD 사업자 간 동기화는 암묵적 전제.

### 규격 구성
- 단말: 38.101-1(FR1) / -2(FR2 OTA) / -3(연동) / -4(성능).

| BS 유형 | 범위 | 방식 |
|---|---|---|
| 1-C | FR1 | conducted |
| 1-H | FR1 | TAB conducted + OTA 2종(EIRP, EIS) |
| 1-O | FR1 | 전부 OTA |
| 2-O | FR2 | 전부 OTA |

- BS 클래스: Wide area(35 m, 제한 없음) / Medium range(5 m, 38 dBm) / Local area(2 m, 24 dBm).
- 단말 23 dBm, MPR, A-MPR(NS_x, 기본 NS_01).

### 송신
- 출력 전력 / 신호 품질(주파수 오차, EVM, 시간 정렬, in-band 방사) / 불요 방사.
- OOB·spurious 경계 2.5×BW. BS는 OBUE(대역 + 10~40 MHz), 단말은 SEM.
- **1/ACIR = 1/ACLR + 1/ACS**. BS ACLR 45 dB, 단말 30 dB → 비용을 BS 쪽으로.
- OBW 99%. 송신 IM: BS −30 dB, 단말 −40 dB.

### 수신
- 기준 감도: QPSK, 95% 처리량. BS 동적 범위: 16QAM + AWGN 20 dB.
- 간섭 내성: 차단(out/in-band), ACS, 협대역 차단, ICS(BS만), IM.

### OTA
- 불요 방사는 **TRP**로 정의(AAS에서 EIRP는 간섭 총량을 통제 못 함).
- FR2: 단말 ACLR·ACS 약 10 dB, BS ACLR 약 15 dB 완화. 협대역 차단 없음.
- 후속: MSR, 다중 대역 BS, 비연속 스펙트럼.

---

## 19장 mmWave RF 기술

| 블록 | 핵심 법칙 | 규격 영향 |
|---|---|---|
| ADC/DAC | Schreier FoM 상한 ~180 dB(<100 MHz), 그 위 10 dB/decade 불리 → BW 2배 = 전력 4배, 1비트 = 전력 4배 | SNDR 요구를 과하게 잡지 말 것 |
| LO 위상잡음 | 주파수 2배 → +6 dB (3→30 GHz +20 dB), ∝1/Ps, ∝1/Q² | PTRS, 큰 SCS의 근거 |
| PA | −20 dB/decade, GaN ~4000 vs CMOS ~100 mW/mm, PAE ~40%@30 GHz, ~25%@77 GHz | 백오프 → 효율·발열, FR2 ACLR 완화 |
| 필터 | Q: 칩 20 / PCB 100 / 세라믹 300 / 고급 500 / 도파관 5000 | 안테나 뒤에 날카로운 필터 어려움 |
| 잡음지수 | NF ≈ 앞단 손실 × 수신기 NF, SNRmax ∝ 1/BW, DR ∝ Vdd·P/BW | 30 GHz NF는 높을 수밖에 없음 |

- 필터 위치: F0/F1(안테나 뒤) / F2(첫 증폭기 뒤) / F3(믹서 고주파 쪽, 합성 후라 소수).
- 800 MHz 대역 IL < 1.5 dB → Q ≈ 500, 4배 넓히면 Q ≈ 100으로 2 dB.
- PCB 필터 IL 3~4 dB, 억압 20 dB, 천이 1.5 GHz. LTCC 마진 포함 IL ~3 dB, 1.5 GHz에서 17 dB.
- OFDM PAPR 10 dB(3σ, 클리핑 < 0.2%), IP3 ≈ CP + 10 dB.
- 비유: 30 GHz 설계 = 15년 전 속도의 소자로 2 GHz 설계(fmax·Vdd 일정).

---

## 20장 첫 release 이후

| 책의 전망 | 핵심 논리 | 실제 결과 |
|---|---|---|
| IAB | 접속·백홀의 주파수·전파 환경 수렴 | Rel-16, 이후 확장 |
| 비면허 | 미니슬롯 + LBT, DFS | NR-U Rel-16, Rel-17 60 GHz |
| NOMA | 일부 용량 이득 | 표준화 안 됨 |
| mMTC | 장기적으로 NR | RedCap Rel-17, eRedCap Rel-18 |
| 공장 자동화 | 산업 프로토콜, 로컬 브레이크아웃 | IIoT/URLLC Rel-16 (TSN, 복제 ≤4경로, multi-TRP) |
| 사이드링크 | 범용 도구, 군집주행 | NR V2X Rel-16 (HARQ 피드백, mode 1/2) → Rel-17·18 확장 |
| 듀플렉스 | 상향 대역 하향, 전이중 | SBFD Rel-18 study → Rel-19 |

- 책에 없던 흐름: Rel-16 **CHO·DAPS**, 2단계 RACH, 측위 / Rel-17 NTN, FR2-2, MBS / Rel-18(5G-Advanced) AI/ML, NCR, **LTM**.
- 전이중 경고: 링크 처리량 2배 가능하지만 간섭 증가로 시스템 이득은 고립 링크에서만 큼 → 실제로 기지국 SBFD로 좁혀짐.

---

## 부록 A. 연구 연결 포인트 모음

> V2X 제어, TVT(control-aware CHO), ICPE(DAPS), 학위 논문(Port-Hamiltonian/UUB) 관점에서 정리한 분석. 확정이 아니라 가능성 수준의 해석 포함.

### A1. 요구사항 정의의 한계 (1·2장)
- 신뢰도와 지연은 독립 축 → 제어기가 버틸 수 있는 지연·손실 조합으로 통신 요구를 역정의하는 근거.
- ITU 신뢰도·지연은 **패킷 하나, 무선 구간**만 정의. 제어 안정성은 종단 간 지연 분포와 **연속 손실(burst)**에 좌우될 가능성이 큼. Rel-16 survival time과 연결.
- 단절 시간 0 ms는 **분석**으로만 평가 → 실제 HO 실패·RLF는 범위 밖.
- 이동성과 저지연이 동시에 필요한 V2X 조합이 ITU 시험 환경에 비어 있음 → 서론 gap 근거.

### A2. LTE-V2X 사이드링크 (4장)
- HARQ 피드백이 없어서 손실이 송신 측에서 관측되지 않는 교란 → 제어 계층에서 다뤄야 하는 이유가 특히 강함. NR V2X가 유니캐스트·그룹캐스트 HARQ를 넣은 동기.
- Mode 4/2는 자원 충돌이 확률적 → 모드별로 교란 모델을 분리.

### A3. 핸드오버 단절 분해 (6·8·16·20장)
- 단절 ≈ L3 필터·TTT 지연 + HO 명령 전달 + **다음 RACH occasion 대기** + RACH/RAR + HARQ 버퍼 초기화로 인한 PDCP 재전송.
- **L3 필터** F_n = (1−a)·F_{n−1} + a·M_n, a = 1/2^(k/4) → 위상 지연을 주는 저역통과 + 이벤트 트리거. k와 TTT를 제어 성능 기준 설계 변수로.
- 기술별 겨냥 항:
  - **CHO** (Rel-16): HO 실패 감소 → TVT
  - **DAPS** (Rel-16): 단절 자체 제거, PDCP 기반 → ICPE
  - **LTM** (Rel-18): L1 측정 + MAC CE로 L3 필터 지연 우회, 사전 TA로 RACH 생략

### A4. 다중 시간척도 교란과 루프 중첩 (6·9·12·13·15장)
- HARQ(슬롯) → 링크 적응 → 폐루프 전력 제어(**TPC 누적 = 적분기**) → 빔 관리·BFR(수 ms~수십 ms) → TA(초) → L3/HO(수백 ms).
- HARQ·링크 적응은 차량 동역학보다 충분히 빨라 준정적 처리 가능. TA·L3·HO는 시간 척도가 겹쳐 **singular perturbation 분리가 어려울 가능성**.
- 학위 논문 결합 모델에서 어떤 무선 루프를 상태로 넣고 어떤 것을 파라미터로 둘지의 기준.

### A5. 지연 분포의 구조 (6·9·13·14장)
- 지연 ≈ 기본 지연 + k × HARQ RTT → **이산 다중모드 분포**, 그 위에 RLC 계단.
- IR 때문에 재전송마다 조건부 성공 확률 ↑ → i.i.d. 베르누이 가정은 꼬리 확률을 과대평가할 수 있음.
- 상향 지연 분해:
  - 동적 grant: SR 대기 + SR + gNB 처리 + PDCCH + **N2** + PUSCH + 복호
  - Configured grant: CG 주기 대기 + PUSCH + 복호
- N1·N2를 심볼로 보면 µs 고정 성분 존재 → SCS를 키워도 비례 감소 안 함.
- TDD 패턴 주기가 상향 지연 하한 → 모델에 명시 파라미터로. SUL·FDD 비교축.

### A6. 제어 메시지용 스택 설정 처방 (6·13·14장)
- RLC **UM** (ARQ·t-Reassembly 꼬리 제거)
- PDCP **재정렬 끔 + discard 타이머 ≈ 제어 주기** (오래된 샘플은 보내지 않음)
- **Configured grant Type 1** (SR-grant 사이클 + PDCCH 병목 제거)
- LCP **최대 PUSCH 길이 제한 + 전용 SR 설정**
- 필요 시 PDCP 복제를 서로 다른 반송파로(LCP 허용 셀)
- RLC in-order 제거 자체가 Age of Information 관점에서 유리(최신 값 우선).

### A7. 신뢰도 병목 (10·12장)
- 손실 ≈ P(PDCCH 놓침) + P(PDSCH 실패 | PDCCH 성공). BFR도 가상 PDCCH BLER 기준.
- Configured grant의 의미는 저지연 + **PDCCH 병목 제거**.

### A8. BFR과 스위칭 시스템 (12장)
- BFR 복구 시간 ≈ 검출(횟수 × 주기) + 후보 탐색 + RACH + 4슬롯 + 응답 창 → 그동안 링크 단절.
- 정상 모드/단절 모드 스위칭 시스템 → **평균 체류 시간(ADT)** 조건으로 허용 BFR 빈도·지속 시간 역산. UUB·Port-Hamiltonian 언어로 연결.

### A9. 고속 이동 수치 (7·8·9·15장)
- 3.5 GHz·250 km/h 도플러 ≈ 810 Hz, TRS 추적 범위(15 kHz) ±1.75 kHz 안.
- 추가 DMRS 최대 3개, 슬롯 간 보간 금지 → 슬롯 안 채널 노화 모델링.
- Type II CSI는 저속 전용 → 고속은 Type I 또는 상호성 기반.
- **TA 드리프트**: 250 km/h → 초당 약 0.23 µs. CP 4.7 µs(15 kHz)는 십수 초, **0.59 µs(120 kHz)는 약 2~3초** → FR2 V2X는 TA 갱신 주기가 설계 변수.
- 측정 갭 동안 PDCCH 미모니터링 → HO 준비 측정 구간이 데이터 공백.

---

## 부록 B. 원문 오류 목록

| 장 | 원문 | 수정 |
|---|---|---|
| 1 | "early 2000", "together will LTE" | 2000s, with |
| 2 | M.2410 요구 "13개" vs "14개" | 표 기준 14행 (세는 방식 차이) |
| 2 | inspection 항목에 "evaluated through simulation" | inspection |
| 3 | "Supplementary Uplink (SDL)" | SUL |
| 3 | "3300–600 MHz" | 3300–3600 MHz |
| 3 | n79 4400–5500 MHz | 4400–5000 MHz |
| 4 | "connected to a single device only" | single cell |
| 4 | "the car in front breaks" | brakes |
| 4 | CA 문단 "number of subcarriers" | carriers |
| 4 | 키워드 "V2Vm VX" | V2V, V2X |
| 5 | "SS-slock" | SS-block |
| 6 | "RRC_ACTIVE" | RRC_CONNECTED |
| 6 | "UDF functionality" | UPF |
| 6 | "Service Data Application Protocol", "SDPA" | Adaptation, SDAP |
| 6 | 페이징 출처 "MME" | AMF |
| 6 | "NR Repository Function" | NF Repository Function |
| 7 | "activated or deceived" | deactivated |
| 7·10 | "primary second cell (PSCell)" | PUCCH SCell |
| 8 | "CS-RS", "NZP-CI-RS", "CRS-RS" | CSI-RS |
| 8 | ZP-CSI-RS "resource blocks" | resource elements |
| 8 | "resource configuration" (8.2) | report configuration |
| 9 | BG1 "8424 bits" | 8448 |
| 9 | "identify matrix" | identity |
| 9 | 그림 9.15: set #2가 둘 다에 할당 | set #3 |
| 9 | 매핑 type 참조 "Section 9.11" | 10.1.11 |
| 10 | 표의 "CFI" | CIF |
| 10 | 상향 DCI "(DCI format 1–1 only)" | 0-1 |
| 10 | 2-3 절 "DCI format 2–2 is aligned" | 2-3 |
| 10 | "join message", "PUSH" | joint, PUSCH |
| 10 | 포맷 1 절 "PUCCH format 2" | format 1 |
| 12 | BFD를 L1-RSRP 기준으로 설명 | 가상 PDCCH BLER (L1-RSRP는 후보 탐색) |
| 12 | "retransmits the recovery response" | request |
| 13 | PRI가 "resource sets" 중 선택 | set 안 자원 선택 |
| 13 | "configured in Table 13.1" (코드북) | K1 표 |
| 13 | "PDCH", "is reordering is configured" | PDCP, if |
| 14 | "Fig. 14.1" (처리 시간) | Table 14.1 |
| 14 | 표 14.1 제목 "in OFDM symbols" | 값은 ms |
| 14 | "local channel" | logical |
| 15 | "random-message 3", "subcarriers pacing" | random-access message 3, subcarrier spacing |
| 16 | "PSS and SS" | SSS |
| 16 | "half of a 5 ms frame" | 10 ms frame |
| 16 | "80 ms (eight subframes)" | eight frames |
| 16 | "Section 6.1.6" | 16.1.6 |
| 16 | SSB 인덱스 "scrambling" 전달 | PBCH DMRS 수열 |
| 16 | long 포맷 = LTE "0 and 2" | LTE 0 and 3 |
| 17 | "Section 5.1.3", "5.1.11" | 5.3, 5.11 |
| 17 | "concept or reserved", "three would" | of, there |
| 18 | 1-O·2-O "conducted (OTA)" | radiated |
| 18 | "soectrum", "PA efficiently" | spectrum, efficiency |
| 19 | 키워드 "ACD" | ADC |
| 20 | "up mixed" | mixed up |

- 11·15·19장은 텍스트 추출 과정에서 수식·기호가 빠져 있음 (본 노트에 복원).
