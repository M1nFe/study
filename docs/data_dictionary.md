# 데이터 사전과 기록 규칙

2026년 10월 15일. [전체 수집 계획](data_collection_master_plan.md) · [빈 CSV 양식](../templates/data_collection/README.md)

양식은 서로 연결할 수 있는 최소 기록 구조다. 현장 입력은 기존 엑셀을 사용할 수 있고 연구자가 회수 후 이 구조로 정리한다. 파일 정보 자동 추출·데이터 검증·분석 실행기는 별도 구현 작업이며, 빈 양식 제공을 구현 완료로 간주하지 않는다.

## 공통 규칙과 저장 위치

- 실제 자료는 `.local/` 또는 연구실 저장소, 공개 Git에는 계획·빈 양식만 둔다. 개인 컴퓨터 절대 경로와 농장 식별정보를 CSV 공개본에 넣지 않는다.
- 원자료는 수정하지 않는다. `relative_path`는 자료 저장 루트 기준 상대 경로이고 `sha256`은 원본 바이트의 64자리 해시다. 변환 파일은 별도 이름·부모 원본 ID·설정으로 추적한다.
- 실제 시각은 ISO 8601의 `YYYY-MM-DDTHH:MM:SS+09:00`처럼 시간대까지 남긴다. `device_*`는 보정 전 기기 시각이며 실제 시각이라고 가정하지 않는다. D0/R/E는 실제 확인된 날짜다.
- 시간 구간은 시작 포함·끝 제외다. 초는 `_s`, kg는 `_kg`, 미터는 `_m`, 온도는 섭씨, 습도는 0~100%다. 비율과 %를 혼용하지 않는다.
- 숫자 결측은 빈칸과 대응 사유 열에 기록한다. 범주 미상은 `unknown`, 예/아니오는 `true/false`를 사용한다. 녹음·시야·측정이 유효하고 사건이 없을 때만 0을 쓴다.
- ID는 익명 내부키다. 표에 없는 동물/세션/결과를 생성해 실제 측정처럼 채우지 않는다. 원 저자 ID는 앞자리 0을 보존하는 문자열로 가져온다.
- 한 열의 여러 ID가 필요한 `neighbor_feeding_ids`는 세미콜론 구분을 사용한다. 나머지 연결은 외래키나 시간 교집합으로 계산한다.

권장 비공개 구조는 다음과 같다. 물리적 저장 루트는 실제 하드 경로를 확인한 뒤 정한다.

```text
research_data/
  soundwel/raw/                 # 기존 ZIP, Key, S1 정본
  soundwel/manifests/           # 코호트, alias/블록 근거, 고정 분할
  soundwel/runs/                # 개발, 본 분석, 민감도 구분
  farm/raw/YYYY-MM-DD/CAM01/card_A/  # 원 파일명 유지
  farm/registers/              # 장치, 세션, 급이, 관리, 생산 자료
  farm/manifests/              # 파일 해시, 회수, 동기, 보존 상태
  farm/annotations/            # 관찰창, 행동, 사건, 판독 버전
  derived/                     # 재생성 가능한 분석 표와 작업 파일
  manuscript/                  # 표, 그림, 버전별 원고
```

## SoundWel 자료

| 양식 | 키와 중요 열 | 정의와 검사 |
|---|---|---|
| sources | `source_id`; dataset/record/release, URL, license/redistribution, bytes/checksum, reported/inventory count | 릴리스 설명 수와 실제 인벤토리 수를 모두 보존. S1과 공개 음원은 별도 출처. 원저자 자료의 재배포 가능 여부는 추정하지 않음 |
| soundwel_samples | `segment_id`; source/path/hash, read/포맷, S1 행/조인, raw ID/context/valence/date, alias·원 녹음·split block, 네 특징 | 한 클립 한 행. `s1_row_zero_based`는 0부터 시작. 조인 충돌/미매칭/결측/제외 사유 명시. `duration_s`는 파일 길이, `Dur`는 분석 특징으로 서로 대조하되 덮어쓰지 않음 |
| soundwel_splits | `(seed, fold, role, arm, segment_id)`; pair/litter/source family, feature_sets, protocol/cohort/연결 블록 | 기존 manifest 열과 의미 유지, 확장 열만 추가. `role=train/test`, K/U 학습은 각각 기록하고 공통 test는 기존 manifest 방식을 유지. 파일 hash는 자기 열에 넣지 않고 실행 대장에 보관 |
| analysis_runs | `run_id`; study/run_kind, 코드·환경·입력·분할·설정 hash, seed/fold/arm/feature_set, 시각/상태 | `run_kind=development/primary/sensitivity/audit`. 별도 실행을 덮어쓰지 않음. 예정은 결과가 아님 |
| soundwel_predictions | `(run_id, segment_id)`; fold/arm/features/block/context, true_label/p_positive/predicted_label | 양성확률 0~1. 참 라벨은 맥락 기반 라벨이며 감정 측정값 아님. 동일 seed의 각 조건 OOF에서 평가 클립이 한 번씩 등장 |

