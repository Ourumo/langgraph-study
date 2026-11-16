# 1주차 (11/16 ~ 11/22)

**환경 세팅, 첫 그래프, 조건 분기**

> **1주차 완료 기준**
>
> 여러 번 실행했을 때 "바로 통과", "한 번 실패 후 통과", "재시도 초과로 종료" 세 경우가 모두 나온다

## 11/16 · 환경 세팅

### 📋 할 일

**저장소 준비**

- [x] GitHub에 `langgraph-study` 저장소 생성, 로컬과 원격(`origin`) 연결
- [x] README.md 작성 (목표, 아키텍처, 설계 원칙, 핵심 스키마, 로드맵)
- [x] `.gitignore` 추가 (`.venv/`, `.env`, SQLite 체크포인터 파일, 개인 학습 계획 문서 제외)
- [x] 주차별 실습 폴더(`week01` ~ `week08`)와 `notes` 폴더 생성
- [x] 주차별 노트 파일(`notes/week01.md`) 생성

**Python 환경**

- [x] Python 3.12 설치 확인 (`python3 --version` → 3.12.3)
  - `langchain` 1.5.0부터 3.10 지원이 끊겨서 3.12 또는 3.13으로 고정
- [x] uv 설치 (`brew install uv`), `uv --version`으로 확인
- [x] 프로젝트 초기화 (`uv init --python 3.12`)
  - `pyproject.toml`, `.python-version`이 생기는지 확인
  - 자동 생성되는 샘플 파일(`main.py` 또는 `hello.py`)은 삭제
  - 기존 README.md가 그대로인지 확인
- [x] LangGraph 설치와 버전 고정 (`uv add "langgraph==1.2.*"`)
  - `pyproject.toml`의 `dependencies`에 `langgraph==1.2.*`가 들어갔는지 확인
  - `.venv/`와 `uv.lock`이 생기는지 확인
- [x] 설치 확인 (`uv run python -c "from importlib.metadata import version; print(version('langgraph'))"`)
  - `1.2.x`가 출력되면 통과
- [ ] 환경 파일 커밋과 push (`pyproject.toml`, `uv.lock`, `.python-version`, `notes/week01.md`)
  - `.venv/`가 커밋에 섞이지 않았는지 `git status`로 확인

**완료 기준**

- [x] `uv run`으로 langgraph `1.2.x` 버전이 출력된다
- [ ] 환경 파일이 GitHub에 올라가 있다

### 📝 핵심 메모

---

## ⚠️ 예정된 작업

이번 주에 남은 작업. 회차를 시작할 때 해당 묶음을 위 회차 섹션으로 옮겨서 사용한다.

**개념 학습**

- [ ] [LangChain Academy](https://academy.langchain.com/) 가입
- [ ] [Quickstart: LangGraph Essentials - Python](https://academy.langchain.com/collections/quickstart) 수강 (State, Node, Edge까지)
  - 예제가 API 키를 요구하면 가짜 함수로 바꿔서 실행
  - Memory 부분은 3주차 체크포인트에서 다시 다루므로 훑어보기만
- [ ] 회차의 핵심 메모에 5줄로 정리 (State, Node, Edge가 각각 무엇이고 어떻게 연결되는지)
- [ ] 완료 기준: State, Node, Edge를 각각 한 줄로 설명할 수 있다

**첫 그래프 (a → b → c 직선 그래프)**

- [ ] 코드를 짜기 전에 노드와 엣지를 Mermaid나 종이에 먼저 그리기
- [ ] 실습 파일 만들기 (예: `week01/linear_graph.py`)
- [ ] 상태 정의: `log: list[str]` 필드 하나를 가진 `TypedDict`
- [ ] 노드 3개(a, b, c) 작성
  - 각 노드는 상태 전체가 아니라 변경분(`{"log": ["a"]}`)만 반환
- [ ] `StateGraph`로 조립하고 `compile()`
  - `START` → a → b → c → `END` 순서로 `add_node`, `add_edge`
- [ ] `invoke(..., version="v2")`로 실행하고 결과의 `.value` 출력
- [ ] 리듀서 비교 실험
  - `log: list[str]`(리듀서 없음)로 실행한 결과 기록
  - `log: Annotated[list[str], operator.add]`로 바꿔 실행한 결과 기록
  - 실행 전에 결과를 먼저 예상해 보고, 실제와 왜 다른지 핵심 메모에 정리
- [ ] 커밋
- [ ] 완료 기준: 리듀서가 있을 때와 없을 때 `log`가 어떻게 달라지는지 설명할 수 있다

**조건 분기 (가짜 작업자 4개)**

- [ ] 흐름을 먼저 그리기: 기획 → 개발 → QA → 위키, QA 실패 시 개발로 되돌림
- [ ] 실습 파일 만들기 (예: `week01/workers_graph.py`)
- [ ] 상태 설계: `log`에 더해 QA 결과를 담을 필드 정하기
- [ ] 가짜 작업자 4개(기획, 개발, QA, 위키) 작성
  - LLM 없이 `log`에 자기 이름을 남기는 함수로 충분
- [ ] QA 노드가 `random`으로 통과 또는 실패를 정해 상태에 기록
- [ ] 라우팅 함수 작성: 상태의 QA 결과를 보고 다음 노드 이름을 반환
- [ ] `add_conditional_edges`로 QA 뒤에 분기 연결 (통과 → 위키, 실패 → 개발)
- [ ] 여러 번 실행해서 `log`로 두 경로가 모두 나오는지 확인
- [ ] 커밋
- [ ] 완료 기준: "바로 통과"와 "실패 후 개발로 되돌아감" 두 경로가 모두 나온다

**보강 (재시도 상한, 그래프 그림, 회고)**

- [ ] 상태에 `retries` 추가
  - 어느 노드에서 증가시킬지 먼저 정하기
- [ ] 라우팅 함수에 상한 추가: 재시도가 2회를 넘으면 `END`로 분기
- [ ] 여러 번 반복 실행해서 세 경우가 모두 나오는지 확인
  - 재현이 필요하면 `random.seed()`로 고정
- [ ] `draw_mermaid()`로 그래프 그림을 뽑아 README에 넣기
  - 손으로 그린 흐름과 같은지 비교
- [ ] 아래 주간 회고 3줄 작성
- [ ] README 로드맵 1주차 체크
- [ ] 커밋과 push

## 📆 주간 회고

### ✅ 된 것 (Done)

### 🚧 막힌 것 (Blocked)

### 💡 배운 것 (Learned)
