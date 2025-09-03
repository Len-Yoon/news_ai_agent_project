# 📡 News Voice Assistant (Python FastAPI + React)

> **음성 기반 뉴스 검색 & 요약 시스템**  
> 사용자가 말로 뉴스를 요청하면 → 관련 기사 검색 → AI 요약 → 음성으로 읽어주는 **풀스택 프로젝트**

<p align="left">
  <img alt="Python" src="https://img.shields.io/badge/Backend-Python%20(FastAPI)-3776AB?logo=python&logoColor=white" />
  <img alt="FastAPI" src="https://img.shields.io/badge/Framework-FastAPI-009688?logo=fastapi&logoColor=white" />
  <img alt="React" src="https://img.shields.io/badge/Frontend-React-61DAFB?logo=react&logoColor=white" />
  <img alt="OpenAI" src="https://img.shields.io/badge/AI-OpenAI-black?logo=openai" />
  <img alt="License" src="https://img.shields.io/badge/License-MIT-green" />
</p>

---

## 🎯 프로젝트 개요
현대인은 하루에도 수많은 뉴스를 접합니다.  
이 프로젝트는 **음성 인터페이스**와 **AI 요약**을 결합해, 사용자가 말로 뉴스를 요청하면 관련 기사를 모으고 핵심을 요약하여 **텍스트와 음성**으로 제공합니다.

---

## ✨ 주요 기능
- 🗣️ **STT**: 사용자의 음성을 텍스트로 변환 (OpenAI Whisper API)  
- 🔎 **뉴스 검색**: 네이버 뉴스 API 연동  
- 🧠 **요약 생성**: AI 기반 듀얼 쿼리 요약  
- 🔊 **TTS**: 요약 결과를 음성(MP3)으로 변환 (OpenAI TTS)  
- 🔤 **핫 키워드 추출**: 최근 기사에서 많이 등장한 키워드 시각화  

---

## 🏗 아키텍처
```text
React (브라우저)
   │  음성 입력(STT) / 결과 표시
   ▼
Uvicorn (ASGI 서버)
   ▼
FastAPI (Python 백엔드)
   ├─ 네이버 뉴스 검색 API 호출
   ├─ OpenAI STT/TTS 호출
   └─ 요약/키워드 처리
```

---

## 🧰 기술 스택
- **Language**: Python 3.10+, JavaScript (ES6)  
- **Backend**: FastAPI, Uvicorn, httpx, pydantic  
- **Frontend**: React (CRA), Web Speech API  
- **AI**: OpenAI Whisper (STT), OpenAI TTS, (옵션) KoBERT + transformers  
- **Data**: Naver News API  

---

## 📂 프로젝트 구조
```plaintext
news_project/
├─ backend/
│  ├─ main.py
│  ├─ voice_chat.py
│  └─ requirements.txt
└─ frontend/
   ├─ public/index.html
   └─ src/
      ├─ App.js / App.css
      ├─ components/
      │  ├─ SearchBar.jsx
      │  ├─ MicButton.jsx
      │  ├─ HeadlineGrid.jsx
      │  └─ ChatBox.jsx
```

---

## ⚡ 실행 방법

### 백엔드
```bash
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# 환경 변수 (.env)
OPENAI_API_KEY=sk-...
NAVER_CLIENT_ID=your_client_id
NAVER_CLIENT_SECRET=your_client_secret

uvicorn main:app --reload
```

### 프런트엔드
```bash
cd frontend
npm install
npm start   # http://localhost:3000
```

---

## 🔄 동작 흐름
1. 사용자가 **마이크 버튼**을 눌러 발화  
2. 음성이 **텍스트** (**STT**)로 변환  
3. 백엔드가 **뉴스 검색 + 요약** 처리  
4. 결과를 **텍스트 + 음성** (**TTS**)으로 반환  
5. 브라우저에서 **기사·요약·핫 키워드 표시**  

---

## 🧠 학습 포인트
- FastAPI 기반 **비동기 API 설계** 경험  
- 외부 API(OpenAI, Naver) **통합 및 예외 처리**  
- 음성 UX (리스닝 → 요약 → 발화) 플로우 설계  
- **프론트–백–AI**를 연결한 풀스택 경험  

---

## 🙌 크레딧
- **FastAPI, Uvicorn** (백엔드)  
- **React, Web Speech API** (프런트엔드)  
- **OpenAI Whisper/TTS** (음성 인식 & 합성)  
- **Naver News API** (뉴스 데이터)  