`Dur/AMRate/Q50/WienEntrMean`은 기존 특징의 정의·단위와 버전을 함께 유지한다. Dur·AMRate·Q50>0, entropy 유한성 검사는 현재 공통 표본 규칙이다. 임의로 0을 만들거나 결측 ID를 대치하지 않는다. `placeholder_date=true`인 날짜로 세션을 만들지 않는다. `identity_status`는 확인/모호/발신자 불명 등 근거를 담으며 문자열 ID 수를 개체 수로 바꾸지 않는다.

K/U/M1/M4 공통 평가, K/U 클래스별 학습 콜·블록 수, 학습/평가 연결 블록 비중첩을 성능 실행 전에 확인한다. 최종 규칙은 [맥락 전이 설계](context_transfer_design.md)를 따른다.

## 농장 등록과 현장 기록

| 양식 | 한 행과 키 | 중요 열과 기록 규칙 |
|---|---|---|
| farm_devices | 한 설정 버전, `device_setting_id` | camera/pen/scene, HW/FW, 설치 시각·높이·위치·ROI·사진, 영상/음성/야간/녹화/암호화 설정. 변경 때 새 버전. 마이크 수치를 확인 못했으면 빈칸 |
| farm_sessions | 한 돈방·24시간 세션, `session_id` | 공통 scene과 pen, 실제 treatment, 치수/재실 수, 단계·D0/R/E, 예정/실제 구간, 선택 근거, pilot/main, 급이 회차. 동일 날짜 두 돈방은 서로 다른 session ID |
| farm_feeding | 한 돈방의 한 급이 회차, `feeding_id` | 작업 시작·실제 사료 접근 F·급이 종료, 시각 근거·오차, 사료 배치·단계·투입·잔량·유실, 측정 여부, 장면 내 순서. 양쪽 돈방의 모든 급이 회차 기록 |
| farm_management | 한 사건, `management_id` | 장면/돈방, 시작/끝, `event_type`, 내용·변경 후 두수·발견/복구, 기록자. 유형은 cleaning/ventilation/lighting/noise/power/animal_move/treatment/other |
| farm_card_transfers | 한 카메라 카드 교체, `transfer_id` | 나가는/들어가는 카드, 정지/재개/녹화·시계 확인, 덮개 재밀폐·청소, 다음 방문, 복사 배치·두 사본 검증·재사용 가능 여부 |
| farm_production | 측정 변수 하나, `production_record_id` | 자료 출처, 돈방/제공된 개체 ID, 측정 단위, 시각/간격, variable/value/unit, 방법·사료 배치·재실 수·결측 사유 |

`scene_id`는 인접 돈방이 소리를 공유하는 관측 장면이다. `pen_id`는 물리적 돈방이며 treatment와 별개 키다. 두 돈방이 CON/TRT 각각 하나라는 사실은 독립 처리 반복이 각 1이라는 뜻이다. 돈방 안의 동물 수·카메라 수·시간창 수로 이를 늘리지 않는다.

사료 조성은 기존 시험에서 받은 값만 기록한다. 예를 들어 SID lysine 또는 에너지 농도를 받으면 `variable`, `value`, `unit`, `feed_batch_id`, `source_id`로 기록한다. 문헌의 사료 조성을 실제 급여 사료값으로 쓰지 않는다. `measurement_unit=animal/pen/feed_batch`를 구분하고 돈방 공급량을 개체 섭취량으로 변환하지 않는다. 실제 개체별 성장 비교가 필요하면 기존 동물 등록·이동 대장 원본을 별도 보존하여 연결한다.

온습도 칸은 기존 농장 계측의 시각·장치·단위를 함께 받을 때만 채운다. 현재 기본 견적에는 환경 로거가 없으므로 비어 있다고 임의 보간하지 않는다. 외부 날씨는 돈방 내부 실측을 대체하지 않는다.

## 원본 파일과 시계

`farm_files`의 키는 `file_id`다. 원본 파일마다 카메라·카드·설정 버전·원 경로·bytes·SHA256, 기기 시작/끝·파일 길이, 실제 영상/음성 포맷, 읽기 상태·재생 점검 범위, 동기 모델, 두 사본 검증 시각, 보존 여부를 기록한다. `session_id`는 파일이 단일 세션에 속할 때 기록하며 여러 세션 경계에 걸치면 빈칸으로 두고 카메라·보정 시각의 교집합으로 연결한다. 같은 파일을 두 개의 독립 원본 행으로 만들지 않는다.

