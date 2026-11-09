# 「Uplink Delay and Reliability Analysis of KB Updates … over 5G NR」 슬라이드별 발표 대본

## 발표의 중심 문장

**Chen 논문은 KB update가 필요한 다중 셀 semantic/bit 통신을 제시했지만, MD–SBS 무선 구간을 Shannon rate 하나로 표현했다. 본 연구는 그 구간을 NR PUSCH/OFDM 기반 uplink로 구체화하여, KB update가 실제로 언제 적용되는지와 그 사이에 발생하는 bit fallback을 정량화한다.**

아래 대본은 본발표 14장 기준이다. 괄호 속 문장은 슬라이드에서 손으로 가리키며 강조할 부분이고, `→` 다음은 다음 슬라이드로 넘기는 연결 문장이다.

---

## Slide 1. Title

### 말할 대본

안녕하세요. 장은영입니다. 이번 발표에서는 과제지향 hybrid semantic/bit 통신에서 KB, 즉 knowledge base update가 상향 무선 링크를 통해 언제 실제로 적용되는지를 다룹니다.

출발점은 Chen 등의 다중 셀 semantic/bit 통신 논문입니다. 이 논문은 KB mismatch가 발생하면 의미 정보 대신 raw bit를 보내도록 설계했습니다. 그런데 KB update가 무선 링크에서 지연되면, update가 도착하기 전까지 이후 과업들이 계속 raw bit 방식으로 전송될 수 있습니다.

본 연구는 이 시간을 **unapplied time**, 즉 지식이 바뀐 뒤 기지국에 실제 적용되기 전까지의 시간으로 정의하고, 이를 5G NR uplink 조건에서 분석하는 연구입니다.

오늘 발표의 핵심 질문은 하나입니다. **KB update가 NR 상향 링크에서 제때 적용되지 않을 때, semantic communication의 traffic-saving 이득이 얼마나 사라지는가**입니다.

→ 먼저 이 문제가 semantic communication에서 왜 생기는지부터 설명드리겠습니다.

---

## Slide 2. Semantic communication needs the same KB on both sides

### 말할 대본

semantic communication은 원시 bit 전체를 보내는 대신, 과업에 필요한 의미만 추출하여 전송합니다. 수신 측은 송신 측과 공유한 KB를 이용해 그 의미를 복원합니다.

따라서 이 방식은 전송량을 줄일 수 있지만, 전제조건이 있습니다. **송신 UE와 수신 edge server, 또는 SBS가 같은 지식을 가져야 한다는 것**입니다.

(오른쪽 세 단계를 가리키며) 송신 측은 task-relevant meaning을 추출하고, 수신 측은 자기 KB로 이를 복원합니다. 두 KB가 다르면, 수신 측은 압축된 의미 정보를 원래 과업 데이터로 올바르게 해석할 수 없습니다.

그래서 semantic communication의 성능은 encoder 자체뿐 아니라, KB를 갱신하고 동기화하는 control layer에 의존합니다. 본 연구는 이 control layer 중에서도 **무선 KB update의 지연**을 봅니다.

→ KB가 실제 환경에서 계속 달라진다는 점이 다음 문제입니다.

---

## Slide 3. Open problem: knowledge sync under dynamic conditions

### 말할 대본

KB는 한 번 맞춰 놓으면 고정되는 정보가 아닙니다. 새로운 과업, 사용자 요구 변화, 환경 변화, 로컬 학습의 차이 때문에 송신 UE와 SBS의 KB는 계속 달라집니다.

(왼쪽 표를 가리키며) 여러 semantic communication application domain에서 KB dependency가 높게 나타나는 이유도 이것입니다. KB mismatch는 예외가 아니라, 동적인 서비스 환경에서 지속적으로 생기는 상태입니다.

기존 survey들은 KB synchronization이 중요하다고 지적하지만, 보통 평균 semantic accuracy나 평균 traffic reduction을 주로 봅니다. 반면 **변경된 knowledge가 실제로 몇 초 안에 적용되는지**, 그리고 그 미적용 구간에서 과업이 몇 건 도착하고 raw-bit traffic이 얼마나 늘어나는지는 충분히 정량화되지 않았습니다.

