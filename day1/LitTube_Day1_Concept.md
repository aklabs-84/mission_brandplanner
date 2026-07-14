<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LitTube Day 1 Concept 정의서</title>
    <!-- Tailwind CSS (V4) -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Marked.js (마크다운 파서) -->
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <!-- Font & Icon -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;800&family=Outfit:wght@400;600;800&family=Space+Grotesk:wght@500;700&family=Fira+Code:wght@400;500&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background: linear-gradient(135deg, #f5f3ff 0%, #fdf2f8 100%);
        }
        .prose h1 { 
            font-family: 'Outfit', sans-serif; 
            font-weight: 800; 
            color: #4c1d95; 
            font-size: 1.875rem; 
            line-height: 2.25rem; 
            margin-bottom: 1.5rem; 
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        .prose h2 { 
            font-family: 'Outfit', sans-serif; 
            font-weight: 700; 
            color: #6d28d9; 
            font-size: 1.5rem;
            line-height: 2rem;
            border-bottom: 2px solid #e9d5ff; 
            padding-bottom: 0.5rem; 
            margin-top: 2.5rem; 
            margin-bottom: 1rem;
        }
        .prose h3 { 
            font-family: 'Outfit', sans-serif; 
            font-weight: 600; 
            color: #7c3aed; 
            font-size: 1.25rem;
            line-height: 1.75rem;
            margin-top: 1.5rem;
            margin-bottom: 0.5rem;
        }
        .prose table { 
            width: 100%; 
            border-collapse: collapse; 
            margin-top: 1.5rem; 
            margin-bottom: 1.5rem; 
            font-size: 0.9rem;
        }
        .prose th { 
            background-color: #f3e8ff; 
            color: #5b21b6; 
            padding: 0.75rem 1rem; 
            text-align: left; 
            border: 1px solid #e9d5ff; 
            font-weight: 700;
        }
        .prose td { 
            padding: 0.75rem 1rem; 
            border: 1px solid #e9d5ff; 
            color: #4b5563;
        }
        .prose tr:nth-child(even) { 
            background-color: #faf5ff; 
        }
        .prose code { 
            font-family: 'Fira Code', monospace; 
            background-color: #f3f4f6; 
            padding: 0.2rem 0.4rem; 
            border-radius: 0.25rem; 
            font-size: 0.875em; 
            color: #dc2626; 
            font-weight: 600;
        }
        .prose pre code { 
            background-color: transparent; 
            padding: 0; 
            color: inherit; 
            font-weight: 400;
        }
        .prose pre { 
            background-color: #1f2937; 
            color: #f9fafb; 
            padding: 1.25rem; 
            border-radius: 0.75rem; 
            overflow-x: auto; 
            font-family: 'Fira Code', monospace; 
            font-size: 0.85rem; 
            line-height: 1.5rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1); 
            margin-top: 1rem;
            margin-bottom: 1rem;
        }
        .prose a { 
            color: #2563eb; 
            text-decoration: underline; 
        }
        .prose ul {
            list-style-type: disc;
            padding-left: 1.5rem;
            margin-top: 0.5rem;
            margin-bottom: 0.5rem;
        }
        .prose li {
            margin-bottom: 0.25rem;
        }
        .prose hr {
            border: 0;
            border-top: 1px solid #e9d5ff;
            margin: 2rem 0;
        }
    </style>
</head>
<body class="min-h-screen py-10 px-4 md:px-8">
    <div class="max-w-4xl mx-auto bg-white rounded-3xl p-6 md:p-10 shadow-2xl border border-purple-100">
        <div id="content" class="prose max-w-none text-gray-800 leading-relaxed">
            <!-- 마크다운 렌더링 영역 -->
        </div>
    </div>

    <!-- 원본 마크다운 텍스트 영역 -->
    <textarea id="markdown-raw" style="display: none;"># 🧭 DAY 1 최종 산출물: 주제 컨셉 정의서 (Mission Kit)

* **프로젝트명**: LitTube (리튜브)
* **제출팀**: 글터수호대 (문해력 구원단)
* **작성일자**: 2026년 6월 28일

---

## 1. 팀 미션 키트 (DECLARE)