`video_sync_id/audio_sync_id`는 영상과 음성의 보정 모델을 각각 참조한다. `copy_A_verified_at/copy_B_verified_at`는 이름이 같은 파일의 존재만 보고 채우지 않는다. 실제 바이트·해시 일치가 확인된 시각이다. `retain_status=selected/temporary/extra_exploratory`를 사용하며 임시 원본 정리 여부와 근거는 변경/관리 대장에 남긴다.

`farm_sync`는 `sync_id`인 보정 모델과 `anchor_id`인 기준점을 함께 가진다. 복합키 `(sync_id, anchor_id)`로 기록한다. `stream=video/audio`를 구분하여 동일 파일의 음성/영상 차이도 검사한다. 재부팅·시각 변경마다 `clock_epoch_id`를 새로 만든다. 같은 모델의 각 기준점에는 동일 보정 계수를 기록한다. `anchor_role=fit/validation`으로 보정에 사용한 점과 사용하지 않은 독립 검증점을 구분한다.

```text
보정 시각 = 기기 시각 + offset_s
           + drift_s_per_hour × (기기 시각 − origin_device_time의 초 차이) / 3600
```

`offset_s`는 기준 기기 시각에서 기준 시계−기기 시계의 차이다. `valid_from/to_device_time` 밖으로 보정을 외삽하지 않는다. `residual_s`는 보정 후 기준점과의 잔차, `reference_uncertainty_s`는 기준 시각 자체의 오차다. 시작·끝 등의 자연 관리 사건으로 보정하고 적합에 쓰지 않은 중간 사건에서 검증한다. 운영 기준은 독립 검증점마다 `abs(residual_s)+reference_uncertainty_s ≤ 1`이다. 독립 검증점이 없으면 `status=unverified`로 두며 적합점 잔차 0으로 통과시키지 않는다. 하나만 확보되면 드리프트를 추정한 척하지 않는다. 근거가 부족한 구간은 정확한 콜 대응을 보류한다. 동기 확인을 위해 새 놀람 자극을 만들지 않는다.

## 행동과 음향 판독

`farm_windows`는 `window_id`당 한 돈방의 6분 관찰창이다. session/scene/pen/feeding을 연결하고 PRE/EARLY/LATE, 보정 구간, ROI 버전, 유효 음성·영상·공통 초, 유효 스캔 수, 이웃 급이 노출·연결 ID, 잡음, 기준 채널, 짝지은 분석 적격성과 제외 사유를 저장한다. `valid_joint_s`는 두 유효 구간의 **실제 교집합**이며 두 유효시간의 작은 값으로 대신하지 않는다.

`farm_valid_intervals`는 `interval_id`당 하나의 유효/무효 구간이다. 창·modality(audio/video)·시각·valid_status(valid/invalid/unknown)·사유·근거 file ID·기준 채널·판독자·버전을 남긴다. `evidence_file_ids`는 세미콜론 구분이다. 같은 창·modality·판독자·판독 버전에서 구간은 전체 360초를 중복 없이 덮어야 하며, 모르는 부분도 unknown으로 남긴다. 음성 유효는 고정 기준 채널에서 판단하고 영상 유효는 사전 고정 ROI와 두 화면의 시야 보완 규칙으로 판단한다. 최종 버전의 valid 구간만으로 각각의 유효초, 교집합, 사건 시간의 분모를 계산한다.

`farm_behavior_scans`의 키는 `(window_id, scan_index, rater_id, annotation_version)`이다. `scan_index=0..5`이며 실제 스캔 시각도 기록한다. 두 카메라 화면을 대조해 중복 돼지를 제거하고 무음 영상으로 판독한다.

| 열 | 정의 |
|---|---|
| n_present | 해당 돈방의 실제 재실 수. 30두로 고정하지 않고 이동·폐사 등 변경 반영 |
| n_visible | 정해진 관찰영역에서 다른 돼지와 구분해 한 번씩 센 가시 고유 두수. 안 보이는 돼지는 이 분모 밖 |
| n_lying | 몸통이 바닥에 닿은 누움 자세 |
| n_sitting | 뒷부분은 바닥, 앞다리는 세운 앉음 자세 |
| n_standing | 네 다리로 지지하고 앞뒤 각 2초 확인에서 위치 이동이 없는 서기 |
| n_moving | 네 다리로 지지하며 같은 확인 구간에서 보행으로 몸 위치가 이동함. 머리 움직임만으로 이동 처리하지 않음 |
| n_posture_unknown | 가시 개체지만 가림·화질·겹침으로 위 자세 분류가 불가능 |
| n_feeder_occupied | 정해진 급이구 영역에 머리가 들어가 있거나 머무는 가시 돼지 수. 실제 섭취량은 아님 |
| n_feeder_unknown | 가시 개체지만 급이구 점유 여부를 판단할 수 없는 수 |