따라서 저는 “KB가 불일치하는가”보다 한 단계 더 구체적으로, **변경 이후 적용까지 걸리는 시간의 분포와 tail**을 보려고 합니다.

→ 이를 수식과 데이터 흐름까지 가진 기준 시스템에서 시작하겠습니다.

---

## Slide 4. Chen's multi-cell hybrid semantic/bit system

### 말할 대본

기준 시스템은 Chen 등의 multi-cell hybrid semantic/bit network입니다.

(왼쪽 그림을 가리키며) MD, 즉 모바일 단말이 task data와 자신의 knowledge class를 가지고 있고, SBS에는 semantic decoder, KB, cloudlet이 있습니다. MBS는 SBS들을 조정하고 reliable backhaul로 연결합니다.

여기서 가장 중요한 무선 구간은 MD에서 SBS로 올라가는 uplink입니다. 이 한 uplink를 통해 세 종류의 데이터가 모두 전송됩니다. 첫째 KB upload (d_K), 둘째 KB가 일치하는 class의 semantic data (ξ d_T), 셋째 SBS에 없는 knowledge class에 대응하는 raw bit data (d_T)입니다.

(오른쪽 아래 link 식을 가리키며) 원 논문은 이 무선 링크를

\[
R_{m,n}=W\log_2\left(1+\frac{p_m^Tg_{m,n}}{\sigma^2}\right)
\]

로 모델링합니다. 즉 대역폭, 송신전력, channel gain으로 계산한 Shannon rate 하나입니다.

이 식에는 PRB, OFDM symbol, PUSCH, MCS, transport block, BLER, HARQ가 없습니다. 따라서 본 연구에서는 **Chen의 KB mismatch 논리와 data flow는 유지하되, 바로 이 MD–SBS uplink만 NR PUSCH/OFDM 링크로 구체화**합니다.

→ 먼저 원 논문에서 세 종류의 전송이 어떻게 같은 uplink를 공유하는지 보겠습니다.

---

## Slide 5. Chen's system: one uplink shared by three transmissions

### 말할 대본

이 그림은 연구의 병목을 간단히 보여줍니다.

한 MD에서 SBS로 가는 uplink는 하나이고, 여기에 KB upload, semantic transmission, raw-bit transmission이 모두 들어갑니다.

KB class가 SBS의 usable KB 집합 (K^{Cu}) 안에 있으면 과업 데이터는 semantic ratio (ξ)만큼 줄어든 semantic data로 전송됩니다. 반대로 (K^{Cu}) 밖이면 원시 데이터 (d_T) 전체를 bit communication으로 보냅니다.

Chen의 결정은 task마다 독립적으로 이루어집니다. upload를 하고, 해당 task의 deadline 안에서 semantic ratio와 SBS association을 정합니다. 채널도 해당 block 동안 quasi-static으로 둡니다.

본 연구의 추가점은 여기입니다. knowledge가 바뀐 시점부터 update가 수신·검증·적용되는 시점까지의 시간축을 넣고, 그 사이에 다음 task들이 도착하도록 합니다. 그래서 “한 task 안에서 upload가 끝났는가”가 아니라, **다음 task가 도착했을 때 KB가 실제로 적용되었는가**를 봅니다.

→ 적용이 늦을 때 실제 cost가 얼마나 큰지 보겠습니다.

---

## Slide 6. Cost of mismatch: Bit fallback, about 6.5× the data

### 말할 대본

KB mismatch가 이 시스템에서 의미하는 것은 task failure 자체가 아닙니다. Chen의 설계에서는 SBS에 없는 class를 raw bit 방식으로 보내서 과업 수행을 보장합니다.

그러나 traffic cost가 커집니다. 이 슬라이드의 약 6.5배는 accuracy requirement가 0.8일 때 필요한 최소 semantic extraction ratio가 ξ=0.153이라는 Chen 식의 조건에서 계산됩니다. semantic mode에서는 대략 (0.153d_T)를 보내지만, KB가 아직 적용되지 않았으면 (d_T) 전체를 보냅니다.

따라서 KB update가 늦으면 그 시간 동안 도착한 task는 semantic transmission의 이득을 받지 못하고 raw bit fallback을 사용합니다. 이 raw data와 KB upload 자체가 같은 uplink 자원을 경쟁합니다.