| 항목 | 상세 내용 |
| :--- | :--- |
| **팀명** | **글터수호대** |
| **타겟 고객** | **유튜브 숏폼 및 자극적인 영상 매체에 과몰입하여 문장 이해력이 저하된 초·중·고등학생** |
| **문제 정의** | 스마트폰 사용 시간은 급증(일평균 4.8시간)한 반면 줄글 독서량은 극도로 감소(월 0.5권 이하)하여, 긴 교과서 지문이나 문학 작품을 해독하지 못하고 어휘 단절 현상을 겪고 있음. |
| **컨셉 시드** | **유튜브 비디오 링크를 입력하면 초·중·고 수준에 맞는 문학(소설, 시, 수필)으로 변환해 주는 AI 서비스.** 귀여운 독서 펫 '글몽이'를 성장시키는 게이머형 성장 루프를 결합하여 문해력을 스스로 키우는 게이머형 에듀테크 앱. |

---

## 2. AI 킥오프 최종 프롬프트 (AI OKR)

학생들이 주제 컨셉을 고도화하고 시장 내 유사 경쟁 제품과의 차별성을 분석하기 위해 Gemini에 실제로 주입한 **AI OKR 최종 조립 프롬프트**입니다.

```text
[Goal]
유튜브 스크립트를 초중고 학생 맞춤형 문학 작품(소설/시/수필)으로 변환하는 교육 솔루션 'LitTube(리튜브)'의 컨셉 기획을 검증하고, 기존 독서 교육 및 에듀테크 서비스와의 차별점 분석표를 작성하려고 해.

[Context]
- 우리 팀은 대학생 가상 창업 미션 중이야. 예산은 0원이며 AI를 적극 활용하여 빠르게 MVP(최소 기능 제품)를 검증하려 해.
- 타겟은 스마트폰 숏폼 비디오에 지나치게 노출되어 활자 기피증과 어휘력 부족을 겪는 초·중·고등학생이야.
- 우리의 핵심 가치는 '아이들이 가장 오랜 시간 머무는 유튜브'를 '문학 독서의 장'으로 전환하고, 독서 펫 '글몽이'의 레벨업 등 게임 요소를 결합해 학습 동기를 부여하는 데 있어.

[Sample]
유사 서비스 비교 시 다음 양식을 지켜줘:
1. 서비스명 / 주요 제공 기능 / 리튜브 대비 강점 및 약점
2. 리튜브가 기존 문해력 앱보다 아이들에게 더 매력적으로 다가갈 수 있는 차별화 포인트 3가지

[Role]
너는 에듀테크 혁신 전문가이자 청소년 교육학 석사야. 아이들이 게임처럼 몰입할 수 있도록 아주 친근하고 명확한 비즈니스 톤앤보이스로 조언해줘. 2026년 최신 교육 기술 트렌드를 반영해 설명해줘.
```

---

## 3. AI 피드백 및 기획 구체화 내용

위 프롬프트를 Gemini에 실행하여 얻은 핵심 피드백과 최종 수렴된 컨셉 기획 포인트입니다.

### ① 기존 솔루션과의 차별점
- **기존 문해력 앱**: 지루한 고전 문학이나 교과서 지문을 억지로 읽게 함 ➔ **학생 이탈율 높음**.
- **리튜브 (LitTube)**: 침착맨 삼국지, 과학쿠키, 슈카월드 등 **학생이 평소 즐겨보던 최애 유튜브 영상**이 학습 텍스트 소스가 됨 ➔ **초기 몰입도 극대화**.

### ② 핵심 게이미피케이션 루프
- 독서 펫 **'글몽이'**는 텍스트를 읽을수록 성장(레벨업 및 형태 진화).
- 본문을 끝까지 스크롤하면(100% 독서 완수) 글몽이가 기뻐하며 춤추는 애니메이션과 함께 코인 보상 지급.
- 획득한 코인으로 퀴즈 스테이지에 도전하거나 글몽이 전용 테마 아이템 구매 연동.
</textarea>

    <script>
        document.addEventListener("DOMContentLoaded", () => {
            const rawMarkdown = document.getElementById("markdown-raw").value;
            // Marked 옵션 설정 (GFM 테이블 활성화 등)
            marked.setOptions({
                gfm: true,
                breaks: true
            });
            document.getElementById("content").innerHTML = marked.parse(rawMarkdown);
        });
    </script>
</body>
</html>
