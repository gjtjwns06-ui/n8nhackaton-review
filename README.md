# n8nhackaton-review
# 📄 n8n AI Paper Reviewer & Reviser Workflows

이 레포지토리는 다중 LLM(GPT, Claude, Gemini)을 활용하여 학술 논문 및 일반 문서를 자동으로 평가하고, 여러 에이전트의 관점을 통해 문서를 반복적으로 개선(Revision)하는 **n8n 자동화 워크플로우 모음**입니다.

## ✨ 주요 기능 및 워크플로우 소개

이 프로젝트에는 다음과 같은 핵심 워크플로우들이 포함되어 있습니다. (JSON 파일을 n8n에 임포트하여 사용할 수 있습니다.)

### 1. 🤖 Multi-LLM 교차 평가 시스템 (Workflow 2 & 3)
* **설명**: GPT, Claude, Gemini 3개의 최상위 모델이 동일한 문서를 동시에 평가합니다.
* **평가 지표**: Originality(독창성), Validity(타당성), Generalizability(일반화 가능성)를 0~5점 척도로 평가합니다.
* **출력**: 각 모델별 평가 점수와 한국어 코멘트를 취합하여 최종 평균 점수를 산출하고, HTML 및 Markdown 형태의 표(Table)로 결과를 반환합니다.

### 2. 🔄 다중 에이전트 기반 반복 개선 루프 (Workflow 5)
* **설명**: 한 편의 논문을 두고 서로 다른 성향을 가진 AI 에이전트들이 토론하며 문서를 발전시키는 고급 워크플로우입니다. 지정된 횟수(`loops`)만큼 문서를 반복해서 수정합니다.
* **에이전트 구성**:
  * **Optimist (낙관론자)**: 문서의 강점과 살릴 수 있는 방향을 제시합니다.
  * **Pessimist (비관론자)**: 학회에서 Reject 될 수 있는 치명적인 논리적 비약과 누락 사항을 날카롭게 지적합니다.
  * **Neutral (중립/메타 리뷰어)**: 두 의견을 종합하여 최우선 개선 과제를 도출합니다.
  * **Editor (편집자)**: 리뷰 결과를 바탕으로 실제 문서 초안을 재작성(Revision) 합니다.

### 3. 📄 PDF 전처리 유틸리티 (Workflow 1 & 3-Variant)
* **설명**: Webhook이나 수동 트리거를 통해 PDF 파일이 업로드되면, 이를 텍스트로 변환(`PDF.co` API 또는 내장 추출 노드 활용)하고 LLM 컨텍스트 한도에 맞게 텍스트를 전처리(Slicing)합니다.

## 🛠 요구 사항 (Prerequisites)

* **n8n**: 로컬, Docker, 또는 Cloud 환경의 n8n 인스턴스
* **API Keys**:
  * OpenRouter API Key (여러 모델 라우팅용) 또는
  * OpenAI, Anthropic (Claude), Google (Gemini) API Key
  * PDF.co API Key (외부 PDF 파싱 워크플로우 사용 시)

## 🚀 사용 방법 (How to Use)

1. 이 레포지토리에서 원하는 워크플로우의 `.json` 파일을 다운로드하거나 코드를 복사합니다.
2. 실행 중인 n8n 대시보드로 이동합니다.
3. 새 워크플로우를 생성하고 우측 상단의 `Import from File`을 선택하거나, 화면에 바로 붙여넣기(`Ctrl+V` / `Cmd+V`)를 합니다.
4. 각 LLM 노드(OpenAI, OpenRouter 등)에 본인의 API Key(Credentials)를 연결합니다.
5. 수동 트리거 노드에서 `Test step`을 누르거나 설정된 Webhook URL로 POST 요청을 보내 워크플로우를 실행합니다.