여기서 NR 링크가 필요해집니다. update와 data가 실제 PUSCH grant, PRB, MCS, HARQ를 공유하면, update의 실패나 재전송이 바로 다음 task의 bit fallback 시간을 늘릴 수 있기 때문입니다.

→ 그래서 본 연구의 범위는 KB mismatch 전체가 아니라, 바로 이 bit fallback 경로와 그 시간축입니다.

---

## Slide 7. Scope: the Bit fallback path and unapplied time Δ

### 말할 대본

이 슬라이드에서 (a_k)는 knowledge class (k)가 바뀐 시점이고, (u_k)는 그 update가 SBS에서 실제 적용된 시점입니다. 저는 이 차이를

\[
\Delta_k=u_k-a_k
\]

로 정의합니다.

(Delta_k) 구간에 task가 도착하면 그 class는 아직 semantic mode로 해석될 수 없으므로 raw bit로 전송됩니다. 반대로 (u_k) 이후 도착한 task는 semantic transmission을 사용할 수 있습니다.

링크 모델은 최종적으로 다음 NR PUSCH chain을 사용합니다.

\[
\text{KB update bits}
\rightarrow \text{UL-SCH/LDPC}
\rightarrow \text{QAM}
\rightarrow \text{PUSCH resource grid}
\rightarrow \text{OFDM IFFT+CP}
\rightarrow \text{multi-cell channel}
\rightarrow \text{FFT/DM-RS equalization}
\rightarrow \text{CRC/HARQ}.
\]

이 chain에서 numerology, PRB, OFDM symbol, DM-RS overhead, MCS와 TBS가 grant당 전달 가능한 bit 수를 정합니다. CRC failure면 HARQ retransmission이 생기고, 이 시간이 (Delta_k)를 늘립니다.

현재 예비 결과는 이 전체 waveform chain을 직접 sample-level로 생성한 결과가 아니라, NR의 spectral-efficiency cap, BLER, retransmission 조건을 반영한 link abstraction 결과입니다. 따라서 오늘의 수치는 **NR condition에서 Chen의 Shannon assumption이 어떤 차이를 만드는지 확인한 예비 타당성 결과**입니다. OFDM waveform, DM-RS channel estimation 및 다중 셀 interference는 최종 링크 시뮬레이터에 포함합니다.

그리고 80 Mbit update를 1초 안에, 99.9% 확률로 적용할 수 있는가를 service criterion으로 둡니다.

→ 이 time axis를 넣으면 기존 방식에서 세 가지 충돌이 생깁니다.

---

## Slide 8. One uplink: whichever goes first, something loses

### 말할 대본

첫 번째 충돌은 scheduling order입니다. KB update를 먼저 보내면 현재 task data가 늦어집니다. 반대로 task data를 먼저 보내면 KB update 적용이 늦어지고, 그 사이에 다음 task가 raw bit fallback을 사용합니다. 같은 uplink 자원 안에서 둘 다 즉시 만족시킬 수 없습니다.

두 번째는 tail입니다. KB data size는 16배 범위를 가지고, fading과 retransmission이 더해지므로 평균 update time만 보는 것은 부족합니다. 1초 이내 적용률 99.9%라는 tail requirement를 별도로 봐야 합니다.

세 번째는 원 논문의 link assumption입니다. Chen의 Shannon rate는 NR MCS cap을 적용한 전송률보다 약 2.7에서 3.7배 크게 나옵니다. 그러면 원 논문 조건에서는 task deadline 안에 끝난 것으로 보이던 update가, NR 조건에서는 더 오래 걸릴 수 있습니다.

기존 연구는 update class 선택, staleness 기반 bandwidth allocation, knowledge alignment를 다뤘지만, **update가 radio link에서 완료되기 전 도착한 task의 fallback cost를 time axis로 분리하여 측정하지는 않았습니다.**

→ 그래서 연구 질문은 완료시간의 분포와 그 경계조건으로 나뉩니다.

---

## Slide 9. Numbers that remain, and the research questions

### 말할 대본

