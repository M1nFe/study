**초보자 ML 과정의 공식 자료**

[시작 화면](README.md) · [주차별 범위](curriculum.md)

확인일: 2026-10-01. 중심은 **CS50P로 Python 기초 → scikit-learn MOOC의 선택 실습**이다. MLCC 한국어는 개념을 보충하고, NumPy·pandas 문서는 필요한 기능만 찾아본다. 아래 자료를 모두 완강하는 계획이 아니다.

| 자료 | 역할·사용 범위 | 시작 조건 |
|---|---|---|
| [Harvard CS50P](https://cs50.harvard.edu/python/) | 1~3주 Python 중심. Functions/Variables, Conditionals, Loops, Exceptions, Libraries, File I/O의 기초 부분만 선택. | 프로그래밍 무경험자도 대상. 공개 강의·자료로 시작 가능. 원 과정은 10주이므로 전체를 3주로 압축하지 않음. |
| [Python 공식 한국어 자습서](https://docs.python.org/ko/3/tutorial/) | 문법 참고. 기초, 흐름 제어, 자료구조, 모듈, 파일, 예외를 필요할 때 조회. | 일반 프로그래밍 기초를 아는 독자를 가정하므로 처음부터 혼자 완독할 주교재로 두지 않음. |
| [NumPy beginner guide](https://numpy.org/doc/stable/user/absolute_beginners.html) | 4주 배열 생성·shape·인덱싱·집계·axis. | 리스트·함수·인덱싱을 먼저 익힘. 고급 broadcasting은 나중에. |
| [pandas introductory tutorials](https://pandas.pydata.org/docs/getting_started/intro_tutorials/index.html) | 4~5주 표 구조·읽기·선택·그림·새 열·요약 통계. | Python 기초 다음. 전체 API 목록을 외우지 않음. |
| [Google MLCC 한국어](https://developers.google.com/machine-learning/crash-course?hl=ko) | 6~10주 회귀·분류·일반화·과적합 등 해당 개념과 짧은 연습. | [선수 요건](https://developers.google.com/machine-learning/crash-course/prereqs-and-prework?hl=ko)의 프로그래밍·기초 수학을 먼저 점검. |
| [Inria/scikit-learn MOOC](https://inria.github.io/scikit-learn-mooc/) | ML 실습 중심. 예측 파이프라인, 평가, 선형모델, 트리, 랜덤포레스트의 지정 부분. | Python 변수·함수·import 필요. NumPy·pandas의 기초를 먼저 배움. |
| [An Introduction to Statistical Learning with Applications in Python](https://www.statlearning.com/) | 선택 교재. 이해가 막힌 주에 회귀·분류·재표집·규제·트리·비지도학습의 관련 설명과 lab만 읽음. | 2023 Python판을 기준으로 선택. 공식 사이트의 자료를 이용하며 별도 전권 완독을 추가하지 않음. |

무료 공개 강의·문서로 시작할 수 있다. 제출 채점·계정·수료증은 공개 자료 열람과 다른 서비스이며 이 계획의 필수 조건으로 두지 않는다. 영어 문단은 번역해 이해해도 되지만 변수명·코드·오류 메시지는 원문과 함께 남긴다. 동영상 전체를 먼저 본 뒤 코딩을 시작하기보다 짧게 보고 즉시 실행한다.

**모델을 배운 뒤 찾아볼 문서.**

- [scikit-learn Getting Started](https://scikit-learn.org/stable/getting_started.html): 학습·예측·교차검증의 개념을 배운 뒤 API 흐름 복습.
- [Common pitfalls](https://scikit-learn.org/stable/common_pitfalls.html): 전처리와 데이터 누수 점검.
- [Iris 데이터 설명](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_iris.html): 8~13주 교육용 분류 예제의 출처·변수 확인.
- [Wine 데이터 설명](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_wine.html): 14주부터 사용하는 최종 교육 프로젝트 자료.
- [Colab 공식 안내](https://research.google.com/colaboratory/faq.html): 브라우저 노트북의 저장·실행 환경 확인.

12주의 K-means·PCA는 ISLP의 비지도학습 설명을 선택해서 읽거나 공식 사용자 가이드에서 찾아본다. scikit-learn MOOC의 추가 영역이 완성된 강좌라고 전제하지 않는다. 해당 주는 앞선 기초의 보충에 사용해도 된다.

기출 해답처럼 실습 해답을 먼저 베끼지 않는다. 첫 시도→도움→수정→다른 조건에서 재풀이를 기록한다. 교재·강의·자료 전체를 이 저장소에 복제하지 않는다.
