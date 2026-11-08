# 5G NR 링크 모델에 필요한 3GPP TS 38.xxx 원문 발췌와 조항 대응

## 사용 범위와 인용 방식

이 문서는 Chen 논문의 추상 MD–SBS 링크를 UE–gNB NR uplink로 바꾸는 데 직접 필요한 조항만 골랐다. 각 표의 **원문 발췌**는 표준의 짧은 원문 구절 또는 조항 제목이다. 표준 전체 본문은 저작권 대상이므로, 전체 문장은 아래의 공식 3GPP 문서 링크에서 확인한다.

기준일: 2026-11-08. Release/version은 구현 시 하나로 고정해야 하며, 여기서는 조항 구조를 가리키기 위해 규격 번호를 사용한다.

| 규격 | NR 링크에서 맡는 역할 |
|---|---|
| TS 38.211 | OFDM numerology·resource grid·PUSCH physical signal과 DM-RS |
| TS 38.212 | UL-SCH CRC/LDPC/rate matching 등 bit 처리 |
| TS 38.213 | uplink physical-layer procedure, 특히 PUSCH power control |
| TS 38.214 | PUSCH 자원할당, MCS와 transport block size |
| TS 38.300 | UE–gNB 및 NR/NG-RAN의 전체 무선접속 구조 |

---

## 1. 3GPP TS 38.211 — NR physical channels and modulation