첫 번째 연구 질문 RQ1은 temporal behavior입니다. knowledge가 바뀐 뒤 실제 적용될 때까지의 Δ 분포를 구합니다. 평균만 보지 않고, 1초 내 적용 확률, 99.9 percentile, 미적용 구간에 도착한 task 수, extra bit, task completion time을 함께 측정합니다.

두 번째 연구 질문 RQ2는 feasibility boundary입니다. spectral efficiency, BLER, knowledge update size (d_K), task arrival rate λ가 바뀔 때 어느 조건에서 1초·99.9% requirement가 만족되는지 찾습니다. 만족 경계에서 필요한 radio resource와 total uplink traffic도 계산합니다.

즉 RQ1은 “얼마나 오래 적용되지 않는가”, RQ2는 “어떤 radio condition에서 semantic communication의 이득이 synchronization cost를 이기는가”입니다.

→ 먼저 Chen의 수식과 파라미터를 재현한 뒤, NR 조건을 적용했을 때도 문제가 남는지 확인했습니다.

---

## Slide 10. Preliminary ① It remains

### 말할 대본

Chen의 Eq. (1)부터 Eq. (9), Eq. (12), Eq. (14), Eq. (15)와 Table I을 재현했고, Fig. 8에서는 original의 19% improvement에 대해 16.4%로 방향과 규모를 확인했습니다. Fig. 11도 knowledge size가 커질수록 sharing gain이 줄어드는 동일한 경향을 재현했습니다.

그 위에 NR condition link abstraction을 적용했을 때, 1초 안에 완료된 KB update는 40.8%였습니다. 중앙값은 1.14초이고 99th percentile은 3.03초입니다.

또 update가 올라가는 중에 다음 task가 도착할 확률은 21.0에서 64.2%였고, 그 task는 semantic mode 대신 raw-bit fallback을 써야 하므로 uplink traffic이 39.1 Mbit에서 138.0 Mbit로 약 3.5배 증가했습니다.

여기서 중요한 것은 이 차이가 단순히 link가 느려졌다는 결과가 아니라는 점입니다. instant-apply baseline과 비교했기 때문에, link-speed 자체의 불이익은 제외하고 **update가 아직 적용되지 않아 발생한 loss만** 따로 측정했습니다.

→ 다음은 기존 해법을 더 적용해도 이 loss가 사라지는지 확인한 결과입니다.

---

## Slide 11. Preliminary ② Checking existing schemes

### 말할 대본

네 가지 점검을 했습니다.

첫째, Chen의 기존 policy를 exact link condition에서 다시 최적화해도 1초 안에 완료된 upload 비율은 40.7%였습니다. 즉 문제는 원 policy의 파라미터를 대충 썼기 때문이 아니라, link condition을 넣어도 남았습니다.

둘째, knowledge sharing을 포기하고 bit-only로 보내면 2 bit/s/Hz에서 deadline 충족률이 26.1%로 떨어집니다. 따라서 raw-bit fallback만 사용하는 방식은 semantic communication의 원래 이득을 포기합니다.

셋째, BLER을 0.1에서 (10^{-5})로 더 엄격하게 해도 1초 내 적용률은 41.6%에서 48.7% 수준입니다. reliability만 높여도 update delay tail 자체가 사라지지는 않습니다.

넷째, 현재 task만 최대화하는 objective에서는 미적용 구간에 도착한 task의 extra traffic이 여전히 253% 남습니다.

또 data-first serial scheduling에서는 raw data가 update를 더 늦추고, 늦어진 update가 다시 raw data를 늘리는 feedback이 생깁니다. 이 때문에 full apply time이 1.23초에서 8.90초까지 길어지는 조건이 나타났습니다.

→ 따라서 upload 순서와 적용 단위 자체를 제어변수로 추가해 보았습니다.

---

## Slide 12. Preliminary ③ Control variables

### 말할 대본

여기서는 radio resource를 더 주지 않고, update를 어떻게 전송·적용할지만 바꿨습니다.

Baseline B는 Chen 방식처럼 전체 knowledge upload가 끝난 뒤 한 번에 적용합니다. P-chen은 Chen의 class ordering을 따르되 class별로 수신 즉시 적용합니다. P-spt는 KB update size가 작은 class부터 보내고 즉시 적용하는 shortest-processing-time 방식입니다. O는 upload 지연이 전혀 없는 instant-apply 기준선입니다.

