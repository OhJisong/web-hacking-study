# web-hacking-study(웹해킹 스터디) 

> 개인 공부용 웹 해킹 레포지토리  
> 웹 기본 구조부터 XSS·SQLi 등 자주 나오는 취약점을 로컬에서 재현·분석·패치해 보고, 결과를 정리하는 곳입니다.  
> **참고:** 파일 맨 아래에 동일 내용의 [영어 번역본(English version)](#web-hacking-study)을 추가해 두었습니다.

---

## 목차
- [소개](#소개)  
- [학습 목표](#학습-목표)  
- [세션별 목표(요약)](#세션별-목표요약)  
- [로컬 실습 환경(간단)](#로컬-실습-환경간단)  
  - [A. Flask 실행 예시](#a-flask-실행-예시)  
  - [B. PHP 내장 서버 실행](#b-php-내장-서버-실행)  
  - [C. DB: SQLite 사용 권장](#c-db-sqlite-사용-권장)  
- [레포 구조](#레포-구조)  
- [빠른 시작(Quick start)](#빠른-시작quick-start)  
- [참고자료](#참고자료)  
- [라이선스·기여](#라이선스기여)

---

## 소개
제가 개인적으로 웹 보안을 공부하며 만든 저장소입니다.  
이론 정리, 실습 코드(로컬 격리), 재현 스크립트, 스크린샷, 보고서를 한데 모아 두었습니다.

> **중요:** 이 자료는 학습 목적으로만 사용하세요. 타인 소유 서비스에 무단으로 적용하지 마세요.

---

## 학습 목표
웹 애플리케이션의 동작 원리를 이해하고, 자주 발생하는 취약점(XSS, SQLi)을 직접 재현·분석·완화해 보는 것이 목표입니다.  
실습은 로컬 환경(Flask / PHP + SQLite)을 사용하고, 모든 결과를 문서로 남겨 포트폴리오로 만들 예정입니다.

**핵심 성과**
- HTTP 요청/응답, 쿠키·세션, 브라우저 렌더링 이해  
- 입력 처리 흐름에서 취약점이 발생하는 지점 파악  
- Reflected / Stored / DOM XSS 및 SQLi(Union, Boolean, Time-based 등) 재현과 방어 적용(이스케이프, Prepared Statement 등)  
- 간단한 취약 앱을 로컬에서 구성하고 재현 스크립트·패치 코드·보고서로 정리

---

## 세션별 목표(요약)
- **Session 1 — 기초 & XSS**
  - 웹 아키텍처·파라미터 처리 정리, Flask로 GET/POST 실습  
  - JavaScript/DOM 기본 이해, Reflected/Stored/DOM XSS 재현 및 방어 적용  
  - 산출물: 재현 코드 · 페이로드 목록 · 패치 전/후 스크린샷

- **Session 2 — 서버사이드 & SQL**
  - PHP 폼 처리·간단 CRUD 실습, SQL 구조 이해  
  - SQLi 유형 재현(Union / Boolean / Time-based 등) 및 영향 분석  
  - 산출물: 재현 스크립트 · 안전 쿼리 예제 · 영향 정리

- **Session 3 — 통합 실습 & 문서화**
  - 취약점 연계 시나리오 실습(예: XSS → 세션 탈취 → 권한 흐름)  
  - 브라우저 도구·로그로 탐지·분석, 최종 리포트 작성  
  - 산출물: 통합 리포트 (`docs/report_session3.md`), 체크리스트 등

**안전 규칙:** 모든 실습은 로컬(127.0.0.1) 또는 격리된 환경에서만 진행합니다.

---

## 로컬 실습 환경(간단)

### 권장(최소)
- Python 3.8+ (Flask)  
- PHP 7.0+  
- SQLite (초기 학습용)  
- git

**주의:** 실습용 앱을 절대 외부에 노출하지 마세요.

### A. Flask 실행 예시
1. 가상환경 생성·활성화
```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate
```

2. 패키지 설치
```bash
pip install --upgrade pip
pip install flask
```

3. 예제 실행
```bash
cd examples/session1/flask_xss_example
python app.py
# 보통 http://127.0.0.1:5000 에서 동작
```

---

## 4. 레포지토리 구조
```
.
├── README.md
├── examples/
│   ├── session1/
│   │   └── flask_xss_example/        # Flask 기반 XSS 실습 코드 (app.py 등)
│   ├── session2/
│   │   └── php_sqli_example/         # PHP + SQLite/간단 DB 예제
│   └── session3/
├── docs/                             # 실습 보고서, 스크린샷
└── scripts/                          # 필요 시 초기화 스크립트 (예: reset_db.sh)
```

---

### 5. 빠른 시작

1. Clone
```bash
git clone https://github.com/<your-username>/web-hacking-study.git
cd web-hacking-study
```

2. Flask 예제 실행
``bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install flask
cd examples/session1/flask_xss_example
python app.py
```

3. PHP 예제 실행
```bash
cd examples/session2/php_sqli_example
php -S 127.0.0.1:8000
```

---

### 6. 참고
- 생활코딩 — 웹 기본 강의 (기초 이해)
- MDN Web Docs — HTTP / DOM (공식 문서)
- PortSwigger Web Security Academy — 실습 중심(영문)

(한글 실습형 블로그는 Tistory/Velog에서 XSS, SQL Injection 태그로 검색하면 실전 예제가 많습니다.)

---

### 7. 라이선스 및 기여
학습·연구용 자료입니다. 재배포 시 출처 표기 바랍니다.
이슈나 PR은 환영합니다. (PR 전에는 로컬에서 충분히 테스트해 주세요.)

---
---

# web-hacking-study

> Personal study repository for web hacking.  
> The goal is to reproduce, analyze, and patch common web vulnerabilities (XSS, SQLi, etc.) locally — from basic web structure up to practical exploits and mitigations.  
> **Note:** The original Korean README is above; the English version (this text) is included here for convenience.

---

## Table of contents
- [Introduction](#introduction)  
- [Learning goals](#learning-goals)  
- [Session objectives (summary)](#session-objectives-summary)  
- [Local lab environment (quick)](#local-lab-environment-quick)  
  - [A. Example: running Flask](#a-example-running-flask)  
  - [B. Running PHP built-in server](#b-running-php-built-in-server)  
  - [C. DB: SQLite recommended](#c-db-sqlite-recommended)  
- [Repository structure](#repository-structure)  
- [Quick start](#quick-start)  
- [References](#references)  
- [License & contribution](#license--contribution)

---

## Introduction
This is a repository I created for studying web security personally.  
It collects theory notes, isolated practice code, reproduction scripts, screenshots, and reports.

> **Important:** Use these materials for learning purposes only. Do not apply them to services you do not own or have permission to test.

---

## Learning goals
The objective is to understand how web applications work and to reproduce, analyze, and mitigate frequently occurring vulnerabilities (XSS, SQLi).  
All exercises are done in local environments (Flask / PHP + SQLite), and results will be documented to serve as portfolio materials.

**Key outcomes**
- Understand HTTP request/response, cookies & sessions, and browser rendering.  
- Identify where vulnerabilities arise in input-processing flows.  
- Reproduce and defend against Reflected / Stored / DOM XSS and SQLi (Union, Boolean, Time-based, etc.) using escapes, prepared statements, and other mitigations.  
- Build simple vulnerable apps locally and document reproduction scripts, patch code, and reports.

---

## Session objectives (summary)
- **Session 1 — Basics & XSS**
  - Review web architecture and parameter handling; practice GET/POST with Flask.  
  - Basics of JavaScript/DOM; reproduce Reflected/Stored/DOM XSS and apply defenses.  
  - Deliverables: reproduction code · payload list · before/after patch screenshots

- **Session 2 — Server-side & SQL**
  - Practice PHP form handling and simple CRUD; understand SQL structure.  
  - Reproduce SQLi types (Union / Boolean / Time-based, etc.) and analyze impacts.  
  - Deliverables: reproduction scripts · safe query examples · impact summary

- **Session 3 — Integrated exercises & documentation**
  - Practice chained-vulnerability scenarios (e.g., XSS → session theft → privilege escalation).  
  - Use browser tools & logs for detection/analysis; write final report.  
  - Deliverables: integrated report (`docs/report_session3.md`), checklists, etc.

**Safety rule:** All exercises are performed only on local (127.0.0.1) or otherwise isolated environments.

---

## Local lab environment (quick)

### Minimum / recommended
- Python 3.8+ (Flask)  
- PHP 7.0+  
- SQLite (recommended for initial learning)  
- git

**Warning:** Never expose practice apps to the public internet.

### A. Example: running Flask
1. Create & activate a virtual environment
```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate

2. Install packages
```bash
pip install --upgrade pip
pip install flask
```

3. Run example
```bash
cd examples/session1/flask_xss_example
python app.py
# Typically accessible at http://127.0.0.1:5000
```

---

## Quick start

1. Clone
```bash
git clone https://github.com/<your-username>/web-hacking-study.git
cd web-hacking-study
```

2. Run Flask example
``bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install flask
cd examples/session1/flask_xss_example
python app.py
```

3.Run PHP example
```bash
cd examples/session2/php_sqli_example
php -S 127.0.0.1:8000
```

---

## References
- 생활코딩 — Web basics (good for fundamentals)
- MDN Web Docs — HTTP / DOM (official documentation)
- PortSwigger Web Security Academy — hands-on practice (English)

---

## License & contribution

This repository is intended for study and research use. Please credit the source if you redistribute.
Issues and PRs are welcome — please test thoroughly in your local environment before submitting a PR.
