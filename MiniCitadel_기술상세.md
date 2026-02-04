# Mini Citadel - 기술 상세 문서

**프로젝트명:** Mini Citadel - Trader Decision Support System (TDSS)  
**역할:** Financial Systems Architect  
**기간:** 1994년 - 현재 (시스템 트레이딩 경험), 2020년 - 현재 (Mini Citadel 개발)  
**Repository:** https://github.com/JuneBay/Mini-Citadel.git  
**로컬 경로:** `c:\JoonBae_Works\Mini-Citadel\`, `Mini-Citadel-v2\`, `Mini-Citadel-v3\`

---

## 📋 프로젝트 개요

1990년대 중반부터 시스템 트레이딩 로직을 설계·개발해 왔으며, 국내 트레이딩 툴과 증권사 API를 활용해 시장 데이터와 보조 지표를 결합한 매매 연구를 이어왔습니다.

**Mini Citadel**은 다중 API 신호를 통합 제어하고 시장 데이터를 고빈도 대시보드로 집계하는 **중앙 집중형 트레이더 의사결정 지원 시스템(TDSS)**입니다. 여러 트레이더 워크스테이션에서 트레이딩 신호를 조율하는 마스터 게이트웨이 역할을 하며, 실시간 API 상태 모니터링, 자동화된 데이터 아카이빙, 커스터마이징 가능한 통합 시각화 인터페이스를 제공합니다.

**핵심 가치:**
- **TDSS (Trader Decision Support System):** 중앙 집중형 트레이더 의사결정 지원
- **Centralized Gateway:** 다중 머신 신호 통합 제어 및 API 상태 모니터링
- **Data Archiving:** 알고리즘 트레이딩을 위한 자동화된 히스토리컬 데이터 수집
- **성능 최적화:** 데이터 구조 재설계로 조회 성능 90% 향상 (50ms → <5ms)
- **Multi-Account Management:** 확장 가능한 다계좌 관리 아키텍처
- **도메인 전문성:** 25년 이상의 금융 시장 경험

---

## 🎯 핵심 성과 및 지표

### 1. 성능 최적화
- **조회 성능 개선:** 50ms → <5ms (90% 향상)
- **데이터 구조:** DataFrame (O(n)) → Dictionary (O(1))
- **메모리 효율:** 직접 메모리 참조로 HTTP 통신 제거

### 2. UI 반응성 개선
- **UI 지연시간 감소:** 500ms → 100ms (80% 감소)
- **업데이트 주기:** 500ms 폴링 → 100ms 폴링
- **내부 통신:** HTTP API 호출 → 직접 메모리 참조

### 3. 중앙 집중형 아키텍처
- **Centralized Gateway:** 다중 트레이더 머신에서 신호 통합 제어
- **API Health Monitoring:** 모든 연결된 API의 실시간 상태 모니터링
- **Multi-Machine Orchestration:** 분산 워크스테이션 간 원활한 조율

### 4. 데이터 관리 및 인텔리전스
- **Automated Data Archiving:** 히스토리컬 분석을 위한 스케줄링된 데이터 파이프라인
- **Multi-Account Support:** 확장 가능한 다계좌 관리 설계
- **Unified Visualization:** 이질적인 API 스트림을 단일 대시보드로 통합

### 5. 시스템 안정성
- **v1.0 UI/UX 호환성 유지**
- **내부 엔진 재설계로 성능 향상**
- **WebSocket 기반 실시간 데이터 처리**
- **Thread-Safe 동시 액세스:** 100% 스레드 안전 작업

---

## 🏗️ 시스템 아키텍처

### 전체 시스템 구조 (v1.2)

```
[키움증권 API]
   ↓ (WebSocket)
[데이터 수집 엔진]
   ├─ TR (Transaction) 데이터
   ├─ RT (Real-Time) 데이터
   └─ 보조 지표 생성
   ↓ (직접 메모리 참조)
[데이터 저장소 (Dictionary/Hashmap)]
   ├─ 종목 코드 → 시세 데이터
   ├─ 포트폴리오 → 보유 종목
   └─ 계좌 정보 → 잔고/수익률
   ↓ (100ms 폴링)
[PySide6 (Qt) UI]
   ├─ 실시간 시세 표시
   ├─ 포트폴리오 현황
   └─ 수익률/리스크 분석
