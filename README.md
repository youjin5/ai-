# ai 에이전트 
 - 소상공인 및 중소기업을 위한 정책자금 추천 + 문서 자동 생성 + 사후 관리 원스톱 AI 서비스
 - TF-IDF 기반 RAG 시스템과 LLM을 결합하여, 실행 역량이 부족한 기업들도 손쉽게 정책자금과 금융상품에 접근할 수 있도록 지원

## 프로그램 개요
 ### 프로젝트 명 
  AI 금융지원 에이전트

 **목표** 소상공인 및 중소기업의 금융 접근성과 실행역량 제고

 **주요 기능**
  - 사용자 입력 기반 맞춤형 정책자금 및 금융상품 추천
  - RAG 기반 정책/금융 정보 질의응답 및 요약
  - 필요한 서류 자동 생성 (사업계획서, 자금신청서 등)
  - 블록체인 기반 MCP(Multi-chain Credential Proof) 검증 구조 적용 가능

## 시스템 아키텍처 (이미지로 수정)
 사용자 입력
↓
[Query Embedding]
↓
[Vector DB (TF-IDF + FAISS)]
↓
[Top-K 문서 Chunk Retrieval]
↓
[LLM (GPT 기반) 응답 생성]
↓
질의응답
정책 요약
서류 자동 생성


## 기술 스택

| 구성요소 | 사용 기술 |
|----------|------------|
| 백엔드 API | Python (FastAPI) |
| 프론트엔드 | React (Vite + Tailwind) |
| LLM | OpenAI GPT-4 / LLAMA2 |
| 벡터 DB | FAISS + TF-IDF |
| 문서 처리 | langchain + PyMuPDF / pdfplumber / python-docx |
| RAG 구현 | Langchain / custom pipeline |
| 배포 환경 | Docker / GitHub Actions |
| 버전관리 | GitHub |
| 기타 | dealsbe.com 내 테스트 서버 기반 |

  **핵심 로직**
- 언어 및 라이브러리: Python, scikit-learn, docx
- 데이터 처리: pandas를 이용한 데이터 구조화
- 검색 모델 (RAG):
  - VectorStore: 간단한 TF-IDF 기반 SimpleVectorStore를 자체 구현하여 정책 및 금융상품 문서를 벡터화하고 인덱싱합니다.
  - Retrieval: 사용자의 사업 프로필을 질의(Query)로 변환하여 VectorStore에서 가장 유사한 문서를 검색합니다.
  - Rewriting: 검색된 문서의 메타데이터(키워드 등)를 활용하여 추가 점수를 부여, 추천 순위의 정확도를 높입니다.
- 문서 생성: python-docx 라이브러리를 사용하여 MS Word(DOCX) 형식의 문서를 생성합니다.


## 요구사항 (Requirements)
### 소프트웨어
- Python 3.10+
- pip (Python 패키지 관리자)
- OS: Windows / macOS / Linux

### 주요 라이브러리
- `transformers` - Hugging Face 모델 사용
- `sentence-transformers` - 쿼리 임베딩 생성
- `faiss` - 벡터 검색
- `langchain` - RAG 파이프라인 구성
- `gradio` or `streamlit` - 사용자 UI
- `docx` - 문서 자동 생성
- `pypdf`, `beautifulsoup4` - 문서 수집 및 정제
- `scikit-learn` - TF-IDF 구축

## 설치 방법 (Installation)
```bash
 1. 리포지토리 클론
git clone https://github.com/yourname/ai-finance-agent.git
cd ai-finance-agent

 2. 가상환경 생성 및 진입 (선택)
python -m venv venv
source venv/bin/activate  # Windows는 venv\Scripts\activate

 3. 라이브러리 설치
pip install -r requirements.txt

 4. .env 설정 (API KEY, 디렉터리 경로 등)
cp .env.example .env
```

## 실행 방법 ---> 파이썬? 코드 실행 방법인가요?
```bash
 1. 로컬 환경 설정: Python 3.8 이상이 설치되어 있는지 확인하고, 프로젝트 저장소를 클론합니다.
   git clone [Github Repository URL]
   cd [repository-name]

 2. 의존성 설치: 필요한 Python 패키지를 설치합니다.
   pip install -r requirements.txt

 3. 서비스 실행: 데모 CLI를 통해 정책자금 또는 금융상품 추천 서비스를 실행합니다.
   - 정책자금 추천 실행(예시): python policy_agent.py
   - 금융상품 추천 실행(예시): python financial_agent.py

1. 벡터 DB 구축 (최초 1회)
python backend/setup_vector_db.py

2. 서버 실행
uvicorn backend.main:app --reload

3. 프론트엔드 (선택)
cd frontend
npm install
npm run dev

```

## 결과 확인 방법

 **결과 확인**
- 터미널에 표시되는 안내에 따라 사업 정보를 입력하거나 미리 정의된 예시 데이터를 사용합니다.
- 추천된 정책 또는 상품 목록을 확인하고, 번호를 선택하여 상세 정보를 볼 수 있습니다.
- "신청서 초안을 생성하시겠습니까?" 질문에 Y를 입력하면, ./ 디렉토리에 [문서명]_초안_[기업명].docx 파일이 생성됩니다.

 1. 추천 결과
- 금융상품/정책자금 추천 리스트와 세부조건이 웹 UI에 출력됨
- 우선순위, 적합도, 필수 제출 서류, 신청절차 포함

 2. 자동 생성 문서
- 폴더에 Word 파일(.docx) 자동 저장
예: 사업계획서_2025_홍길동.docx, 자금신청서_소상공인.docx
