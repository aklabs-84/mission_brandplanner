# 작업 로그 (Work Log) - Day 3 Few-shot 실습 재설계

*   **기록 일시**: 2026년 07월 11일
*   **대상 파일**: [day3_integrated_workbook.html](file:///Users/byunmose/Downloads/우송대학교/day3/day3_integrated_workbook.html)

---

## 1. 요청 내용

Day 3의 두 실습(실습1 "기획서 심폐소생 게임" Stage 3, 실습2 "기획서 플랜 빌더"의 Few-shot 예시뱅크)이
실제 few-shot 프롬프팅 개념(여러 예시로 AI에게 일관된 패턴을 학습시켜 새로운 입력에 적용시키는 기법)을
제대로 반영하지 못하고, 단순 "나쁜 문장 → 좋은 문장" 1회성 교정 수준에 머물러 있다는 문제 제기.
두 실습 모두 재설계 요청. 사용자는 두 항목 모두 "라이트(Recommended)" 스코프로 진행을 확정.

## 2. 작업 내용

### 실습1 (Stage 3 게임) — "실전 적용" 보너스 라운드 추가
*   `STAGE3_DEFS` 각 미션에 `bonus` 객체(`testInput`, `minLength`, `requireDigit`, `keywords`, `hint`) 추가.
*   정답을 맞히고 로딩 연출이 끝나면 곧바로 다음 문제로 넘어가던 흐름을,
    같은 패턴을 새로운 문장에 직접 타이핑해서 적용해보는 보너스 라운드로 연결하도록 변경.
*   새 함수 `showBonusRound(q)`, `checkBonusRound()`, `advanceStage3()` 추가.
*   `#stage3-bonus` UI 블록(테스트 문장, 입력창, 힌트, 채점 버튼, 피드백) 신설.
*   보너스 라운드 오답 시 인내심 -10 (기존 객관식 오답 페널티 -20과 구분).
*   `resetGame()`에 `#stage3-bonus` 초기화 로직 추가.

### 실습2 (플랜 빌더 Few-shot 예시뱅크) — 고정 모범 예시 + 실전 테스트 추가
*   3개 카테고리(제품/타겟/성과) 각각에 항상 보이는 "모범 예시(고정)" 참조 박스 추가.
*   기존 입력창 라벨을 "내 예시: 나쁜/좋은 문장"으로 명확화.
*   `#fs-test-input`(테스트 입력) / `#fs-ai-output`(AI 응답 붙여넣기) 블록 신설 → 최종 프롬프트에
    모범 예시 + 내 예시 + 테스트 입력이 하나의 few-shot 프롬프트로 조립되도록 구성.
*   `FEWSHOT_FIXED_EXAMPLES`, `FEWSHOT_CATEGORY_DEFS` 상수 신설, `syncFewshot()` / `getFsTextRaw()` 재작성.
*   `state.plan.fewshot`에 `testInput`, `aiOutput` 필드 및 localStorage 복원 로직 추가.

## 3. 검증

*   `node --check`로 인라인 스크립트 전체 문법 검증 통과.
*   로컬 `python3 -m http.server`로 파일을 서빙해 브라우저(Claude in Chrome)로 실제 동작 확인:
    *   Stage 3 정답 선택 → 로딩 연출 → 보너스 라운드(테스트 문장/힌트/입력창) 정상 표시.
    *   보너스 라운드에 패턴에 맞는 문장 입력 후 채점 → "🎉 [실전 적용 통과]" 정상 반환.
    *   플랜 빌더 FEWSHOT 스텝(`goToPlanStep(1)`)에서 고정 모범 예시 박스, "내 예시" 라벨, 실전 테스트
        입력창이 의도한 대로 렌더링됨을 스크린샷으로 확인.
    *   `#fs-p1-bad`/`#fs-p1-good`/`#fs-test-input` 값 입력 후 `syncFewshot()` 호출 → `#fsPreview`에
        모범 예시 + 내 예시 + 테스트 입력이 결합된 최종 few-shot 프롬프트가 의도한 포맷대로 생성됨을 확인.
*   `buildFinalPlan()`은 `getFsTextRaw()`를 그대로 호출하므로 별도 수정 없이 최종 MISSION KIT 결과물에도
    강화된 few-shot 섹션이 자동 반영됨.

## 4. 추가 요청 — 죽은 코드 정리 및 보너스 라운드 채점 방식 개선

### 죽은 코드 정리
`showStage3Question`, `handleStage3Answer`, `setupStage3`가 파일 내에 각각 2번씩 선언되어 있던 것을
확인 (`Docs/day3_workbook_fix_log.md`에 기록된 이전 세션의 복구 작업 과정에서 남은 잔재로 추정).
JS 함수 재선언 특성상 나중에 선언된 쪽만 실제로 실행되고 있어 동작에는 문제가 없었으나, 가독성을 위해
앞쪽의 죽은 선언 블록(구 `day3_integrated_workbook.html:2089-2205`, `showStage3Question` +
중첩된 `handleStage3Answer`)을 삭제. 삭제 후 `node --check` 문법 검증 및 Stage 3 → 보너스 라운드
흐름 재검증 통과.

### 보너스 라운드: "제출 후 모범 답안 비교" 방식으로 개선
사용자 피드백: 기존 보너스 라운드가 숫자/키워드 포함 여부만으로 자동 채점되어, 실제로 few-shot 예시의
패턴을 이해했는지와 무관하게 통과할 수 있어 학습 체험이 여전히 막연하다는 지적. 두 가지 방향
(①제출 후 모범 답안 비교 공개, ②패턴 먼저 식별 후 작성)을 제안했고 ①(라이트) 선택.

*   `STAGE3_DEFS`의 각 미션 `bonus` 객체에 `modelAnswer`(AI라면 이렇게 바꿨을 예시)와
    `patternLabel`(핵심 패턴 요약) 필드 추가.
*   `#stage3-bonus` 안에 `#bonus-compare` 블록 신설: "[내 문장]" vs "[AI라면 이렇게 바꿨을 거예요]"를
    나란히 보여주고, 패턴 라벨을 명시해 직접 비교하도록 함.
*   `checkBonusRound()`를 수정해 통과/실패와 무관하게 제출할 때마다 비교 블록을 공개하도록 변경
    (기존에는 통과 시에만 짧은 성공 메시지만 노출).
*   기존에는 통과 시 1.2초 후 자동으로 다음 미션으로 넘어갔으나, 비교 내용을 충분히 읽을 수 있도록
    자동 진행을 없애고 "다음 미션으로 →" 버튼(`#bonus-next-btn`, 통과 시에만 노출)을 눌러야 넘어가는
    방식으로 변경. 실패 시에는 비교 블록만 보이고 버튼은 숨겨진 채 재입력·재채점 가능.
*   `showBonusRound()`에 새 요소(`#bonus-compare`, `#bonus-next-btn`) 초기화 로직 추가.

## 5. 검증 (추가 작업분)

*   `node --check`로 문법 재검증 통과.
*   브라우저에서 실패 케이스("그냥 좋아요" 입력) → 비교 블록 노출 + "다음 미션으로" 버튼 숨김 확인.
*   성공 케이스(패턴에 맞는 문장 입력) → 비교 블록 + "다음 미션으로" 버튼 노출 확인, 버튼 클릭 시
    `state.game.currentStage3Index` 증가 및 다음 문항으로 정상 전환 확인.
*   스크린샷으로 [내 문장]/[AI라면 이렇게 바꿨을 거예요] 비교 UI가 의도한 레이아웃으로 렌더링됨을 확인.

## 6. 플랜 빌더 FEWSHOT 단계 — 예시 찾는 법 안내 + 이중 스크롤 제거

사용자 피드백: (1) "모범 예시(고정)"만 보여줘서는 그와 같은 패턴의 "내 예시"를 어떻게 만들어야 할지
막막하다, (2) ①제품/②타겟/③성과 3개 입력 블록이 자체 스크롤 박스(`max-h-[300px] overflow-y-auto`)
안에 갇혀 있어 페이지 스크롤과 별개로 한 번 더 스크롤해야 하는 이중 스크롤이 불편하다.

*   `plan-view-1`(`day3_integrated_workbook.html:1012` 부근) 헤더 아래에 "💡 모범 예시를 참고해서 내
    예시를 만드는 법" 안내 박스 추가: ① 모범 예시의 나쁜→좋은 문장을 비교해 무엇이 바뀌었는지 찾기,
    ② 그 변화 규칙을 우리 팀 문장에 그대로 적용해보기, 2단계로 설명.
*   3개 카테고리(①제품/상품, ②타겟, ③성과)의 "모범 예시(고정)" 박스 바로 아래에 각각 "🔍 패턴 힌트"
    한 줄을 추가해 무엇이 바뀌었는지 구체적으로 명시:
    - ① 감정적 표현 → 구체적 수치·시간·기능
    - ② 막연한 대상 → 구체적 상황·시간·장소
    - ③ 막연한 기대 → 기한 + 정량 수치 목표
    (Stage 3 게임 보너스 라운드에 추가했던 `patternLabel`과 동일한 관점으로 통일)
*   3쌍 입력을 감싸던 `<div class="space-y-3 max-h-[300px] overflow-y-auto pr-1">`에서
    `max-h-[300px] overflow-y-auto pr-1`을 제거해 내부 스크롤을 없애고, 페이지 전체 스크롤 한 번으로
    3개 카테고리를 모두 훑어볼 수 있도록 변경.
*   브라우저에서 `goToPlanStep(1)`로 재확인: 안내 박스·패턴 힌트 정상 노출, 이중 스크롤 없이 3개
    카테고리가 자연스럽게 이어짐, `syncFewshot()` 입력·미리보기 동작 이상 없음. `node --check` 통과.

## 7. 플랜 빌더 FEWSHOT 단계 — SKELETON(ISI) 내용 가져오기 기능 추가

사용자 피드백: "내 예시"를 백지에서 새로 써야 해서 부담이 크다, 앞 단계인 SKELETON(ISI) 입력 내용이나
Day2 미션 키트를 활용해 예시뱅크 작성을 도울 수 없는지 문의. Day2 미션 키트는 자유 서술 5개 필드
(`제목/리드/인사이트/제안/다음 스텝`)로 구성되어 있어 제품/타겟/성과 3개 카테고리에 깔끔하게
매핑되는 필드가 없고, 브라우저/오리진 세션이 같아야만 `day2_workbook_state` 로컬스토리지를 읽을 수
있다는 제약이 있어 데이터 소스로는 부적합하다고 판단, 이를 사용자에게 안내하고 **SKELETON(ISI)만
활용(라이트)**으로 스코프를 확정.

*   `day3_integrated_workbook.html:1040` 부근(①제품/상품), `:1066`(②타겟), `:1092`(③성과) 각 카테고리의
    🔍 패턴 힌트 아래에 "📎 SKELETON 참고 (Strategy/Insight/Impact): '…'" 참조 박스와 `[가져오기]` 버튼
    (`fs-p1-isi-ref` / `fs-p2-isi-ref` / `fs-p3-isi-ref`) 신설.
*   매핑: Strategy → ①제품/상품(p1), Insight → ②타겟(p2), Impact → ③성과(p3)
    (`FEWSHOT_ISI_MAP` 상수, `day3_integrated_workbook.html:2534` 부근).
*   `renderFewshotIsiRefs()` 신설: `state.plan.isi`의 각 필드 값을 참조 박스에 표시하고, 값이 비어 있으면
    "(SKELETON 단계에서 아직 입력하지 않았어요)"로 안내하며 가져오기 버튼을 비활성화.
*   `importIsiToFewshot(key)` 신설: 클릭 시 해당 ISI 값을 "내 예시: 나쁜 문장" 텍스트영역(`fs-p1-bad` 등)에
    채우고 `syncFewshot()`을 호출해 미리보기·상태를 동기화. ISI 값이 비어 있으면 아무 동작도 하지 않음
    (버튼 비활성화와 별개로 함수 자체도 방어적으로 처리).
*   `goToPlanStep(stepIdx)`가 FEWSHOT 스텝(stepIdx===1)으로 전환될 때마다 `renderFewshotIsiRefs()`를
    호출하도록 연결 — 모든 진입 경로(최초 로드, 로컬스토리지 복원, 배지 클릭, 이전/다음 버튼)가 공통으로
    `goToPlanStep`을 거치므로 별도 훅 없이 항상 최신 ISI 값이 반영됨.

### 검증
*   `node --check`로 문법 검증 통과.
*   브라우저에서 `syncSkeleton()`으로 Strategy/Insight만 입력 → FEWSHOT 단계 진입 시 ①②는 값 노출 +
    버튼 활성화, ③(Impact 미입력)은 안내 문구 + 버튼 비활성화 확인.
*   ①②의 가져오기 버튼 클릭 → 해당 "내 예시: 나쁜 문장" 텍스트영역에 ISI 텍스트가 정확히 채워지고
    `state.plan.fewshot.p{1,2}.bad` 및 `#fsPreview` 미리보기에 즉시 반영됨을 확인. ③(비활성화 버튼)은
    프로그램적으로 클릭을 강제해도 값이 채워지지 않음을 확인(방어 로직 정상 동작).
*   SKELETON 단계로 되돌아가 Impact를 입력한 뒤 다시 FEWSHOT 단계로 이동 → ③ 참조 박스가 새 값으로
    갱신되고 버튼이 활성화됨을 확인(동적 갱신 정상 동작).
*   스크린샷으로 3개 카테고리 모두 참조 박스·가져오기 버튼이 의도한 레이아웃으로 렌더링됨을 확인.

## 8. 비고

*   화면 캡처는 Claude in Chrome 확장 도구로 촬영해 육안 검증에는 사용했으나, 도구 특성상 로컬
    디스크 경로로 저장되지 않아 `Docs/sessions/screenshots/`에 파일로 남기지는 못함.

## 9. Few-shot 실습 단계 걷어내기 — 용어만 순화, 구조는 유지

사용자 요청: "그 방향으로 정리해줘, few-shot 실습 단계 걷어내줘". `AskUserQuestion`으로 Stage 3(실습1) 처리
방식을 확인한 결과 **"용어만 순화, 구조는 유지(Recommended)"** 선택 — 3단계 게임 구조·객관식·채점/진행
로직은 그대로 두고 "퓨샷/Shot1/Shot2" 용어만 "막연한 문장 vs 구체적인 문장 고르기"로 순화, 보너스
라운드(4~7절에서 추가했던 실전 적용 라운드)는 완전히 삭제.

### 실습2 (플랜 빌더) — FEWSHOT 스텝 완전 삭제
*   `plan-view-1`(FEWSHOT 스텝) 블록 전체 삭제: 안내 박스, 3개 카테고리(제품/타겟/성과) 입력 텍스트영역,
    SKELETON 참고 박스·가져오기 버튼, 실전 테스트 입력/AI 응답 붙여넣기 블록, `#fsPreview` 미리보기.
*   배지 4개(`p-badge-0~4`, FEWSHOT 포함 5단계) → 3개(`p-badge-0~3`, SKELETON/PAGES 1-4/PAGES 5-8/LAUNCH
    4단계)로 재번호. `goToPlanStep()`의 루프 상한(`i<=4`→`i<=3`)과 STEP 라벨(`STEP X / 5`→`STEP X / 4`)도
    함께 수정.
*   `state.plan.fewshot` 객체를 `state` 전역에서 완전히 제거. `loadStateFromLocalStorage()`의 fewshot 복원
    블록(값 복원 + 8개 DOM 요소 값 주입)도 함께 삭제.
*   `FEWSHOT_ISI_MAP`, `renderFewshotIsiRefs()`, `importIsiToFewshot()`, `syncFewshot()` 4개 함수 삭제.
    `FEWSHOT_FIXED_EXAMPLES`/`FEWSHOT_CATEGORY_DEFS` 상수는 `getFsTextRaw()`가 계속 참조하므로 유지.
*   `getFsTextRaw()`를 고정 모범 예시만으로 스타일 가이드 텍스트를 생성하도록 단순화(학생 입력 의존 제거).
    `buildFinalPlan()`이 MISSION KIT에 넣는 섹션 라벨도 `[Few-shot 문장 가이드]` → `[문장 스타일 가이드
    (예시 참고)]`로 순화.
*   `DOMContentLoaded` 초기화 블록(최초 렌더/복원 두 분기)에 남아있던 `syncFewshot();` 호출 2곳 삭제.

### 실습1 (Stage 3 게임) — 보너스 라운드 삭제 + 용어 순화
*   `#stage3-bonus` UI 블록(테스트 문장/입력창/힌트/채점 버튼/비교 피드백/다음 버튼) 전체 삭제.
*   `showBonusRound()`, `checkBonusRound()` 함수 삭제. `handleStage3Answer()`의 정답 처리 마지막 단계를
    `showBonusRound(...)` 호출에서 `advanceStage3()` 직접 호출로 변경.
*   `showStage3Question()`/`resetGame()`에 남아있던 `#stage3-bonus` 초기화 참조 삭제.
*   `STAGE3_DEFS` 3개 미션의 제목/설명/보기/보드 UI 텍스트에서 "퓨샷/Shot 1/Shot 2" 표현을 "참고 예시/같은
    패턴으로 문장 다듬기"로 순화. 각 미션의 `bonus` 서브 객체(테스트 입력/모범 답안/패턴 라벨 등)도 함께
    삭제. 채점 로직(`updateGrade`의 60→73→86→100% 배분, `advanceStage3()` 흐름, 인내심/오답 페널티)은
    손대지 않음.
*   게임 인트로/아웃트로 화면 텍스트에서 "(Few-Shot 수치 튜닝 가공)" → "(문장 구체화 튜닝)"으로 순화.

### 배경/참고 콘텐츠는 그대로 유지 (스코프 아님)
*   학습가이드 탭의 "개념 2: Few-shot 프롬프팅" 용어사전 카드(모달, 웹툰 이미지 포함)와 AI 도구 정보
    탭의 "기업 CS Few-shot 활용" 사례는 실습(exercise)이 아닌 배경 지식이므로 삭제 대상에서 제외하고
    그대로 유지.

### 검증
*   Python 정규식으로 인라인 `<script>` 2개 블록을 추출해 `node --check` 문법 검증 — SYNTAX OK.
*   `python3 -m http.server 8791`로 로컬 서빙 후 Claude in Chrome으로 기능 검증:
    *   Plan Builder: 배지 4→3개(SKELETON/P.1-4/P.5-8/LAUNCH)로 정상 렌더링, `state.plan.fewshot` 부재
        확인, LAUNCH 단계 MISSION KIT 출력에 "문장 스타일 가이드" 섹션이 고정 예시 기반으로 정상 포함됨을
        `textContent.includes(...)`로 확인.
    *   Stage 3: 3개 미션 모두 정답 선택 → 보너스 라운드 없이 `advanceStage3()`로 바로 다음 문항 전환,
        등급 60%→73%→86%→100% 정상 산출, 마지막 문항 통과 시 "최종 결과 확인" 버튼 정상 노출.
    *   `read_console_messages`로 에러 패턴 검색 — 콘솔 에러 없음.
    *   스크린샷으로 Stage 3 보드(보너스 블록 삭제 후에도 `#stage3-feedback`~하단 네비 사이 여백/레이아웃
        깨짐 없음)와 Plan Builder SKELETON/LAUNCH 단계 화면이 의도한 대로 렌더링됨을 육안 확인.
    *   용어사전 "개념 2: Few-shot 프롬프팅" 카드와 AI 도구 정보 탭의 CS 사례가 그대로 남아있음을 grep으로
        재확인.
