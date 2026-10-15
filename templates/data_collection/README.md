# 데이터 수집 빈 양식

[전체 계획](../../docs/data_collection_master_plan.md) · [데이터 사전](../../docs/data_dictionary.md)

이 폴더의 CSV는 헤더만 있는 빈 양식이다. **실제 작성본은 이 폴더에 저장하지 않고** Git에서 제외된 `.local/` 또는 연구실 저장소에 복사한다. 실제 원자료·ID·경로·결과를 공개 저장소에 올리지 않는다.

2026년 10월 16일 확정한 농장 구성은 **ATTD 제외·Tapo C320WS 4대(보유 2대＋추가 2대)·내장 마이크·현장 SD 저장**이다. CON/TRT 2돈방에 카메라 2대씩 배치하고 2~3일마다, 최대 72시간 이내에 카드를 회수한다. 외부 녹음기·NVR·상시 현장 PC를 전제로 입력하지 않는다. 인접 돈방은 약 1.2m 가벽으로 나뉘므로 실제 음원 출처를 모르면 `unknown`으로 남긴다.

## 지금 작성할 순서

1. 연구자가 `sources.csv`와 장치/세션 등록표를 준비한다. 날짜·급이 시각·펌웨어·측정값을 추정해서 채우지 않는다.
2. 현장에서는 `farm_feeding.csv`와 `farm_management.csv`에 양쪽 돈방의 실제 급이 시각·사료 투입/잔량/유실·관리·재실 수 변동을 남긴다. D0/R/E 체중(가능하면 주 1회), 실제 사료 배치의 배합표·조성 자료와 기간 집계는 `farm_production.csv`에 기록한다. 방문 연구자는 `farm_card_transfers.csv`로 카드 교체와 재개를 확인한다.
3. 회수 후 `farm_files.csv`와 `farm_sync.csv`를 작성한다. 바이트·해시·코덱 같은 값은 파일에서 추출하며, 현재 양식이 자동 추출 기능을 제공하는 것은 아니다.
4. 사전 선정한 창을 `farm_windows.csv`에 고정하고 행동 스캔과 음향 사건을 판독한다. 서로 다른 판독자의 행은 보존한다.
5. SoundWel은 기존 실자료 manifest를 정본으로 사용한다. 이 빈 양식으로 기존 파일을 덮어쓰지 않는다. 실행 시 run과 예측 테이블을 생성한다.

## 기존 엑셀과 연결

현재 사용 중인 **농장 C320WS SD저장 200만원 견적과수집계획** 엑셀을 현장 입력창으로 계속 사용할 수 있다. 같은 값을 엑셀과 CSV에 두 번 수기로 입력하지 않는다. 회수 후 엑셀 사본에서 내보내어 데이터 사전의 단위·키에 맞춰 변환한다. 아래는 대응표이며 자동 동기화 기능은 아니다.

2026년 10월 16일 만든 **농장_Tapo수집_견적과_ATTD검토** 엑셀은 `견적`·`운영과용량`·`ATTD 판단` 3개 시트의 구매·운영·근거 문서다. 현재 구매 판단에는 이 최신 견적을 사용하고, 현장 관측 입력에는 기존 수집 엑셀 또는 아래 CSV를 사용한다. 새 견적 엑셀을 급이·행동·파일 대장으로 대신 사용하지 않는다. 이전 견적의 외부 녹음 장비 등 대안은 현재 확정 구성에 포함하지 않는다.

| 엑셀 시트 | 대응하는 분석용 표 |
|---|---|
| 현장계획·설치와음질 | farm_devices, farm_sessions, protocol_changes |
| 수집일정 | farm_sessions |
| 급이일지 | farm_feeding, farm_management |
| 관찰창 | farm_windows, farm_valid_intervals |
| 행동스캔 | farm_behavior_scans |
| 음향사건 | farm_acoustic_events, farm_event_detections |
| 파일과동기 | farm_files, farm_card_transfers, farm_sync |
| 장비견적·저장용량 | 운영 계획 자료로 보관, 관측 결과로 변환하지 않음 |

이웃 돈방 급이 노출, 음향 기준 채널, 실제 유효시간, 동기 보정 구간 등 엑셀에 동일한 전용 열이 없는 값은 보조 대장에 기록하고 ID로 연결한다. 새 열을 만들 때 기록 단위를 바꾸지 않는다.

성장·사료 자료는 `farm_production`의 기존 범용 열로 받는다. 사료의 CP·총 라이신·SID lysine·에너지는 확보된 항목만, 배치와 단위·기준·출처 및 **계산값/표시값/분석값**을 구분한다. ATTD 열이나 분변 수집 양식은 추가하지 않는다. 돈방 공급량은 섭취량과 다르며, 잔량·이월·유실과 실제 pig-days가 검증된 경우에만 ADFI를 계산한다. G:F는 같은 기간·집합의 유효 증체량과 섭취량을 확보한 경우에만 보고한다. 세부 변수명과 결측·입출식 처리는 [데이터 사전의 성장·사료 기록](../../docs/data_dictionary.md#attd-없이-확보할-성장사료-기록)을 따른다.

## 파일별 역할

| 양식 | 한 행의 단위 |
|---|---|
| sources | 한 자료 릴리스의 한 원본 파일 |
| soundwel_samples | 한 발성 클립 |
| soundwel_splits | 한 seed/fold/조건에서 한 클립의 역할 |
| analysis_runs | 한 모델 적합 또는 명시한 감사 실행 |
| soundwel_predictions | 한 실행에서 평가 클립 한 개의 예측 |
| farm_devices | 한 카메라의 한 설정 버전 |
| farm_sessions | 한 돈방의 한 24시간 세션 |
| farm_feeding | 한 돈방의 한 급이 회차 |
| farm_management | 한 관리·환경·장치 사건 |
| farm_files | 한 원본 파일 |
| farm_card_transfers | 한 카메라의 한 카드 교체 |
| farm_sync | 한 시계 구간의 한 동기 기준점 |
| farm_windows | 한 돈방의 한 6분 관찰창 |
| farm_valid_intervals | 한 창에서 음성 또는 영상이 유효/무효/불명인 한 구간 |
| farm_behavior_scans | 한 창·시점·판독자·판독 버전 |
| farm_acoustic_events | 한 장면의 한 음향 사건·판독자·버전 |
| farm_event_detections | 사건이 한 파일/채널에서 확인된 한 구간 |
| farm_production | 기존 시험의 측정 변수 하나와 측정 단위 |
| protocol_changes | 하나의 계획 변경 |

측정하지 않은 숫자는 빈칸과 사유로 남긴다. 불명 출처는 `unknown`, 실제 무발성·무행동은 관찰이 유효할 때만 0으로 기록한다. 실제 작성본에는 [데이터 사전](../../docs/data_dictionary.md)의 키·시간·분모 검사를 적용한다.
