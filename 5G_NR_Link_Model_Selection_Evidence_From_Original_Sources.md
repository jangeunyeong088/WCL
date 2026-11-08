# 5G NR 링크 모델 선택: 원문 발췌 기반 설득 근거

## 발표에서 먼저 밝혀야 할 사실

Chen, Fang, Wang (2025)은 **5G NR을 명시하지 않았다.** 논문은 MD–SBS 사이의 무선 전송을 Shannon rate로 추상화했다. 따라서 아래 근거의 역할은 “원 논문이 5G NR을 사용했다”를 증명하는 것이 아니라, **원 논문의 KB update 지연 문제를 실제 셀룰러 무선 전송 단위로 확장하려면 왜 NR PHY/MAC 링크 모델이 필요한가**를 원문으로 뒷받침하는 것이다.

---

## 1. Chen 원문에서 직접 발췌할 근거

### 근거 A — 연구 대상은 MD가 SBS로 보내는 multi-cell uplink이다

**원문 위치:** Chen et al., Section II, p. 14366, Fig. 1 인접 본문.

> “we consider a multi-cell multi-user network, consisting of N SBSs … and M MDs within the coverage of multiple SBSs.”

> “The MBS serves as a central coordinator that can communicate with the SBSs through reliable backhaul links.”

**이 문장으로 말할 수 있는 사실:** 연구의 무선 구간은 MD와 SBS 사이이고, MBS–SBS 구간은 reliable backhaul로 따로 둔다. 그러므로 실제 무선 링크 모델을 넣는 대상은 MBS–SBS가 아니라 **MD→SBS uplink**다.

---

### 근거 B — 원 논문의 링크 rate는 NR 자원·패킷 절차가 빠진 Shannon 추상식이다

**원문 위치:** Chen et al., Section II, p. 14367, Eq. (2).

> `R_{m,n} = W log_2(1 + p_m^T g_{m,n}/σ²)`

> “W denotes the pre-allocated subchannel bandwidth to transmit the requested data.”

**원문이 정의한 변수:**

| 원문 변수 | 원문 의미 |
|---|---|
| `W` | pre-allocated subchannel bandwidth |
| `p_m^T` | MD transmission power |
| `g_{m,n}` | MD m에서 SBS n으로의 link gain |
| `σ²` | noise power |

**이 식으로 말할 수 있는 사실:** 원 논문은 대역폭·송신전력·link gain을 넣어 연속값 rate `R_{m,n}`를 만든다. 여기에는 PRB, OFDM symbol, MCS, TBS, coding block, CRC, BLER, HARQ round가 변수로 존재하지 않는다. 따라서 `data/R_{m,n}`은 **추상 transmission time**이며 NR grant 단위 완료시간이 아니다.

---

### 근거 C — KB update에는 resource overhead와 latency가 실제 제약이다

**원문 위치:** Chen et al., Introduction, p. 14365.

> “Due to the incurred resource overhead and increased latency, the necessary knowledge updating and sharing … cannot always be guaranteed.”

> “Knowledge updating and sharing are crucial in semantic communications to maintain the perfect alignment of KBs.”

**이 문장으로 말할 수 있는 사실:** 논문 자체가 KB update의 자원소모와 latency를 연구 문제로 둔다. 따라서 update가 단지 이상적인 bit rate로 전달된다고 놓지 않고, 실제 uplink의 자원 할당·전송 성공·재전송과 연결하는 확장은 연구문제와 직접 연결된다.

---

### 근거 D — 원 논문은 target task의 delay requirement를 제약으로 사용한다

**원문 위치:** Chen et al., Abstract, p. 14364; Introduction, p. 14365.

> “satisfying the semantic accuracy and delay requirements of target tasks.”

> “semantic extraction ratio … affect[s] … task completion time.”