```

### v1.0 → v1.2 아키텍처 변경

#### v1.0 (Before)
```
[키움증권 API] → [pandas DataFrame] → [HTTP API] → [UI]
- 조회: O(n) 선형 검색
- 통신: HTTP API 호출 (500ms 지연)
- 업데이트: 500ms 폴링
```

#### v1.2 (After)
```
[키움증권 API] → [Dictionary (Hashmap)] → [직접 메모리 참조] → [UI]
- 조회: O(1) 해시 조회
- 통신: 직접 메모리 참조 (<100ms)
- 업데이트: 100ms 폴링
```

### 주요 기술적 구현

#### 1. 해시맵 기반 데이터 구조
- **종목 코드를 키로 하는 Dictionary 구조**
- **O(1) 조회 성능**
- **메모리 효율적인 데이터 저장**

#### 2. 실시간 데이터 처리
- **WebSocket 기반 실시간 시세 수신**
- **asyncio 비동기 처리**
- **이벤트 드리븐 업데이트 (0.1초 폴링)**

#### 3. 직접 메모리 참조
- **HTTP API 제거**
- **엔진과 UI 직접 연결**
- **지연시간 80% 감소**

#### 4. Excel 연동
- **자동 Excel 파일 감지**
- **포트폴리오 데이터 Import/Export**
- **pandas 기반 데이터 처리**

#### 5. 정밀한 계산
- **슬리피지 계산:** 실제 체결가와 목표가 차이
- **수익률 계산:** 실시간 포트폴리오 수익률
- **리스크 관리:** 포트폴리오 리스크 지표

---

## 💻 기술 스택 (Tech Stack)

### Backend (Python)
| 라이브러리 | 용도 |
|-----------|------|
| **Python 3.x** | 전체 시스템 개발 |
| **asyncio** | 비동기 처리 |
| **websockets** | WebSocket 통신 |
| **pandas** | 데이터 처리 |
| **openpyxl** | Excel 파일 처리 |

### GUI (PySide6)
| 컴포넌트 | 용도 |
|---------|------|
| **PySide6 (Qt)** | GUI 프레임워크 |
| **QTableWidget** | 데이터 테이블 표시 |
| **QTimer** | 주기적 업데이트 (100ms) |
| **QThread** | 백그라운드 작업 |

### APIs & Services
| API | 제공자 | 용도 |
|-----|--------|------|
| **키움증권 API** | 키움증권 | 실시간 시세, 계좌 정보 |

### 데이터 저장
| 형식 | 용도 |
|-----|------|
| **Dictionary (Hashmap)** | 실시간 데이터 저장 |
| **Excel (XLSX)** | 포트폴리오 백업/복원 |

### Development Tools
- **Git:** 버전 관리
- **GitHub:** 코드 저장소
- **PyCharm:** 개발 환경

---

## 🔧 해결한 주요 기술적 문제

### 1. 조회 성능 병목
**문제:** pandas DataFrame 선형 검색으로 1,000개 종목 조회 시 500ms 소요  
**해결:** 종목 코드를 키로 하는 Dictionary(Hashmap) 구조로 변경  
**결과:** 조회 시간 50ms로 단축 (90% 향상)

### 2. UI 지연시간
**문제:** HTTP API 호출로 UI 업데이트 시 500ms 지연 발생  
**해결:** 엔진과 UI를 직접 메모리 참조로 연결  
**결과:** UI 지연시간 100ms로 감소 (80% 개선)

### 3. 실시간 데이터 처리
**문제:** 동기 방식으로 실시간 데이터 처리 시 UI 블로킹 발생  
**해결:** asyncio 기반 비동기 처리 + WebSocket 연동  
**결과:** UI 블로킹 없이 실시간 데이터 처리 가능

### 4. Excel 파일 자동 감지
**문제:** 사용자가 Excel 파일 경로를 수동으로 입력해야 함  
**해결:** 자동 Excel 파일 감지 로직 구현  
**결과:** 사용자 편의성 향상, 오류 감소

### 5. v1.0 호환성 유지
**문제:** 성능 개선 시 기존 UI/UX 변경으로 사용자 혼란  
**해결:** 내부 엔진만 재설계하고 UI/UX는 v1.0과 동일하게 유지  
**결과:** 사용자 학습 비용 0, 성능 향상만 체감

---

## 📊 성과 비교표

| 항목 | v1.0 (Before) | v1.2 (After) | 개선율 |
|-----|--------------|-------------|--------|
| **조회 성능** | 500ms (O(n)) | 50ms (O(1)) | **90% 향상** |
| **UI 지연시간** | 500ms (HTTP) | 100ms (메모리) | **80% 감소** |
| **업데이트 주기** | 500ms 폴링 | 100ms 폴링 | **5배 빠름** |
| **메모리 사용** | DataFrame (높음) | Dictionary (낮음) | **메모리 효율** |
| **UI/UX** | v1.0 | v1.0 호환 | **학습 비용 0** |

---

## 🚀 비즈니스 활용 용도 (Use Cases)

### 1. 개인 투자자
- **실시간 포트폴리오 관리:** 보유 종목 실시간 추적
- **수익률 분석:** 실시간 수익률 계산
- **리스크 관리:** 포트폴리오 리스크 지표 확인

### 2. 시스템 트레이딩
- **자동 매매:** 보조 지표 기반 자동 매매
- **백테스팅:** 과거 데이터 기반 전략 검증
- **실시간 모니터링:** 매매 전략 실시간 추적

### 3. 금융 데이터 분석
- **시장 데이터 수집:** 실시간 시세 데이터 수집
- **보조 지표 생성:** 이동평균, RSI, MACD 등
- **데이터 시각화:** 차트 및 그래프 생성

---

## 💡 Resume/이력서용 핵심 포인트

### 영문 (English)
- Developed real-time portfolio management system (trading system) with WebSocket integration for live market data from Kiwoom API
- Redesigned data structure from DataFrame to hashmap-based dictionary, improving lookup performance (50ms → 5ms, documented in v1.2 report)
- Reduced UI response latency by replacing HTTP API calls with direct memory reference architecture (500ms → 100ms)
- Implemented automatic Excel file detection, precise slippage calculation, and portfolio risk management features
- Maintained v1.0 UI/UX compatibility while achieving performance improvement through internal engine redesign

### 한글 (Korean)
- 키움증권 API WebSocket 연동 실시간 포트폴리오 관리 시스템 구축
- 데이터 구조 재설계로 조회 성능 개선 (50ms → 5ms, v1.2 보고서 문서화)
- HTTP API 호출을 직접 메모리 참조 아키텍처로 교체하여 UI 반응 지연시간 감소 (500ms → 100ms)
- 자동 Excel 파일 감지, 정밀 슬리피지 계산, 포트폴리오 리스크 관리 기능 구현
- 내부 엔진 재설계를 통한 성능 향상과 동시에 v1.0 UI/UX 호환성 유지

---

## 📈 시스템 트레이딩 경험 (1994년 - 현재)

### 전문성
- **국내 시스템 트레이딩 환경에서 로직 설계 경험 축적**
- **Cybos, Yes Trader, Signal Maker 등 국내 트레이딩 툴 경험**
- **25년 이상의 금융 시장 경험**

### 도메인 감각
- **금융 시장 흐름을 빠르게 파악**
- **금융 정보 제공 영역에 특히 강점**

### 데이터/시스템
- **증권사 API 기반 TR/RT 데이터 수집**
- **보조 지표 통합 (이동평균, RSI, MACD 등)**

### 아키텍처/분석
- **API 게이트웨이 계층화**
- **지표 생성·시각화**
- **SQL(MySQL/MariaDB) 저장·조회**

---

## 🔐 보안 및 비공개 정보

금융 보안상 세부 전략은 비공개이나, 차트 외 데이터까지 통합하는 구조를 설계·검증하는 것이 핵심입니다.

---

## 📁 관련 문서

- **GitHub Repository:** https://github.com/JuneBay/Mini-Citadel.git
- **로컬 경로:** 
  - `c:\JoonBae_Works\Mini-Citadel\` (v1)
  - `c:\JoonBae_Works\Mini-Citadel-v2\` (v2)
  - `c:\JoonBae_Works\Mini-Citadel-v3\` (v3)
- **성과 비교표:** 본 문서 "성과 비교표" 섹션 참조

---

**작성일:** 2026-01-26  
**최종 수정:** 2026-01-26
