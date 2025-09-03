
# 📡 News Voice Assistant (FastAPI + React)

> **음성 기반 뉴스 검색 & 요약 시스템**  
> 말로 뉴스를 요청하면 실시간 검색 → 요약 → 음성으로 읽어주는 풀스택 데모입니다.

<p align="left">
  <img alt="React" src="https://img.shields.io/badge/Frontend-React-61DAFB?logo=react&logoColor=white" />
  <img alt="FastAPI" src="https://img.shields.io/badge/Backend-FastAPI-009688?logo=fastapi&logoColor=white" />
  <img alt="OpenAI" src="https://img.shields.io/badge/AI-OpenAI-black?logo=openai" />
  <img alt="License" src="https://img.shields.io/badge/License-MIT-green" />
</p>

---

## 🎯 프로젝트 개요
현대인은 방대한 기사 속에서 핵심만 빠르게 파악하고 싶습니다. 이 프로젝트는 **음성 인터페이스** + **AI 요약**을 결합해, 사용자가 말로 뉴스를 요청하면 관련 기사를 모으고 핵심을 요약하여 **텍스트와 음성**으로 제공합니다. 또한 **핫 키워드**를 추출해 최근 트렌드를 한눈에 보여줍니다.

> 데모/학습 목적의 프로젝트입니다. 실제 서비스 배포 시 API 비용/정책 및 보안을 반드시 검토하세요.

---

## 🎯 문제 정의 & 목표
- **문제**  
  - 뉴스 양이 많아 **핵심 파악**이 어렵다.  
  - **손이 자유롭지 않은 상황**(운전, 운동)에서는 텍스트 검색이 불편하다.  
  - 단순 나열이 아니라 **트렌드 중심 요약**이 필요하다.
- **목표**  
  1) **음성으로 자연스럽게 검색** → STT 인식  
  2) **뉴스 수집 + 요약** → 핵심만 빠르게 파악  
  3) **핫 키워드 집계** → 트렌드 감지  
  4) **풀스택 설계/구현** → 포트폴리오로 설명 가능한 구조

---

## ✨ 주요 기능
- 🗣️ **STT**: 마이크로 말하면 텍스트로 변환 (OpenAI Whisper API)
- 🔎 **뉴스 수집**: 네이버 검색 API를 통해 관련 기사 수집
- 🧠 **요약**: 메인 키워드 + 보조 키워드(듀얼 쿼리)로 넓게 수집 후 요약
- 🔤 **핫 키워드**: 최근 기사에서 자주 등장하는 토큰 카운트
- 🔊 **TTS**: 요약 결과를 음성(MP3)으로 재생
- 🧩 **(옵션)** KoBERT + transformers: 타이틀 의미 유사도 기반 재정렬

---

## 🏗 아키텍처
```text
+-------------+        +------------------+        +-------------------+
|   Browser   | <----> |    FastAPI API   | <----> |  External APIs    |
|  React CRA  |        |  (uvicorn/httpx) |        |  (Naver, OpenAI)  |
+-------------+        +------------------+        +-------------------+
    |  STT/TTS (via API)         |  fetch articles/summary/keywords
    |  UI: Search, Mic, Grid     |  JSON responses (summary, list)
```

---

## 🧰 기술 스택
- **Frontend**: React (CRA), Web Speech API(`webkitSpeechRecognition`), Fetch API
- **Backend**: FastAPI, httpx, uvicorn, pydantic
- **AI**: OpenAI STT/TTS, (옵션) KoBERT + transformers + torch
- **Data**: Naver Search API
- **Etc.**: CORS, 환경변수(.env), 가드(타임아웃/타입체크)

---

## 📁 프로젝트 구조
```plaintext
news_project/
├─ backend/
│  ├─ main.py                 # API 라우팅: /api/*
│  ├─ voice_chat.py           # STT/TTS 보조 유틸
│  ├─ patched_cleanre.py      # 텍스트 전처리
│  └─ requirements.txt
└─ frontend/
   ├─ package.json
   ├─ public/index.html
   └─ src/
      ├─ App.js / App.css
      ├─ components/
      │  ├─ SearchBar.jsx
      │  ├─ MicButton.jsx
      │  ├─ HeadlineGrid.jsx
      │  ├─ ChatBox.jsx
      │  ├─ ListeningLoader.jsx
      │  └─ Loader.jsx
      └─ assets/
         ├─ hamster.png
         ├─ hamster2.png
         └─ hamster3.png
```

> 실제 폴더명/파일명은 저장소 기준으로 업데이트하세요.

---

## ⚡ 빠른 시작

### 0) 사전 준비
- Node.js 18+, Python 3.10+
- OpenAI API Key
- Naver Client ID / Secret

### 1) 백엔드 실행
```bash
cd backend
python -m venv .venv && source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# (옵션) KoBERT 사용 시
# CUDA 환경 예시:
# pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
pip install transformers

# .env 작성
cat > .env << 'EOF'
OPENAI_API_KEY=sk-...
NAVER_CLIENT_ID=your_client_id
NAVER_CLIENT_SECRET=your_client_secret
EOF

uvicorn main:app --reload  # http://localhost:8000
```

