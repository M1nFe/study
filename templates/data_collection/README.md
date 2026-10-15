# 데이터 수집 빈 양식

[전체 계획](../../docs/data_collection_master_plan.md) · [데이터 사전](../../docs/data_dictionary.md)

이 폴더의 CSV는 헤더만 있는 빈 양식이다. **실제 작성본은 이 폴더에 저장하지 않고** Git에서 제외된 `.local/` 또는 연구실 저장소에 복사한다. 실제 원자료·ID·경로·결과를 공개 저장소에 올리지 않는다.

## 지금 작성할 순서

1. 연구자가 `sources.csv`와 장치/세션 등록표를 준비한다. 날짜·급이 시각·펌웨어·측정값을 추정해서 채우지 않는다.
2. 현장에서는 `farm_feeding.csv`와 `farm_management.csv`에 급이·관리 기록을 남긴다. 방문 연구자는 `farm_card_transfers.csv`로 카드 교체와 재개를 확인한다.
3. 회수 후 `farm_files.csv`와 `farm_sync.csv`를 작성한다. 바이트·해시·코덱 같은 값은 파일에서 추출하며, 현재 양식이 자동 추출 기능을 제공하는 것은 아니다.
4. 사전 선정한 창을 `farm_windows.csv`에 고정하고 행동 스캔과 음향 사건을 판독한다. 서로 다른 판독자의 행은 보존한다.
5. SoundWel은 기존 실자료 manifest를 정본으로 사용한다. 이 빈 양식으로 기존 파일을 덮어쓰지 않는다. 실행 시 run과 예측 테이블을 생성한다.

## 기존 엑셀과 연결

현재 사용 중인 **농장 C320WS SD저장 200만원 견적과수집계획** 엑셀을 현장 입력창으로 계속 사용할 수 있다. 같은 값을 엑셀과 CSV에 두 번 수기로 입력하지 않는다. 회수 후 엑셀 사본에서 내보내어 데이터 사전의 단위·키에 맞춰 변환한다. 아래는 대응표이며 자동 동기화 기능은 아니다.

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
