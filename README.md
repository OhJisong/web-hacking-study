# web-hacking-study

> 개인 학습용 웹 해킹 스터디 레포지토리  
> 웹 기본 구조부터 XSS·SQLi 등 주요 취약점의 이론·재현·방어를 로컬 환경에서 간단히 실습하고 기록합니다.  
> **참고:** 이 파일의 맨 아래에 동일 내용의 **영문 번역본(English version)** 이 추가되어 있습니다.

---

## 목차
- [1. 소개](#1-소개)  
- [2. 학습 목표](#2-학습-목표)  
- [3. 세션별 목표(요약)](#3-세션별-목표요약)  
- [4. 로컬 실습 환경 (간단 설정)](#4-로컬-실습-환경-간단-설정)  
  - [A. Flask (간단 실행)](#a-flask-간단-실행)  
  - [B. PHP (내장 서버 실행)](#b-php-내장-서버-실행)  
  - [C. DB: SQLite 권장](#c-db-sqlite-권장)  
- [5. 레포지토리 구조](#5-레포지토리-구조)  
- [6. 빠른 시작 (Quick start)](#6-빠른-시작-quick-start)  
- [7. 참고자료](#7-참고자료)  
- [8. 라이선스 및 기여](#8-라이선스-및-기여)

---

## 1. 소개
이 레포지토리는 웹 보안(웹 해킹)을 체계적으로 학습하기 위해 만든 공간입니다.  
이론 정리, 실습 코드(로컬 격리 실행), 재현 스크립트, 결과 스크린샷 및 보고서를 한 곳에 모아 기록합니다.

**주의**: 본 자료는 학습 목적입니다. 실제 서비스나 타인 소유 시스템에 무단 적용하지 마세요.

---

## 2. 학습 목표

이 스터디의 목적은 웹 애플리케이션의 동작 원리를 이해하고 실제로 자주 발생하는 취약점(XSS, SQL Injection)을 직접 재현·분석·완화해 보는 것입니다. 
실습 중심으로 Flask / PHP + SQLite 환경을 사용하여 로컬에서 안전하게 실습하고 각 실습 내용을 문서화하여 포트폴리오 형태로 정리합니다. 
최종적으로는 모의 해킹(CTF) 준비 및 취약점 보고(기초 수준의 리서치/CVE 이해)까지 연결할 수 있는 기초 역량을 갖추는 것이 목표입니다.

### 주요 학습 성과(Expectations)
- 웹 기본 동작(HTTP 요청/응답, 쿠키·세션, 브라우저 렌더링) 이해  
- 입력 처리 흐름에서 취약점이 발생하는 지점 파악 능력 배양  
- Reflected / Stored / DOM XSS와 SQL Injection(Union, Boolean, Time-based 등) 각각을 재현하고, 방어 패턴(이스케이프, Prepared Statement 등)을 적용할 수 있음  
- 간단한 취약 애플리케이션을 로컬에서 구성하고(python Flask, PHP + SQLite), 취약점 재현 스크립트·패치 코드·보고서를 작성·관리할 수 있음  
- 실습 결과를 바탕으로 문제 재현 단계, 분석 결과, 패치 방법을 명확히 문서화(README/docs)할 수 있음

### 세션별 목표
- **Session 1 — 기초 및 XSS**
  - 웹 아키텍처와 HTTP 흐름 정리(클라이언트↔서버, 폼/쿼리 파라미터 처리)  
  - 간단한 Flask 앱으로 GET/POST 흐름 실습  
  - JavaScript/DOM 기초를 통해 브라우저에서 스크립트가 어떻게 실행되는지 이해  
  - Reflected / Stored / DOM XSS 사례 각각을 재현하고, 출력 이스케이프·CSP 등으로 방어 적용  
  - 산출물: 재현 코드, 공격 페이로드 목록, 패치 전/후 스크린샷

- **Session 2 — 서버사이드와 SQL**
  - PHP로 폼 처리와 DB 연동(간단한 CRUD) 실습  
  - SQL 기초(SELECT/INSERT/UPDATE) 복습 및 쿼리 구조 이해  
  - SQL Injection 유형(Union, Boolean, Time-based) 실전 재현과 영향 분석  
  - Prepared Statement(파라미터화 쿼리) 및 입력 검증으로 방어 적용  
  - 산출물: 재현 스크립트, 안전한 쿼리 예제, 취약점 영향 정리

- **Session 3 — 통합 실습 및 문서화**
  - 앞의 취약점들을 연계한 시나리오 실습(예: XSS → 세션 탈취 → 권한 상승 흐름 탐색)  
  - 브라우저 개발자 도구·로그를 이용한 탐지·분석 방법 실습  
  - 각 실습에 대한 최종 보고서(문제 정의 → 재현 절차 → 원인 분석 → 패치 코드 → 증빙 스크린샷) 작성  
  - 산출물: 통합 리포트(예: `docs/report_session3.md`), 체크리스트, 재현 자동화 스크립트

**안전 규칙**: 모든 실습은 반드시 로컬(127.0.0.1) 또는 격리된 환경에서만 진행합니다. 타인·실서비스 대상의 무단 테스트는 엄격히 금지합니다.

---

## 3. 로컬 실습 환경 (간단 설정)
### 권장(최소)
- Python 3.8+ (Flask 실습)  
- PHP 7.0+ (간단 PHP 실습)  
- SQLite (초기 학습용 — 별도 서버 설치 불필요)  
- git

### 주의
- 모든 실습은 **로컬(127.0.0.1)** 또는 격리된 환경에서만 실행하세요.  
- 실습용 취약 애플리케이션을 인터넷에 노출하면 안 됩니다.

### A. Flask (간단 실행)
1. 가상환경 생성 및 활성화
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

### 5. 빠른 시작작

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
학습/연구용 자료입니다. 재배포 시 출처를 표기해 주세요.
 질문이나 개선 제안은 Issue/PR 환영합니다.

---