공식 규격 페이지: [3GPP portal — TS 38.211](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3213)  
공식 archive: [38.211 FTP archive](https://www.3gpp.org/ftp/Specs/archive/38_series/38.211/)

| 관련 조항 | 원문 발췌 | Chen→NR 모델에서 결정하는 것 |
|---|---|---|
| §4.2 | “Numerologies” | SCS와 numerology `μ`; slot 길이와 OFDM symbol 시간 |
| §5.3 | “Modulation” | PUSCH에서 MCS가 선택한 QPSK/16QAM/64QAM/256QAM 매핑 |
| §6.2 | “Physical resources” | RE, PRB, slot/mini-slot에 전송을 실어 보내는 자원 단위 |
| §6.3.1 | “Physical uplink shared channel” | Chen의 MD→SBS 무선 uplink를 UE→gNB PUSCH로 치환하는 핵심 조항 |
| §6.4.1.1 | “Demodulation reference signal for PUSCH” | gNB 채널추정에 쓰는 PUSCH DM-RS와 overhead |

**모델 연결:** 원 논문의 `W`는 이 규격의 PRB 수, SCS, 할당 symbol 수로 바뀐다. `g_mn`은 수신기가 DM-RS로 추정하는 PUSCH 채널로 해석할 수 있지만, 실제 어떤 채널모델을 쓸지는 별도로 정해야 한다.

---

## 2. 3GPP TS 38.212 — Multiplexing and channel coding

공식 규격 페이지: [3GPP portal — TS 38.212](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3214)  
공식 archive: [38.212 FTP archive](https://www.3gpp.org/ftp/Specs/archive/38_series/38.212/)

| 관련 조항 | 원문 발췌 | Chen→NR 모델에서 결정하는 것 |
|---|---|---|
| §6.2.2 | “LDPC base graph selection” | payload 크기와 code rate에 따른 base graph 선택 |
| §6.2.4 | “Channel coding of UL-SCH” | KB update/task data가 UL-SCH transport block일 때의 coding chain |
| §6.2.5 | “Rate matching” | 할당된 PUSCH 자원에 맞춰 coded bits를 맞추는 처리 |
| §6.3.2 | “Rate matching” | UCI와 PUSCH multiplexing이 있는 경우의 coded-bit 자원 분배 |

짧은 원문 발췌: “each code block of the transport block is encoded with either LDPC base graph 1 or 2.”

**모델 연결:** 원 논문의 `d_mn^K`, `ξ_mn d_mn^T`, `d_mn^T`는 모두 전송 전에 UL-SCH transport block으로 분할되고 LDPC/rate matching을 거친다. 그러므로 NR 재구성에서는 ‘보낼 bit 수’와 ‘한 PUSCH grant에서 실을 수 있는 TBS’를 구분해야 한다.

---

## 3. 3GPP TS 38.213 — Physical layer procedures for control

공식 규격 페이지: [3GPP portal — TS 38.213](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3215)  
공식 archive: [38.213 FTP archive](https://www.3gpp.org/ftp/Specs/archive/38_series/38.213/)

| 관련 조항 | 원문 발췌 | Chen→NR 모델에서 결정하는 것 |
|---|---|---|
| §7 | “Uplink power control” | 추상 송신전력 `p_m^T`를 PUSCH power-control 절차로 교체 |
| §7.1.1 | “UE behaviour” | UE의 PUSCH power 산정 동작 |
| §8.3 | “PUSCH scheduled by RAR UL grant” | random access 이후 UL grant에 따른 PUSCH scheduling 절차 |

짧은 원문 발췌: “Uplink power control determines a power for PUSCH, PUCCH, SRS, and PRACH transmissions.”

**모델 연결:** Chen 식의 `p_m^T`는 자유롭게 주는 한 개의 연속 최적화 변수가 아니다. NR에서는 최대전력, path loss, PRB 수, TPC 등 제약 아래 PUSCH power가 정해진다. 이 규격은 PHY procedure를 정할 뿐, semantic KB mismatch 판정은 정하지 않는다.

---

## 4. 3GPP TS 38.214 — Physical layer procedures for data

공식 규격 페이지: [3GPP portal — TS 38.214](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3216)  
공식 archive: [38.214 FTP archive](https://www.3gpp.org/ftp/Specs/archive/38_series/38.214/)

| 관련 조항 | 원문 발췌 | Chen→NR 모델에서 결정하는 것 |
|---|---|---|
| §6.1.2 | “Time domain resource allocation” | 어느 slot/mini-slot의 몇 OFDM symbol을 PUSCH에 쓰는가 |
| §6.1.3 | “Frequency domain resource allocation” | PRB allocation, 즉 원 논문 `W`의 주파수축 구체화 |
| §6.1.4.1 | “MCS index table for PUSCH” | MCS index를 modulation order와 target code rate로 바꿈 |
| §6.1.4.2 | “Transport block size determination” | allocation+MCS로 한 grant의 TBS를 산출 |
| §6.1.5 | “PUSCH-related procedure” | PUSCH data procedure의 세부 규칙 |

**모델 연결:** 이 규격이 Shannon rate를 실제 NR의 ‘grant당 TBS’로 바꾸는 중심이다. `W log2(1+SINR)`만으로는 PRB/OFDM symbol/DM-RS/MCS에 따른 정수 TBS와 전송 시점을 얻을 수 없다.

---

## 5. 3GPP TS 38.300 — NR and NG-RAN overall description; Stage-2

공식 규격 페이지: [3GPP portal — TS 38.300](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3191)  
공식 archive: [38.300 FTP archive](https://www.3gpp.org/ftp/Specs/archive/38_series/38.300/)

원문 표제: “NR; NR and NG-RAN Overall description; Stage-2”

| 관련 부분 | 원문 표제/조항명 | Chen→NR 모델에서 결정하는 것 |
|---|---|---|
| 표제 | “NR and NG-RAN Overall description; Stage-2” | 원 논문의 MD/SBS가 실제 NR에서는 UE/gNB가 되는 시스템 경계 |
| §4 | “Overall architecture” | NG-RAN, 5GC, gNB의 큰 구조 |
| §6 | “NR radio protocol architecture” | application data가 SDAP/PDCP/RLC/MAC/PHY를 통과하는 protocol stack |
| §7 | “NR radio interface protocol” | NR 무선 interface에서 계층이 수행하는 역할 |

**모델 연결:** KB update와 semantic/bit task data는 application-level payload이다. TS 38.300은 이것이 UE–gNB 구간에서 어떤 radio protocol architecture를 통과하는지의 구조를 제공한다. 반면 KB 자체의 의미, SBS 간 knowledge sharing 알고리즘, semantic encoder의 압축비 `ξ_mn`는 이 규격의 범위 밖이다.

---

## 모델 구현 시 5개 규격을 사용하는 순서

1. **TS 38.300**으로 MD/SBS 표기를 UE/gNB 및 protocol stack에 대응시킨다.
2. **TS 38.214**로 각 slot에서 PRB, symbol, MCS, TBS를 산출한다.
3. **TS 38.212**로 TBS가 UL-SCH LDPC/rate matching을 거쳐 PUSCH coded bits가 되는 과정을 둔다.
4. **TS 38.211**로 그 coded bits를 PUSCH, PRB, OFDM symbol, DM-RS에 매핑한다.
5. **TS 38.213**으로 path loss와 power control을 반영한 PUSCH 송신전력을 계산한다.

그 결과 KB update의 완료시간은 `payload/R` 하나가 아니라, **grant 대기 + TBS 분할 수 + PUSCH 전송 구간 + BLER/HARQ 재전송**으로 계산할 수 있다. HARQ의 상세 동작은 TS 38.321(MAC)도 함께 참조해야 하므로, 이 5개 규격만으로 HARQ timing을 완결하지는 않는다.

## 공식 출처 목록

- 3GPP, TS 38.211, *NR; Physical channels and modulation*.
- 3GPP, TS 38.212, *NR; Multiplexing and channel coding*.
- 3GPP, TS 38.213, *NR; Physical layer procedures for control*.
- 3GPP, TS 38.214, *NR; Physical layer procedures for data*.
- 3GPP, TS 38.300, *NR; NR and NG-RAN Overall description; Stage-2*.