**이 문장으로 말할 수 있는 사실:** 링크 모델은 단순 throughput 비교용이 아니라, KB update와 과업의 **deadline/완료시간**을 판정하는 모델이어야 한다. 그래서 단순 `W` 값보다 slot, scheduling, TBS와 오류·재전송에 따른 actual delivery time이 필요한 것이다.

---

### 근거 E — Chen이 사용한 channel은 NR waveform 모델이 아니다

**원문 위치:** Chen et al., Section IV, p. 14375–14376 및 Table I.

> “Both distance-based path loss and small-scale fading are considered in link gains.”

> “ρ_{m,n} is the additional Rayleigh distributed … small-scale channel fading.”

**이 문장으로 말할 수 있는 사실:** 원 논문은 path loss와 Rayleigh fading을 link gain `g_{m,n}`에 넣었다. 이는 채널 추상화이며, NR numerology/PUSCH/DM-RS/MCS 기반 PHY signal model을 이미 구현했다는 뜻은 아니다.

---

## 2. 3GPP 원문 조항: 추상 링크를 NR 링크로 바꾸는 직접 근거

아래의 인용은 표준 조항명 또는 짧은 원문 표제다. 표준 전체 문구는 공식 3GPP 링크에서 대조한다.

| 표준·조항 | 원문 표제/짧은 발췌 | Chen의 추상 변수에서 NR 링크로 바꾸는 대상 |
|---|---|---|
| [TS 38.300](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3191), §4, §6 | “Overall architecture”; “NR radio protocol architecture” | MD/SBS 표기를 UE/gNB 및 radio protocol stack의 실제 시스템 경계로 대응 |
| [TS 38.211](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3213), §4.2, §6.3.1, §6.4.1.1 | “Numerologies”; “Physical uplink shared channel”; “Demodulation reference signal for PUSCH” | `W`와 `g_{m,n}`을 PUSCH PRB·symbol·DM-RS 기반의 물리 전송으로 구체화 |
| [TS 38.212](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3214), §6.2.4, §6.2.5 | “Channel coding of UL-SCH”; “Rate matching” | KB update bit가 UL-SCH transport block에서 LDPC/rate matching을 거쳐 PUSCH coded bit가 됨 |
| [TS 38.213](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3215), §7 | “Uplink power control” | 연속 최적화 변수 `p_m^T`를 최대전력·path loss·TPC 등의 PUSCH power procedure로 구체화 |
| [TS 38.214](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3216), §6.1.2–§6.1.4.2 | “Time domain resource allocation”; “MCS index table for PUSCH”; “Transport block size determination” | `R_{m,n}`을 slot별 time/frequency allocation, MCS, 정수 TBS로 구체화 |

### 표준 문장으로 확정되는 모델 변화

Chen 식의 `W log₂(1+SINR)`는 어느 대역폭에서 어느 정도의 이론 rate가 가능한지를 나타낸다. 그러나 TS 38.214 §6.1.2–§6.1.4.2는 실제 NR 상향 전송을 **시간 자원 + 주파수 자원 + PUSCH MCS + transport block size**로 정한다. 즉 KB update의 payload가 언제 얼마나 실리는지를 계산하려면 이 조항들이 필요한 것이다.

TS 38.212 §6.2.4–§6.2.5는 그 TBS가 UL-SCH channel coding과 rate matching을 거친다는 것을 정하고, TS 38.211 §6.3.1은 그 coded bits의 실제 physical channel이 PUSCH임을 정한다. 그러므로 Chen의 MD→SBS wireless leg를 NR로 구체화했을 때의 경로는 다음과 같다.

`KB update payload → UL-SCH transport block → LDPC/rate matching → PUSCH → gNB`

---

## 3. HARQ와 BLER에 관해 발표에서 정확히 말할 문장

초기의 “38.211–38.214·38.300만으로 HARQ까지 정의한다”는 표현은 엄밀하지 않다.