### 2) 프런트엔드 실행
```bash
cd frontend
npm install

# API 기본 경로는 http://localhost:8000/api
# 필요 시 .env 커스터마이즈:
echo "REACT_APP_API_URL=http://localhost:8000/api" > .env

npm start  # http://localhost:3000
```

---

## 🔐 환경 변수

### backend/.env
```ini
OPENAI_API_KEY=sk-...
NAVER_CLIENT_ID=your_client_id
NAVER_CLIENT_SECRET=your_client_secret
```

### frontend/.env
```ini
REACT_APP_API_URL=http://localhost:8000/api
```

---

## 🔌 API 명세

### 1) 헤드라인 빠른보기
**GET** `/api/headline_quick?query=삼성전자`  
**Response (요약):**
```json
[
  {"title": "기사 제목", "link": "https://...", "snippet": "요약"},
  {"title": "기사 제목2", "link": "https://...", "snippet": "요약2"}
]
```

### 2) 뉴스 트렌드 요약 (듀얼 쿼리)
**POST** `/api/news_trend`
```json
{
  "query": "인공지능",
  "assist": "규제 OR 투자",
  "days": 3,
  "top_k": 8
}
```
**Response (요약):**
```json
{
  "summary": "핵심 요약 본문 ...",
  "sources": [{"title": "...", "link": "..."}, {"title": "...", "link": "..."}]
}
```

### 3) 인기 키워드
**GET** `/api/popular_keywords?query=반도체&days=2`  
**Response:**
```json
{
  "keywords": [["삼성", 12], ["파운드리", 8], ["수요", 7]]
}
```

### 4) 음성 → 텍스트 (STT)
**POST** `/api/generate-stt`  
- Form field: `audio` (wav/mp3/m4a)

**Response:**
```json
{ "text": "인식된 문장..." }
```

### 5) 텍스트 → 음성 (TTS)
**POST** `/api/generate-tts`
```json
{ "text": "오늘의 주요 뉴스를 알려드릴게요." }
```
- Response: MP3 바이너리 스트림

> 모든 엔드포인트는 Content-Type, 타임아웃, CORS 등 기본 가드가 포함되어 있습니다.

---

## 🧩 프런트엔드 컴포넌트
- `SearchBar.jsx`: 텍스트/음성 검색 입력, onSubmit 핸들러
- `MicButton.jsx`: 마이크 토글 제어, 리스닝 상태 표시
- `HeadlineGrid.jsx`: 기사 목록 카드 렌더링
- `ChatBox.jsx`: 질의/응답 로그 스크롤 뷰
- `Loader.jsx`, `ListeningLoader.jsx`: 로딩/리스닝 인디케이터

---

## 🔄 동작 흐름
1. 사용자가 마이크 버튼을 눌러 **질문을 말함** → STT로 텍스트 변환  
2. 백엔드가 **네이버 뉴스 검색**으로 관련 기사를 수집  
3. (옵션) KoBERT 임베딩으로 **의미 유사도 재정렬**, 상위 N개 선택  
4. 선택된 기사들을 **요약 모델로 통합 요약** 생성  
5. 요약 텍스트를 **TTS로 음성 변환** → 브라우저에서 재생  
6. 동시에 **핫 키워드** 집계 결과를 프런트에 표시

---

## 🎥 데모
- 스크린샷: `/frontend/assets/`에 이미지 추가 후 아래에 삽입하세요.
```md
![홈 화면](./frontend/assets/demo_home.png)
![검색 결과](./frontend/assets/demo_results.png)
```
- 동영상: 유튜브/웨이브 링크를 README 상단 또는 여기 섹션에 배치 권장

---

## 🛠 트러블슈팅
- **401/403/429**: OpenAI/네이버 키/요금제 확인, 레이트리밋 점검  
- **CORS 오류**: `main.py`의 `allow_origins`에 프런트 주소 추가  
- **브라우저 STT 미동작**: `webkitSpeechRecognition`은 데스크톱 크롬 권장  
- **TLS/SSL 이슈**: 사내망/구형 사이트의 경우 낮은 보안 레벨 사이퍼가 필요할 수 있음  
- **KoBERT 미설치**: 자동 폴백으로 동작(문제 아님). 정교함 필요 시 `torch`, `transformers` 설치

---

## 🧠 학습 포인트
- FastAPI + httpx 비동기 설계로 **동시성 요청** 처리
- AI API 통합 및 **폴백 전략**(KoBERT 선택적 사용)
- 음성 UX 고려: **리스닝/로딩 상태** 시각화, 예외 처리
- 프런트/백 간 **계약 기반 개발**(명세 우선 → 구현)

---

## 🗺 로드맵
- [ ] 다국어 지원(영/일)  
- [ ] 서버 사이드 캐싱/레이트리밋  
- [ ] 벡터DB(예: Weaviate)로 의미 검색 고도화  
- [ ] 모바일 UI 최적화 & 접근성 개선  

---

## 📄 라이선스
MIT

---

## 🙌 크레딧
- FastAPI, React, httpx, BeautifulSoup4  
- OpenAI (STT/TTS), Naver Search API  
- (옵션) KoBERT (transformers, torch)
