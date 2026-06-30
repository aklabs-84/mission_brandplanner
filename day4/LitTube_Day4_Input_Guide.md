# 🌐 DAY 4 실습 입력 가이드: 노코드 랜딩 페이지 빌딩 (LitTube)

* **프로젝트명**: LitTube (리튜브)
* **제출팀**: 글터수호대 (문해력 구원단)
* **사용 도구**: NotebookLM (브랜드 소스) → Gemini Gems (브랜드 매니저 생성) → Gemini (광고 이미지) → Google Stitch (사이트 생성) → Netlify (배포)
* **작성일자**: 2026년 6월 28일

---

## 1. STEP 2 — 브랜드의 두뇌 구축 (NotebookLM 소스 & Gemini Gems)

### ① NotebookLM — 소스로 추가할 내용

Day 3 기획서 내용을 구글 문서로 정리한 뒤 NotebookLM 소스로 추가합니다.
아래 내용을 구글 문서에 그대로 붙여넣어 소스로 등록하세요.

**[구글 문서 → NotebookLM 소스 추가용 텍스트]**

```
=== LitTube(리튜브) 브랜드 핵심 정보 ===

서비스명: LitTube (리튜브)
팀명: 글터수호대

[타겟 고객]
유튜브 숏폼에 과몰입하여 활자 기피증을 겪는 초·중·고등학생.
이들의 학부모 (교육 효과에 불안감을 가진 30~40대 부모).
학교 국어 선생님 및 교육청 담당자.

[핵심 문제]
청소년 하루 스마트폰 사용 4.8시간, 연간 독서량 1.2권.
교과서 지문을 반 이상 이해 못하는 알파세대 문해력 위기.
기존 독서 강제 교육은 이탈율 80%로 역효과.

[솔루션 — 제품 컨셉]
유튜브 URL 입력 → AI(Gemini 2.5)가 초중고 맞춤 소설/시/수필 3종 자동 변환.
독서 펫 '글몽이' 레벨업 + 퀴즈 + 단어장 수집 = 게이미피케이션 독서 루프.
스마트 사전: 어려운 단어 클릭 시 초중고 맞춤 설명 팝업.

[핵심 성과 수치]
파일럿 독서 완료율: 85% (기존 20% 대비 4.2배)
목표 MAU: 5만 명 (6개월)
목표 ARR: 3.6억 원 (학교 구독 100개 학급)
시장 규모: 에듀테크 8.2조, 문해력 세그먼트 3,500억, 연 12.5% 성장

[브랜드 핵심 가치]
"유튜브를 문학으로 바꾸다"
"게임처럼 시작해서 문학으로 끝난다"
"아이들이 이미 있는 곳에서 만난다"

[경쟁 차별점]
기존 문해력 앱: 지루한 고전 문학 강제 노출 → 이탈 높음
리튜브: 학생이 좋아하는 유튜버 영상이 소재 → 몰입 극대화

[가격 정책]
B2C: 월 9,900원 (학생 개인 구독)
B2B: 월 30,000원 (학급 단위 구독, 학생 30명 기준)
B2G: 교육청 연간 라이선스 협의

[CTA 후보]
"🐾 글몽이와 무료 체험 시작하기"
"📚 우리 학급 단체 신청하기"
"선생님용 무료 파일럿 신청"
```

### ② Gemini Gems — '글튜브 AI 브랜드 매니저' 생성 설정값

Gemini Gems(gem.google.com) 에서 새 Gem을 만들 때 아래 내용을 입력하세요.

**Gem 이름:**
```
글튜브 AI 브랜드 매니저
```

**Gem 설명 (지침 입력란):**
```
너는 에듀테크 스타트업 'LitTube(리튜브)'의 전담 브랜드 매니저 AI야.

[브랜드 핵심 정보]
- 서비스명: LitTube (리튜브) — 유튜브 스크립트를 AI로 소설/시/수필 변환 + 독서 펫 글몽이 게이미피케이션
- 타겟: 유튜브 과몰입 초·중·고등학생 / 학부모(30~40대) / 국어 선생님 / 교육청
- 핵심 메시지: "유튜브를 문학으로 바꾸다" / "아이들이 이미 있는 곳에서 만난다"
- 핵심 수치: 독서 완료율 85%, 단어 습득 4.2배, 목표 MAU 5만, 연 ARR 3.6억
- 경쟁 우위: 영상 소재 + 게이미피케이션 결합 = 시장 공백 영역

[톤앤보이스]
- 학생에게: 밝고 친근하며 게임처럼 신나는 말투
- 학부모·교사에게: 따뜻하고 신뢰감 있는 교육 전문가 말투
- 투자자에게: 데이터 기반의 자신감 있는 비즈니스 말투

[내가 요청할 때 해줘야 할 것들]
1. "스티치용 웹사이트 생성 프롬프트 만들어줘" → 5섹션 구조로 바로 사용 가능한 프롬프트 3가지 제안
2. "카피라이팅 써줘" → Hero 카피, Problem 카피, CTA 버튼 텍스트 각 3가지 제안
3. "이미지 생성 프롬프트 써줘" → Gemini 이미지 생성에 넣을 영어 프롬프트 제안
4. "SNS 게시물 써줘" → 인스타그램/카카오 채널용 학부모/학생 각각 버전 제안

항상 브랜드 일관성을 유지하고, 모든 제안은 3가지 버전으로 줘.
```

### ③ Gemini Gems에게 첫 번째 요청 (마케팅 소스 생성)

Gem을 만든 후 아래 질문을 채팅창에 입력하세요.

