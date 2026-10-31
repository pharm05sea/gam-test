# 건강 테스트 — Claude 작업 메모

공개 데이터로 만든 건강 숫자 추측 게임이다. 주제당 5문항이고, 모든 정답은 `data/raw/`의 원본에서 `tools/build_topics.py`가 계산한다.
답은 한국어로 한다.

## 구조
- `tools/fetch_all.py`: 원본을 `data/raw/`로 받는다 (`SOURCES` 목록)
- `tools/build_topics.py`: 원본을 읽어 `topics/<id>.json`, `topics.json`, `tools/정답지.md`를 만든다
  - `SHOW` 목록에 든 주제만 내보낸다. 목록 순서가 화면 순서다
  - 지금 보이는 주제: `mild-cognitive-impairment`, `life`, `dementia-awareness`
  - 목록에서 뺀 예전 주제 10개는 계산 코드만 남아 있다
- `index.html` · `app.js` · `style.css`: 화면. `?t=<id>`로 주제를 바로 연다
- 배포: GitHub `pharm05sea/gam-test` → Vercel `https://gam-test-rho.vercel.app` (push하면 자동 배포)

## 주제를 더할 때
1. `build_topics.py`에 `topic("새-id", …)` 블록을 만들고 `SHOW`에 id를 더한다
2. `python tools/build_topics.py --only 새-id`로 그 주제만 빌드한다
3. 다른 주제 파일이 바뀌지 않았는지 확인한다

## 지킬 것 (사용자가 정한 규칙)
- 정답 숫자를 코드에 직접 쓰지 않는다. 원본 파일에서 읽거나 계산한다
- 세는 방법·분모·결측·가중치·분류 기준이 애매하면 정하지 말고 사용자에게 묻는다
- 다른 주제는 다시 계산하지 않는다 (`--only` 사용). 기존 `topics/` 파일은 고치지 않는다
- 원자료에 없는 응답이나 수치를 만들지 않는다. 기사 숫자로 대신하지 않는다
- basis는 기간·대상·센 방법을 한 문장으로, caveat(한계)도 한 문장으로 쓴다
- `data/`와 `tools/정답지.md`는 공개하지 않는다 (`.gitignore`). 커밋 목록에 `data/raw/`가 보이면 멈추고 알린다
- 커밋은 사용자가 요청할 때만 한다. 커밋 설명은 무엇을 왜 바꿨는지 한 줄로 쓴다
- 사용자가 "고치지 마", "지적만 해"라고 하면 파일을 건드리지 않는다

## 예외
- `dementia-awareness`(2025년 치매 인식도 조사)는 원자료가 공개되지 않아, 사용자가 허락한 대로 결과보고서 공표값을 `data/raw/dementia_awareness_2025.csv`에 옮겨 적고 그 파일을 읽는다. basis와 caveat에 "보고서 공표값"이라고 밝힌다

## 로컬 환경
- 로컬 서버: `python -m http.server 8002` → http://localhost:8002/ (8000도 사용)
- 포트 8001은 사용자의 포트폴리오 서버이므로 건드리지 않는다
- 브라우저는 `Start-Process "<주소>"`로 연다. Edge를 직접 실행하지 않는다
- 윈도우 콘솔은 한글이 깨지므로 긴 한글 출력은 UTF-8 파일로 쓰고 읽는다
