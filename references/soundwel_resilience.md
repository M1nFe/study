**SoundWel·행동·회복 연구의 읽기 순서와 근거**

[시작](../README.md) · [실행 계획](../docs/soundwel_execution.md) · [Lee 연결](../docs/lee2024_bridge.md)

검토일 2026-10-08. 빠른 실험·집필을 위한 선별 목록이며 체계적 고찰이나 분야 전체 검색 결과가 아니다. 논문 심사 여부와 확인 범위를 구분한다. 숫자·방법의 세부 인용은 최종 원문 위치를 확인한 뒤 원고에 넣는다.

| 읽는 시기 | 1차 문헌·자료 | 우리 연구에서 결정할 것 |
|---|---|---|
| D1 | Lee 등 (2024), [이유 스트레스와 놀람 반응](https://doi.org/10.1016/j.applanim.2024.106243), Applied Animal Behaviour Science | 반응 크기와 회복을 같은 개념으로 쓰지 않기. 이번에는 최종 초록·highlights 확인, 본문 세부값 미확인 |
| D1~2 | Briefer 등 (2022), [돼지 발성 맥락·valence 분류](https://pmc.ncbi.nlm.nih.gov/articles/PMC8901661/), Scientific Reports; [2023 정정](https://www.nature.com/articles/s41598-023-45242-9); [SoundWel 공개자료](https://zenodo.org/records/8252482) | 전체 연구와 공개 부분집합을 구별. 라벨·ID·특징·녹음 단위 원출처 확인 |
| D2~3 | Bernett 등 (2024), [생물학적 ML의 데이터 누출 점검](https://www.nature.com/articles/s41592-024-02362-y), Nature Methods | 실제 split의 개체·세션·파생 파일 겹침 검사 |
| 1~2주 | Lee 등 (2024), [홈펜 행동과 이유 스트레스 회복](https://www.frontiersin.org/journals/animal-science/articles/10.3389/fanim.2024.1461526/full), Frontiers in Animal Science | ethogram, 사건 횟수와 지속시간, 종단 관찰을 후속 SOP에 연결 |
| 2~3주 | Luttman 등 (2025), [Yorkshire의 코르티솔 기반 분류·성장·행동](https://pmc.ncbi.nlm.nih.gov/articles/PMC12578500/), Journal of Animal Science | 다른 품종·상황에 같은 관계가 유지된다고 가정하지 않기 |
| 2~3주 | Nääs 등 (2026), [Animals 16(8):1148](https://pmc.ncbi.nlm.nih.gov/articles/PMC13113075/) | 돼지 음향 연구에 이미 그룹 평가가 있음. 그룹 CV 자체를 새 기여로 주장하지 않기 |
| 3~4주 | Pann 등 (2026), [CompAg 240:111181](https://www.sciencedirect.com/science/article/pii/S0168169925012876) | 상업 농장 간 평가와 SoundWel 팀 이동의 차이. 과제·라벨이 다른 점수를 직접 순위화하지 않기 |
| 3~4주 | Gorssen 등 (2026), [bioRxiv 프리프린트](https://www.biorxiv.org/content/10.64898/2026.07.06.736900v1) | 맥락 라벨을 정서 정답으로 사용하는 해석 문제 점검. 심사 완료 논문처럼 인용하지 않기; 공개 초록 기반 확인 |
| 4주 이후 선택 | Kong 등, [PANNs 원저](https://arxiv.org/abs/1912.10211), [저자 코드](https://github.com/qiuqiangkong/audioset_tagging_cnn) | 이미 사용 가능한 동결 표현과 단순 기준 비교; 같은 fold로 평가 |
| 5주 선택 | [Perch 2 연구](https://arxiv.org/abs/2508.04665), [공식 코드](https://github.com/google-research/perch-hoplite) | 전이 표현 후보 1개. 짧은 콜 padding·duration 지름길, 실행 환경을 먼저 확인. 필수 일정 아님 |
| 현장 준비 | Souza 등 (2025), [aSwine 연구](https://link.springer.com/article/10.1007/s10489-025-06555-6), [저자 자료](https://github.com/andremsouza/aswine) | 연속 축사음의 검출·배경 문제를 별도 과제로 연습. SoundWel 정서 라벨의 외부 검증으로 바꾸지 않기 |
| 현장 준비 | Heseker 등 (2024), [ScreamDetect](https://www.sciencedirect.com/science/article/pii/S2772375524001874) | 탐지 누락·오경보·사람 확인 비용을 함께 보고 |
| 현장 준비 | Statham 등 (2020), [영상 놀람·동결 자동 측정](https://www.nature.com/articles/s41598-020-65954-6) | 영상으로 놀람을 자동화한다는 사실만으로 새 기여라 주장하지 않기; 시간 해상도 확인 |

**최근 경향을 계획에 반영한 판단.** 문헌을 종합하면 이 연구에서는 새 네트워크를 늘리는 것보다 라벨 의미·그룹 독립성·환경 이동·현장 측정의 연결을 먼저 해결할 가치가 크다. 이는 위 논문들의 결과를 바탕으로 한 연구 설계 판단이며, 특정 모델이나 원고의 성공 예측이 아니다. 생리적 상태·행동 맥락·발성 종류·감정은 서로 다른 정답이다.

Briefer 논문의 2023 정정은 Data availability에 Zenodo 음원 주소를 추가한 것이다. 이를 결과나 분류 정확도를 수정한 정정으로 설명하지 않는다.

논문마다 1쪽만 남긴다: 질문, 실제 관측, 독립 단위, 라벨 출처, 평가 분할, 직접 지지된 주장, 우리 계획을 바꿀 점. 공개 자료로 재실험하려면 별도로 다운로드 가능 여부·라이선스·그룹 식별자·음원 중복을 확인한다. 원문 공개와 원자료 공개를 혼동하지 않는다.

**일반 통계·학습 자료:** [ISLP 공식 자료](https://www.statlearning.com/), [Modern Statistics for Modern Biology](https://www.huber.embl.de/msmb/index.html). 첫 2주는 회귀·재표집·실험 단위·반복 구조 등 당장 사용하는 범위를 골라 읽는다. 전권 완독은 분석 시작 조건이 아니다.
