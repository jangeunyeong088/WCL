# Chen 원 논문의 링크 모델을 5G NR 링크로 구체화한 지점

## 결론

Chen·Fang·Wang의 원 논문은 **5G NR 무선 링크를 모델링하지 않았다.** 원 논문이 둔 것은 MD–SBS 사이의 일반적인 단일 무선 링크와 Shannon 전송률이다. 발표자료의 “5G NR” 부분은 그 일반 링크를 **UE–gNB 상향 PUSCH 기반 링크로 다시 표현한 추가 모델**이다.

원 논문: Chen, C., Fang, F., and Wang, Y., “Knowledge Sharing-Enabled Semantic Rate Maximization for Multi-Cell Task-Oriented Hybrid Semantic-Bit Communication Networks,” *IEEE Transactions on Communications*, 2025, doi: [10.1109/TCOMM.2025.3606647](https://doi.org/10.1109/TCOMM.2025.3606647).

## 1. 원 논문이 실제로 둔 통신 모델

원 논문 Section II의 시스템은 다수의 MBS, SBS 및 MD로 구성된다.

| 구간 | 원 논문 명시 | 모델링 방식 |
|---|---|---|
| MBS–SBS | 신뢰성 높은 유선 backhaul | 지식 공유를 위한 충분한 backhaul 용량 가정 |
| MD–SBS | 무선 uplink | MD가 SBS에 KB update 또는 과업 데이터를 보냄 |
| 채널 | `g_mn` | 거리 기반 대규모 감쇠 + Rayleigh 소규모 페이딩 |
| 대역폭 | `W` | MD에 사전 할당된 subchannel bandwidth |
| 전송률 | Eq. (1) | `R_mn = W log2(1 + p_m^T g_mn / σ²)` |
| 시간 변화 | Section III | quasi-static block fading; time slot 안에서는 채널이 변하지 않는다고 둠 |

따라서 원 논문에는 아래 NR 구성요소가 없다.

`5G NR`, `UE`, `gNB`, `PUSCH`, `PRB`, `numerology`, `slot`, `MCS`, `TBS`, `LDPC`, `BLER`, `HARQ`, `DM-RS`, `OFDMA`, `MIMO`.

원 논문의 “subchannel”과 Shannon rate는 자원·PHY 상세를 추상화한 수학적 링크 모델이다. `W`는 NR의 PRB 또는 BWP라고 명시되지 않았고, `R_mn`은 NR TBS 처리량으로 산출되지 않았다.

## 2. 원 논문의 데이터 흐름

원 논문에는 통신 구간과 데이터 종류가 다음처럼 분리되어 있다.

| 데이터 | 송신자 → 수신자 | 원 논문 표기 | 통신 의미 |
|---|---|---|---|
| KB update | MD → SBS | `d_mn^K` | MD에서 바뀐 knowledge를 SBS의 KB에 반영 |
| semantic task data | MD → SBS | `ξ_mn d_mn^T` | KB가 일치할 때 semantic encoder가 줄인 데이터 |
| bit task data | MD → SBS | `d_mn^T` | KB 불일치 시 원 비트 데이터 |
| shared knowledge | MBS → SBS | — | 신뢰성 유선 backhaul을 통한 SBS 간 knowledge sharing |

각 MD–SBS 데이터는 모두 Eq. (1)의 동일한 추상 무선률 `R_mn`으로 delay/semantic-rate 최적화에 들어간다. 원 논문은 데이터 종류별 NR bearer, logical channel, transport channel 또는 physical channel을 따로 정의하지 않았다.

## 3. “원 논문 링크 → 5G NR 링크”로 바뀌는 정확한 지점

아래 오른쪽 열은 Chen 원문의 내용이 아니라, NR 물리·MAC 층 변수로 바꾸기 위해 새로 넣어야 하는 구체화이다.

| Chen 원문 모델 | NR로 구체화할 때의 대응 | 새로 생기는 NR 변수/절차 |
|---|---|---|
| MD | UE | UE identity, serving cell, RRC/MAC 상태 |
| SBS | gNB | serving gNB, scheduler |
| MD→SBS wireless uplink | UE→gNB uplink | UL-SCH → PUSCH 전송 |
| `W` (pre-allocated bandwidth) | 할당된 PRB 수 및 BWP 대역폭 | `N_PRB`, SCS, slot/mini-slot, time-domain allocation |
| `p_m^T` | PUSCH 송신전력 | NR UL power control, `P_CMAX`, path loss, TPC |
| `g_mn` | PUSCH 유효 채널 | path loss, fading, DM-RS 기반 채널추정, 필요 시 MIMO channel matrix |
| `R_mn = W log2(1+SINR)` | 수신 가능한 transport-block 처리량 | MCS, modulation order, code rate, TBS, allocated RE/PRB |
| 한 번의 추상 전송 | 전송 블록 단위 송신 | CRC, LDPC, rate matching, modulation, PUSCH mapping |
| 오류 없는 Shannon rate | block error가 있는 링크 | BLER target, HARQ retransmission, scheduling/HARQ timing |
| time slot의 block fading | NR time unit | numerology `μ`, slot, symbol 또는 mini-slot |

## 4. 식이 어떻게 달라지는가

### 원 논문

원 논문의 delay는 보내야 할 데이터량을 `R_mn`으로 나누는 방식이다. `R_mn`은 연속값 Shannon capacity이므로, 정수 PRB·정수 OFDM symbol·정수 transport block·재전송이라는 NR 패킷성은 나타나지 않는다.

### NR 링크로 바꾼 뒤

1. gNB scheduler가 UE에 PUSCH 시간·주파수 자원과 MCS를 grant한다.
2. KB update 또는 task data가 UL-SCH transport block으로 들어간다.
3. UL-SCH에 CRC, LDPC coding, rate matching, scrambling과 modulation을 적용한다.
4. PUSCH가 선택된 PRB와 OFDM symbol에 매핑되고 DM-RS가 함께 송신된다.
5. gNB가 CRC 결과로 ACK/NACK를 내고, NACK이면 HARQ 재전송이 생긴다.
6. 링크 delay는 payload만의 `D/R`가 아니라 grant 대기, TBS 단위 채움, 전송 구간, HARQ 왕복 및 재전송을 포함한다.

이를 측정식으로 쓰면, 유효 goodput은 단순 Shannon `R_mn` 대신 예를 들어

`G_eff = delivered_payload_bits / (scheduling_wait + all_transmission_time + HARQ_retransmission_time)`

로 계산할 수 있다. 여기서 실제 전송 가능 bit 수는 `N_PRB`, symbol 수, DM-RS 등 overhead, MCS, TBS와 BLER/HARQ 결과에 의해 정해진다.

## 5. 기존 발표의 “NR 링크”가 뜻하는 범위

발표자료의 “one MD, NR link, time axis added”는 원 논문의 다중 셀 최적화 전체를 그대로 3GPP NR 규격 모델로 바꾼 것이 아니라, 다음을 추가한 재구성 범위로 읽힌다.

| 유지한 것 | NR로 바꿔야 하는 것 | 원 논문에 없어서 별도 결정이 필요한 것 |
|---|---|---|
| KB update/semantic/bit 전환 논리 | MD–SBS → UE–gNB | numerology, carrier/BWP, PRB 수, TDD pattern |
| KB mismatch가 bit fallback을 유발한다는 논리 | abstract rate → PUSCH/TBS goodput | MCS selection, BLER target, HARQ process/round limit |
| update data량·task data량 | delay calculation | channel model, mobility, MIMO/antenna, DM-RS overhead |
| knowledge-sharing concept | wireless UL leg | RLC/PDCP/SDAP 및 backhaul mapping |

즉 NR로 재현하려면 **NR의 PHY/MAC을 명시적으로 넣어야 한다.** 반대로 KB 의미, mismatch 판정, semantic encoder/decoder, MBS–SBS knowledge sharing 최적화는 3GPP TS 38.xxx가 정하는 기능이 아니라 Chen 원 논문이 정한 application/semantic layer 기능이다.

## 6. 발표에서 사실대로 말할 수 있는 문장

> Chen 원 논문은 MD–SBS 구간을 Shannon 전송률 기반의 일반 무선 uplink로 추상화했다. 본 재구성에서는 그 구간을 UE–gNB 상향 PUSCH로 대응시키고, 대역폭 변수 `W`를 NR 자원할당·MCS·TBS·BLER/HARQ가 반영된 전송 goodput으로 구체화한다.

> 따라서 NR PHY/MAC은 원 논문의 가정이 아니라, KB update delay와 reliability를 실제 셀룰러 링크 단위로 산출하기 위해 추가한 링크 계층 모델이다.

## 7. 추적 가능한 출처

- Chen et al., 원 논문: Section II (System Model), Eq. (1), Section III (KB updates and data transmission), Table I.
- 3GPP TS 38.211: PUSCH 및 OFDM resource mapping.
- 3GPP TS 38.212: UL-SCH LDPC coding 및 rate matching.
- 3GPP TS 38.213: PUSCH power control 및 physical-layer procedures.
- 3GPP TS 38.214: PUSCH resource allocation, MCS, TBS.
- 3GPP TS 38.300: NR/NG-RAN architecture와 protocol overview.
