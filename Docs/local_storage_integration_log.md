# 📝 [Scribe Log] 통합 워크북 로컬 스토리지 상태 저장/복원 시스템 구축 보고서

- **작성일자:** 2026년 7월 11일
- **작성자:** 서기 (Doc)
- **작업 목적:** 학생들이 미션을 수행하며 입력한 모든 상태 데이터(입력값, 진행 스텝, 활성 탭 등)가 브라우저를 새로고침하거나 실수로 창을 닫아도 소실되지 않고 실시간으로 복원되도록 보장하는 로컬 스토리지 상태 관리 시스템 연동.

---

## 👥 1. 프로젝트 팀 기여 및 역할 회고

이 프로젝트는 다학제간 6대 역할을 기반으로 다음과 같이 유기적으로 협업하여 완료하였습니다.

1. **Blueprint - 건축가 (Architect):**
   - Day 1부터 Day 5까지 독립적으로 운용되는 고유의 로컬 스토리지 키 관리 정책 수립 (`dayN_workbook_state`).
   - 복원 대상이 되는 전역 상태 객체(`state`)와 화면 UI 싱크 흐름 설계.
2. **Relector - 작업자 (Worker):**
   - 각 일차별 통합 워크북 HTML에 `saveStateToLocalStorage()` 및 `loadStateFromLocalStorage()` 함수 탑재.
   - 각 입력 필드 동기화 함수(`sync...`), 단계 네비게이션(`goToStep`), 탭 스위치(`switchGuideTab`) 내부의 핵심 이벤트를 추적하여 실시간 저장 트리거 주입.
3. **Fix - 해결자 (Solver, AS):**
   - 대용량 HTML 파일 수정 시 정규식 오버랩 및 라인 꼬임 에러 발생을 감지하고, 수정 작업 범위를 쪼개 순차적으로 치환을 적용하여 코드 안전성 확보.
4. **Counselor - 상담가 (Consultant):**
   - 한글 자소 분리 파일 경로 및 디코딩 오류 발생 시 근본 원인(시스템 인코딩 디코딩 불일치)을 분석하고, 안전하게 치환을 자동화하는 파이썬 커스텀 스크립트 도구를 작성하여 우회할 수 있는 기술적 해결 방향 제안.
5. **UI Polish - 디자이너 (Designer):**
   - 상태 복원 시 단순히 값만 입력되는 것에 그치지 않고, 복원된 플래그에 맞추어 퀴즈 정답 결과의 시각적 피드백 연출 및 탭 하이라이트 동적 활성화 UI가 온전하게 유지되도록 조율.
6. **Doc - 서기 (Scribe):**
   - 작업 단계 및 태스크 진척도를 `task.md`를 통해 추적 및 관리하고, 최종 결과물 론칭 로그를 작성하여 깃허브 업로드 전 보존 조치 완료.

---

## 🛠️ 2. 구현 내역 (Implementation Details)

### 2.1 로컬 스토리지 키 정책
일차별로 독립된 데이터를 보장하기 위해 브라우저 세션 간 충돌이 없는 전용 스토리지 키를 배정하여 구현했습니다.
* **Day 1:** `day1_workbook_state` (OKR & 발표 마스터)
* **Day 2:** `day2_workbook_state` (시장 조사 - 기구현 완료 상태에서 예외 케이스 보완)
* **Day 3:** `day3_workbook_state` (3줄 기획서 ISI & 8장 슬라이드 기획)
* **Day 4:** `day4_workbook_state` (구글 스티치 & Netlify 글로벌 배포)
* **Day 5:** `day5_workbook_state` (포멜리 SNS 광고 & AI 피칭 론칭)

### 2.2 핵심 구현 자바스크립트 구조
각 HTML 파일 하단 `<script>` 블록에 아래와 같은 복원/저장 아키텍처가 공통 설계되었습니다. (예시: Day 4)