자세 합계는 `n_visible`과 같고 `n_visible ≤ n_present`여야 한다. 누움 비율 등의 분모는 `n_visible − n_posture_unknown`, 급이기 점유 비율의 분모는 `n_visible − n_feeder_unknown`이다. 분모가 0이면 비율은 결측이다. `n_feeder_occupied ≤ n_visible − n_feeder_unknown`를 확인한다. 전체 가시성 `n_visible/n_present`와 불명 비율도 함께 보고한다. 영상 무효는 숫자 0으로 채우지 않는다.

`farm_acoustic_events`는 `(scene_event_id, rater_id, annotation_version)`당 한 사건이다. 장면 시각, 기준 채널에서 들렸는지, 출처·근거·확신도, 유형·콜 수, 겹침·클리핑·잡음·경계 절단을 기록한다. 유형은 `pig_vocal/other_sound/uncertain`, 판정 상태는 `event_status=confirmed/candidate/unresolved`, 확신도는 `high/medium/low`다. 최종 판독에서 식별 가능한 돼지 발성으로 판정되고 경계가 기록된 사건을 confirmed로 둔다. 이 판정은 실제 발신 개체나 감정을 확정하는 뜻이 아니다. `source_scope=study_pen/outside/unknown`이고 study_pen이면 `source_pen_id`가 등록된 물리적 돈방 ID를 참조한다. 그 외에는 source_pen_id를 비워 둔다. 처리명은 별도 세션 대장에서 연결한다. 한 사건에 콜 수를 분리할 수 없으면 `call_count`를 비워 둔다. `primary_detected=false`인 다른 채널 전용 사건은 보조 자료로 남기고 기준 채널 주계수에 추가하지 않는다.

사건 경계는 같은 소리로 식별되는 구간의 시작/끝으로 정한다. 복수 콜이 겹쳐 분리하기 어려우면 복합 사건으로 보존하고 임의의 한 돼지 콜로 만들지 않는다. `farm_event_detections`는 사건의 각 파일/채널 구간을 기록한다. `detection_id`는 고유키이고 `file_id` 및 `(scene_event_id, rater_id, annotation_version)`을 각각 참조한다. `onset/offset_file_s`는 그 원본 파일 내 초다. `match_status=confirmed/uncertain`와 근거를 남긴다. uncertain 검출은 별개의 확정 콜로 합산하지 않는다.

기준 채널 주지표는 최종 판독 버전의 `event_type=pig_vocal`, `event_status=confirmed`, `primary_detected=true`를 모두 충족한 사건을 사용한다. 사건 수는 **시작점이 유효 관찰창 안에 있는 사건**으로 센다. 창 시작 이전부터 계속된 사건은 새 사건으로 세지 않지만 발성 시간에는 창 안의 교집합을 포함한다. 발성 시간은 해당 구간의 합집합과 유효 구간의 교집합으로 계산하여 중첩 시간을 이중 합산하지 않는다. 음질로 판단 불가능한 구간을 유효시간에서 제외하고 그 이유를 남긴다.

독립 판독행을 평균하거나 덮어쓰기 전에 판독자 차이를 보고한다. 사건 경계·일치 허용오차는 파일럿에서 실제 시간 해상도에 맞춰 정하고 프로토콜에 남긴다. 해결된 최종 주석은 별도 판독 버전으로 만든다. 관측된 녹음에 대한 판독 일치도는 모든 실제 돼지 발성의 검출 민감도가 아니다.

## 실행 전후의 필수 검사

1. 모든 키가 지정 단위에서 유일하고 참조하는 ID가 존재하는지 확인한다. 실제 데이터를 넣기 전 빈 양식에는 적용할 결과가 없다.
2. 예정/실제 시간·기기/보정 시간·파일럿/본관찰·개발/본분석을 혼합하지 않는다.
3. 60창·360스캔은 예정량이다. 실제 수는 계획/확보/유효/제외/미수집과 사유를 분리해 보고한다.
4. 중복 보존 날짜, 유효시간이 계획시간을 초과하는 행, 음수 길이, 자세 합계 불일치, 잘못된 분모를 확인한다.
5. 공통 장면의 같은 소리를 여러 돈방·카메라에서 독립 자료로 세지 않는다. 파일/시계 경계를 넘는 사건도 중복 확인한다.
6. `protocol_changes`에 변경 시점·이유·전후 값·이미 본 결과·영향받는 ID를 기록한다. 원자료나 과거 계획을 소급해서 바꾸지 않는다.

이 검사는 파일·기록의 일관성을 위한 것이다. 논문에 필요한 독립 표본 수나 측정 타당성을 자동 보장하지 않는다.