동일한 link, task sequence, class set에서 비교하면, P-spt는 deadline-violation gap을 22.7에서 58.0% 줄였고, extra-Bit gap도 25.5에서 37.9% 줄였습니다. 1초 내 적용률은 최대 55.3%까지 올라갔습니다.

여기서 결론은 “P-spt가 최종 optimal”이라는 것이 아닙니다. **KB update의 전송순서와 class별 apply timing은, 추가 spectrum 없이도 Δ와 raw-bit fallback을 줄이는 실제 제어변수**라는 예비 근거입니다.

→ 마지막으로 현재 주장 범위와 다음 검증 단계를 명확히 말씀드리겠습니다.

---

## Slide 13. Claim scope, research plan and contributions

### 말할 대본

현재 주장 범위는 명확합니다. Chen의 real cellular and edge structure를 출발점으로 하여, single MD 환경에서 NR link condition을 반영한 KB update delay를 분석한 예비 연구입니다. 다중 UE contention, association 재최적화, apply-confirmation signaling, full HARQ round-trip, waveform-level OFDM receiver는 다음 단계입니다.

그래서 두 개의 falsification criterion을 둡니다. 첫째, exact link에서 기존 policy를 재최적화했는데도 1초·99.9%를 만족한다면 RQ2는 폐기합니다. 둘째, synchronization variable을 넣어도 deadline-violation gap이 전혀 줄지 않으면 제어 가능성 주장을 폐기합니다.

현재 결과는 두 기준 모두 아직 통과하지 못했습니다. 기존 policy의 1초 내 적용률은 40.7%이고, synchronization variable은 22.7에서 58.0%의 gap reduction을 보였습니다.

따라서 기여는 세 가지입니다. 첫째, KB update가 적용되지 않은 시간의 분포와 손실을 정량화합니다. 둘째, 1초·99.9% requirement를 만족하는 radio condition과 cost의 경계를 제시합니다. 셋째, update order와 apply unit이 loss를 줄이는 제어변수임을 보입니다.

→ 정리하면, 이 연구는 semantic communication의 traffic saving을 synchronization cost까지 포함해 평가하는 기준을 만드는 연구입니다.

---

## Slide 14. References

### 말할 대본

발표에 사용한 기준 논문은 Chen et al.의 2025년 IEEE Transactions on Communications 논문이고, NR link parameter와 procedure는 3GPP TS 38.211, 38.212, 38.213, 38.214, 38.300 및 HARQ를 위한 TS 38.321에 근거합니다.

이상으로 발표를 마치겠습니다. 감사합니다.

---

# Backup: 질문이 나오면 넘길 슬라이드별 답변

## Backup B1. Excluded conditions and reasons

> 본 단계에서는 single uplink의 update-to-apply time을 분리하기 위해 multi-MD contention, association 재최적화, downlink distribution, Wi-Fi, sidelink, satellite, commercial measurement를 제외했습니다. 이 요소들은 중요하지만 현재 질문인 “NR uplink에서 KB update가 적용되기 전 발생하는 fallback loss”와 다른 변수를 동시에 추가합니다.

## Backup B2. Chen's parameters and scenarios

> Chen의 Table I 파라미터를 재현 시작점으로 사용했습니다. KB data는 5–80 Mbit, raw task data는 20–100 Mbit, bandwidth는 10 MHz, transmission power는 0.1 W입니다. original link는 Rayleigh fading과 거리감쇠를 갖는 Shannon rate입니다. NR 구체화는 이 원 데이터를 유지하면서 delivery process만 PUSCH 조건으로 바꾸는 구조입니다.

## Backup B3. Original figures and reproduction details

> Fig. 8의 수치와 Fig. 11의 추세를 원 논문과 대조했습니다. 목적은 Chen의 semantic/bit logic을 새로 발명한 것이 아니라, 해당 logic 위에 NR link condition과 time axis를 얹기 전에 원식을 재현하는 것이었습니다.

## Backup B4. Operating-condition numbers

> 이 표의 수치는 문제 자체의 증거와 control-variable 결과를 분리하기 위한 것입니다. BLER 조건을 바꿔도 1초 내 적용률이 충분히 올라가지 않고, spectral efficiency가 낮아질수록 deadline violation이 커진다는 것이 핵심입니다.