- **PUSCH, resource allocation, MCS, TBS**: TS 38.211·38.212·38.213·38.214.
- **HARQ entity, HARQ process와 retransmission 처리**: [TS 38.321](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3194) (MAC specification)도 함께 인용해야 한다.
- **BLER target**: 3GPP가 모든 link-level simulation에 하나의 보편 BLER 값을 강제하는 것이 아니다. 논문/시뮬레이터가 목표 BLER, link adaptation 및 channel model을 별도 설정한다.

따라서 발표에서는 이렇게 말하는 것이 정확하다.

> 본 모델은 TS 38.211–38.214와 TS 38.300에 따라 MD–SBS 추상 uplink를 UE–gNB PUSCH transmission으로 대응시킨다. MCS와 TBS로 grant당 payload를 산출하고, 오류·재전송까지 포함하는 reliability 확장에서는 TS 38.321의 HARQ procedure 및 별도 BLER/channel-model 가정을 추가한다.

---

## 4. 최종 설득 논리: 원문 → 모델 선택

| 논증 단계 | 직접 원문 근거 | 결론 |
|---|---|---|
| 1 | Chen Section II: MD–SBS multi-cell wireless network | NR로 구체화할 대상은 MD→SBS 구간 |
| 2 | Chen Eq. (2): `R_{m,n}=W log₂(1+SINR)`; “pre-allocated subchannel bandwidth” | 원 링크는 Shannon rate 기반의 추상 모델 |
| 3 | Chen Introduction: knowledge updating에 “resource overhead and increased latency” | KB update 전달시간·성공 여부가 문제의 핵심 변수 |
| 4 | Chen Abstract/Introduction: delay requirements, task completion time | 링크를 actual delivery time까지 표현할 필요가 있음 |
| 5 | TS 38.211–38.214·38.300의 PUSCH/UL-SCH/MCS/TBS/architecture 조항 | 실제 cellular uplink 단위로 계산할 수 있는 3GPP NR 표준 모델을 선택 |
| 6 | TS 38.321 | HARQ 재전송까지 확장할 때의 MAC layer 근거 |

## 5. 질의응답용 최종 답변

> Chen 원 논문은 MD–SBS 간 무선 전송을 `W log₂(1+SINR)`로 표현하고, KB update의 resource overhead와 latency, 그리고 과업의 delay requirement를 문제의 제약으로 둡니다. 다만 그 식에는 실제 무선 전송의 slot, PRB, MCS, transport block, coding, retransmission이 포함돼 있지 않습니다. 본 연구는 KB update의 완료시간과 전송 성공 여부를 셀룰러 링크 단위로 분석하기 위해, 그 MD–SBS 구간을 3GPP NR의 UE–gNB PUSCH로 구체화했습니다. TS 38.214의 resource allocation/MCS/TBS, TS 38.212의 UL-SCH coding, TS 38.211의 PUSCH를 사용해 추상 rate를 실제 전송 단위로 바꿉니다. HARQ까지 다루는 경우에는 TS 38.321을 추가로 적용합니다.

## 참고문헌

1. H. Chen, F. Fang, and X. Wang, “Knowledge Sharing-Enabled Semantic Rate Maximization for Multi-Cell Task-Oriented Hybrid Semantic-Bit Communication Networks,” *IEEE Transactions on Communications*, vol. 73, no. 12, pp. 14364–14380, Dec. 2025, doi: [10.1109/TCOMM.2025.3606647](https://doi.org/10.1109/TCOMM.2025.3606647).
2. 3GPP TS 38.211, *NR; Physical channels and modulation*.
3. 3GPP TS 38.212, *NR; Multiplexing and channel coding*.
4. 3GPP TS 38.213, *NR; Physical layer procedures for control*.
5. 3GPP TS 38.214, *NR; Physical layer procedures for data*.
6. 3GPP TS 38.300, *NR; NR and NG-RAN Overall description; Stage-2*.
7. 3GPP TS 38.321, *NR; Medium Access Control (MAC) protocol specification*.
