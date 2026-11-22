# AI_Audit_Project1 · 가온미래은행㈜ FY2025 기말감사 × AI 포트폴리오

은행 기말감사 실무(대출채권 기대신용손실, 금융회사 Risk Component, 분개 테스트, 이자수익, 특수관계자)를
데이터 전처리, 머신러닝, 딥러닝과 묶어 18일 동안 수행하는 포트폴리오 프로젝트입니다.

> **모든 회사, 인물, 거래, 수치는 실습용으로 만든 가상 자료입니다.** 실존 기관과 관계가 없습니다.

## 감사 대상 회사

| 항목 | 내용 |
|---|---|
| 회사 | 가온미래은행㈜ (가상 지방은행, 대전·충청 기반) |
| 회계연도 | 2025-01-01 ~ 2025-12-31 (비교: 2024) |
| 규모 | 총자산 약 2.9조원, 대출채권 약 2.1조원 (5,000건) |
| 핵심 감사 영역 | 대출채권 ECL(K-IFRS 1109): Stage 분류, PD·LGD·EAD, 미래전망정보, 개별평가, overlay |
| 기타 영역 | 분개 테스트, 이자수익 cut-off, 대손상각 승인, 특수관계자, 담보, PF 집중위험, 자본적정성 |

자세한 회사 정보는 [`data/00_company_profile.md`](data/00_company_profile.md)를 보세요.

## 폴더 구조

```
data/
├── 00_company_profile.md          회사 개요 · 자료 가정
├── 01_journal_entries.csv         전표 (약 1만 라인, 2025-01 ~ 2026-01-15)
├── 01a_je_users.csv               전표 사용자 · 승인한도 · 퇴사일
├── 01b_holidays.csv               공휴일
├── 01c_chart_of_accounts.csv      계정과목표
├── 02_trial_balance.csv           시산표 (전기·당기)
├── 03_financial_statements.xlsx   회사 작성 재무제표 + 주요 주석 (감사 전)
├── 04_board_minutes/              이사회·주총·리스크관리위·감사위 의사록 9건
├── 05_loan_master.csv             여신원장 + 회사 산출 Stage/PD/LGD/EAD/ECL
├── 05a_borrowers.csv              차주 정보
├── 05b_collateral_appraisals.csv  담보 감정 대장
├── 05c_property_price_index.csv   부동산 가격지수
├── 05d_credit_review_notes.csv    여신 심사의견 텍스트 (NLP용)
├── 05e_pf_projects.csv            PF 사업장
├── 06_loan_monthly_history.csv    월별 잔액·연체 이력 36개월 (시계열·딥러닝용)
├── 06a_pd_training_2024.csv       PD 모형 학습 데이터 (전처리 실습용으로 일부러 지저분함)
├── 07_borrower_financials.csv     기업 차주 재무
├── 08_ecl_parameters.xlsx         회사가 적용한 ECL 파라미터
├── 08a_individual_assessment.csv  개별평가 내역
├── 09_macro_scenarios.csv         미래전망 시나리오 · 가중치
├── 09a_macro_history.csv          거시변수 · 부도율 이력
├── 10_interest_income_ledger.csv  이자수익원장 (은행의 매출원장)
├── 11_writeoff_recovery.csv       대손상각 · 회수 이력 (LGD 추정용)
├── 12_risk_reports.xlsx           BIS·LCR·NPL·집중도 리스크 보고서
├── 13_loan_confirmations.csv      대출 잔액 외부조회 결과
├── 14_related_parties.md          특수관계자 목록
├── 15_ecl_policy.md               ECL 회계정책 · 내규
└── 16_ecl_provision_report.md     회사 충당금 산정 보고서
docs/
└── data_dictionary.md             파일별 컬럼 설명 · 연결 키
```

컬럼 정의와 파일 간 연결 키는 [`docs/data_dictionary.md`](docs/data_dictionary.md)에 있습니다.

## Colab에서 불러오기

```python
!git clone https://github.com/matcha430/AI_Audit_Project1.git
import pandas as pd
BASE = "AI_Audit_Project1/data/"

je    = pd.read_csv(BASE + "01_journal_entries.csv", dtype={"account_code": str})
loans = pd.read_csv(BASE + "05_loan_master.csv", parse_dates=["origination_date", "maturity_date", "appraisal_date"])
fs    = pd.read_excel(BASE + "03_financial_statements.xlsx", sheet_name=None)  # 시트별 dict
```

## 감사인으로서 알아둘 점

- 이 자료는 **회사가 감사인에게 제출한 자료**라는 설정입니다. 회사 보고서와 원장, 의사록, 정책서가 서로 항상 맞는다고 가정하지 마세요.
- 오류, 통제 미비, 부정 징후가 여러 곳에 일부러 심어져 있습니다. 어디에 있는지는 일차별 과제를 하면서 찾게 됩니다.
- 금액 단위는 원입니다. 재무제표만 백만원 단위입니다.

## 18일 일정

일차별 과제와 평가 기준은 별도 계획표로 제공됩니다.