## Backup B5. Re-run values vs original preliminary document

> fixed seed와 독립 재구현을 다시 수행해 수치를 확인했습니다. 일부 수치는 바뀌었지만, 1초 내 적용률 약 40%, update 진행 중 task arrival, 그리고 synchronization variable에 따른 gap reduction이라는 방향은 유지됩니다.

## Backup B6. Target systems

> 직접 적용 대상은 UE에서 edge server로 task를 offload하고, UE의 knowledge change가 update로 올라가며, update와 data가 같은 PUSCH 자원을 공유하는 cellular HetNet입니다. Wi-Fi, sidelink, satellite은 access structure가 다르므로 현재 모델의 직접 대상이 아닙니다.

## Backup B7. Fit as wireless communication research

> radio condition을 없애서 update가 즉시 적용되면 본 연구가 정의한 loss는 0이 됩니다. 즉 결과는 semantic model만의 효과가 아니라 wireless link의 resource allocation, retransmission 및 delay에 의해 생깁니다.

## Backup B8. Shared finding

> KB mismatch가 동적 task, 환경 변화, local learning 차이로 생긴다는 점은 Chen, Xu, Ma, Zhang, Lu, Tian 등의 여러 연구에서 공통적으로 확인됩니다. 본 연구의 차이는 mismatch 존재 자체보다, update가 무선 링크에서 언제 적용되는지를 분포로 본다는 점입니다.

## Backup B9. Two paths of mismatch

> 일부 문헌에서는 KB mismatch가 semantic interpretation failure를 일으키는 것으로 모델링합니다. Chen의 시스템에서는 raw bit fallback으로 task failure를 피합니다. 저는 Chen의 시스템 가정을 따르므로, mismatch의 비용은 task failure가 아니라 더 큰 traffic과 delay입니다.

## Backup B10. Existing solutions

> 기존 연구는 KB selection, staleness-aware bandwidth allocation, federated alignment 같은 방법을 제시합니다. 그러나 현재 연구의 decision unit은 “하나의 task와 그 시점의 KB”에서 “knowledge change 이후 update가 적용될 때까지의 시간”으로 확장됩니다.

## Backup B11. Reproduction

> 재현 범위는 Chen의 Eq. (1)–(9), (12), (14), (15)와 Table I입니다. multi-MD contention을 아직 넣지 않았으므로 absolute value보다 original figure와의 trend match를 중심으로 확인했습니다.

## Backup B12. Policy timeline

> 이 비교는 같은 link, 같은 task sequence, 같은 class set을 사용합니다. 따라서 결과 차이는 extra radio resource가 아니라 update ordering과 apply unit이라는 control variable에서 나옵니다.

## Backup B13. Claim scope: a system model under standard 5G conditions

> 현재 수치는 standard NR parameters를 반영한 link abstraction 결과입니다. 최종 모델은 PUSCH OFDM waveform, DM-RS channel estimation, multi-cell interference 및 HARQ round-trip을 포함합니다. 따라서 commercial 5G network에서 측정된 일반 성능이라고 주장하지 않으며, standard-parameter simulation에서 Chen의 Shannon link 가정이 만드는 update-time gap을 보인다는 것이 현재 주장 범위입니다.

---

# 꼭 고칠 표현 3개

1. Slide 4의 `system model built on real cellular and edge structure`는 그대로 두되, 발표 중에는 **“Chen의 시스템 구조가 real cellular/edge 구조를 참고했다는 뜻이지, 원 논문이 5G NR PHY를 구현했다는 뜻은 아닙니다”**라고 설명한다.
2. Slide 7의 `updates also go through PUSCH, HARQ, RLC ARQ`는 현재 예비결과에 대해 **“최종 NR link model에서 반영할 전송 chain”**으로 읽어야 한다. 현재 결과가 IFFT/CP/DM-RS receiver까지 구현됐다는 표현은 쓰지 않는다.
3. Slide 10의 `on NR`은 구두로 **“NR-parameterized link abstraction에서”**라고 말한다. 그래야 preliminary result와 final waveform model의 범위를 섞지 않는다.
