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

## 📚 목차
- [프로젝트 개요](#-프로젝트-개요)
- [문제 정의 & 목표](#-문제-정의--목표)
- [주요 기능](#-주요-기능)
- [아키텍처](#-아키텍처)
- [기술 스택](#-기술-스택)
- [프로젝트 구조](#-프로젝트-구조)
- [빠른 시작](#-빠른-시작)
- [환경 변수](#-환경-변수)
- [API 명세](#-api-명세)
- [프런트엔드 컴포넌트](#-프런트엔드-컴포넌트)
- [동작 흐름](#-동작-흐름)
- [데모](#-데모)
- [트러블슈팅](#-트러블슈팅)
- [학습 포인트](#-학습-포인트)
- [로드맵](#-로드맵)
- [라이선스](#-라이선스)
- [크레딧](#-크레딧)

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