```text
Google Stitch로 LitTube(리튜브) 랜딩 페이지를 만들 거야.
아래 두 가지를 만들어줘.

1. Stitch 사이트 생성 프롬프트 (5섹션 구조 포함, 영문으로 작성)
   섹션: Hero / Problem & Solution / How It Works / Social Proof / CTA

2. 한국어 핵심 카피라이팅 3종
   ① Hero 헤드라인 (임팩트 있는 1줄)
   ② Problem & Solution 본문 (2~3줄)
   ③ CTA 버튼 텍스트 (행동을 유발하는 7단어 이내)
```

---

## 2. STEP 3 — 광고 이미지 생성 (Gemini 이미지 생성 프롬프트)

Gemini(gemini.google.com)의 이미지 생성 기능에 아래 프롬프트를 넣어 LitTube 광고 이미지를 만드세요.

### 메인 광고 이미지 프롬프트 (영문)

```
A warm and modern editorial-style advertisement image for an Korean edtech app called "LitTube".
Scene: A Korean middle school student sitting at a cozy desk at night, holding a smartphone showing a YouTube video,
and beside the screen a magical book is opening with glowing orange and teal particles floating out of it,
transforming into literary text. A cute small white creature pet (called 'Geulmong-i') is sitting next to the book, 
glowing softly and looking happy. The mood is warm, hopeful, and slightly magical. 
Color palette: warm cream white background, orange (#FF6B35) and teal (#1A9396) accents, soft lighting.
Style: 2026 modern Korean editorial illustration, clean and premium, no text overlay.
Aspect ratio: 16:9.
```

### 캐릭터 소개 이미지 프롬프트 (영문)

```
A cute mascot character design sheet for a Korean educational app.
The character is called 'Geulmong-i' (글몽이), a small round white creature that loves reading books.
It has big sparkling eyes, tiny wings made of book pages, and glows softly in warm orange.
Show 4 emotion states: sleeping (💤), running fast while reading, celebrating with arms raised (🎉), 
and leveling up with a golden crown.
Style: 2D flat illustration, clean, friendly, suitable for a Korean elementary to high school audience.
Background: transparent or soft cream.
```

---

## 3. STEP 4 — Google Stitch 사이트 빌딩 (입력 프롬프트)

stitch.google.com 에서 새 프로젝트를 만들고 아래 프롬프트를 입력란에 붙여넣으세요.

### Stitch 사이트 생성 프롬프트 (한글)

```
아래 구조로 한국 에듀테크 스타트업 'LitTube(리튜브)'의 원페이지 랜딩 사이트를 만들어줘.

[디자인 설정]
- 배경색: 따뜻한 크림 화이트 (#FDFBF7)
- 포인트 컬러: 오렌지 (#FF6B35), 테알 (#1A9396)
- 폰트: Noto Sans KR (한국어), Outfit (영어)
- 스타일: 2026 Glassmorphism + Bento Grid, 세련되고 따뜻한 에듀테크 느낌

[섹션 1 — Hero]
- 헤드라인: "유튜브를 문학으로 바꾸다"
- 서브헤드: "즐겨보던 유튜브 영상 링크 하나만 넣으면, AI가 초중고 맞춤 소설·시·수필로 변환해 드립니다."
- CTA 버튼: "🐾 글몽이와 무료 시작하기 →" (오렌지 버튼)
- URL 입력 인터페이스 박스 포함

[섹션 2 — Problem & Solution]
- Problem 카드: "스마트폰 4.8시간 vs 독서 1.2권 — 우리 아이의 문해력이 무너지고 있습니다."
- Solution 카드: "유튜브 영상 URL 하나로 문학 작품이 만들어지고, 독서 펫 글몽이가 아이와 함께 성장합니다."

[섹션 3 — How It Works (3단계)]
① 유튜브 URL 입력 / ② AI 소설·시·수필 변환 / ③ 퀴즈 + 글몽이 레벨업

[섹션 4 — Social Proof]
- 독서 완료율 85% / 단어 습득 4.2배 / 교사 얼리어답터 1,000명

[섹션 5 — Final CTA]
- 버튼 1: "🏫 우리 학급 단체 신청하기" (이메일 링크)
- 버튼 2: "지금 바로 무료 체험" (Hero 섹션 스크롤)
```

### 카피라이팅 수정 요청 프롬프트 (Stitch 내 AI 채팅)

사이트가 생성된 후 Stitch 내부 AI에게 아래 수정 요청을 순서대로 합니다.

```text
수정 요청 1: Hero 섹션의 헤드라인 글자 크기를 더 크게 하고, 
"글몽이와 무료 시작하기" 버튼 색을 오렌지(#FF6B35)로 변경해줘.

수정 요청 2: How It Works 섹션의 아이콘을 각각 🔗(URL 입력), ✨(AI 변환), 🐾(레벨업)으로 교체해줘.

수정 요청 3: 푸터에 "© 2026 LitTube(리튜브) · 글터수호대 | AK LABS" 텍스트와 
링크(https://litt.ly/aklabs)를 추가해줘.
```

---

## 4. STEP 5 — Netlify 배포 후 URL 기록

사이트 완성 후 Export → Netlify 연동 → 배포 완료 후 아래 칸에 URL을 기록해 팀 미션 로그에 저장하세요.

| 항목 | 내용 |
| :--- | :--- |
| **배포 URL** | `https://littube-[팀이름].netlify.app` |
| **배포 일시** | 2026년 6월 28일 |
| **배포 상태** | ✅ LIVE |
| **QR코드 생성** | qr.io 또는 qrcode-monkey.com에서 URL로 QR 생성 |

**LitTube 예시 배포 URL:**
```
https://glittersuhodae-littube.netlify.app
```