```javascript
// 로컬 스토리지 상태 저장 함수
function saveStateToLocalStorage() {
    try {
        localStorage.setItem('day4_workbook_state', JSON.stringify(state));
    } catch (e) {
        console.error("로컬 스토리지 저장 실패:", e);
    }
}

// 로컬 스토리지 상태 로드 함수
function loadStateFromLocalStorage() {
    try {
        const saved = localStorage.getItem('day4_workbook_state');
        if (saved) {
            const parsed = JSON.parse(saved);
            
            // 전역 상태 병합
            if (parsed.activeMainTab !== undefined) state.activeMainTab = parsed.activeMainTab;
            if (parsed.activeGuideTab !== undefined) state.activeGuideTab = parsed.activeGuideTab;
            if (parsed.currentStep !== undefined) state.currentStep = parsed.currentStep;
            if (parsed.isQuizCleared !== undefined) state.isQuizCleared = parsed.isQuizCleared;
            if (parsed.isDeployCleared !== undefined) state.isDeployCleared = parsed.isDeployCleared;
            if (parsed.teamName !== undefined) state.teamName = parsed.teamName;
            if (parsed.netlifyUrl !== undefined) state.netlifyUrl = parsed.netlifyUrl;
            
            if (parsed.inputs) {
                state.inputs.brandName = parsed.inputs.brandName || '';
                state.inputs.slogan = parsed.inputs.slogan || '';
                state.inputs.color = parsed.inputs.color || '';
                state.inputs.target = parsed.inputs.target || '';
                state.inputs.benefit = parsed.inputs.benefit || '';
                state.inputs.cta = parsed.inputs.cta || '';
            }

            // UI 바인딩 요소 값 실시간 강제 복원
            document.getElementById('teamName').value = state.teamName;
            document.getElementById('inp-brand-name').value = state.inputs.brandName;
            document.getElementById('inp-slogan').value = state.inputs.slogan;
            document.getElementById('inp-color').value = state.inputs.color;
            document.getElementById('inp-target').value = state.inputs.target;
            document.getElementById('inp-benefit').value = state.inputs.benefit;
            document.getElementById('inp-cta').value = state.inputs.cta;
            document.getElementById('inp-netlify-url').value = state.netlifyUrl;

            return true;
        }
    } catch (e) {
        console.error("로컬 스토리지 로드 실패:", e);
    }
    return false;
}
```

---

## 🚨 3. 발생 문제 및 해결기 (Troubleshooting & Tips)

### 3.1 라인 변경에 따른 치환(Replace) 어긋남 방지
- **문제점:** 파일 크기가 수천 라인에 달해 자바스크립트의 다중 영역을 동시에 수정할 때, 앞선 치환으로 라인 번호가 실시간 변동하여 후속 치환이 실패하거나 코드가 임의의 위치로 삽입되어 꼬이는 현상 발생.
- **해결책:** 
  1. 수정을 다중 비연속 덩어리로 무리하게 적용하기보다, 함수별로 Target 범위를 좁게 잡아서 순차적으로 적용.
  2. 수정 즉시 `grep_search`를 사용하여 대상 함수의 정확한 신규 줄 번호를 파악한 후 치환 진행.

### 3.2 HTML 파싱 인코딩 에러 우회 (자소 분리 자막 파일명 등)
- **문제점:** IDE의 `replace_file_content` 도구가 대형 HTML 파일을 가공할 때 인코딩 판정에 실패하는 에러(`Encountered error in step execution: while decoding file, failed to detect charset with sufficient confidence`)가 발생하여 파일 저장 불가.
- **해결책:** 
  - 로컬 Python 3 환경을 활용하여 인코딩 우회 치환 유틸리티 스크립트(`replace_tool.py`)를 개발.
  - 치환할 원본과 대상 텍스트를 임시 `.txt` 파일에 쓴 후, Python 스크립트의 `open(..., encoding='utf-8')`로 파일 내용을 교체함으로써 인코딩 판독 에러 문제를 완전히 우회하여 빠른 작업이 가능하도록 함.

---

## 📢 4. 깃허브 업로드 전 최종 안내 사항
- 본 수정 작업이 완료됨에 따라 학생들은 미션 진행 중 새로고침을 하거나 다른 탭으로 이동했다 돌아와도 이전에 입력해 두었던 팀 정보, 브랜드 프롬프트 및 배포 주소 등이 완전하게 유지됩니다.
- 본 프로젝트 관련 추가 안내 문서 및 플랫폼 소개는 **아크랩스 공식 홈페이지**([https://litt.ly/aklabs](https://litt.ly/aklabs))를 참고해 주십시오.
