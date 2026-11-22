# 데이터 사전 (Data Dictionary)

가온미래은행㈜ FY2025 기말감사 실습 자료입니다. 모든 CSV는 UTF-8(BOM)이고 금액 단위는 원입니다. 재무제표만 백만원 단위입니다.
Colab에서는 `pd.read_csv(path)`로 바로 읽을 수 있습니다.

## 파일 목록

| 파일 | 행 수 | 설명 |
|---|---|---|
| `00_company_profile.md` |  | 가상 은행 기본정보, 조직, 주요 재무, 2025년 주요 사항, 자료 가정 |
| `01_journal_entries.csv` | 10,060 | 총계정원장 전표(라인 단위). 2025-01-01 ~ 2026-01-15 입력분. 결산 기준은 effective_date ≤ 2025-12-31 |
| `01a_je_users.csv` | 16 | 전표 사용자 마스터: 부서, 직위, 승인한도, 입사·퇴사일 |
| `01b_holidays.csv` | 18 | 2025년 공휴일(주말 제외) |
| `01c_chart_of_accounts.csv` | 35 | 계정과목표와 재무제표 표시 항목 매핑 |
| `02_trial_balance.csv` | 35 | 계정별 시산표: 전기말 잔액, 당기 차변·대변 합계, 당기말 잔액 (차변 +, 대변 −) |
| `03_financial_statements.xlsx` |  | 회사 작성 재무제표(감사 전, 백만원): 재무상태표, 포괄손익계산서, 충당금 변동·Stage별 대출·특수관계자 주석 |
| `04_board_minutes/` |  | 이사회·주주총회·리스크관리위원회·감사위원회 의사록(2025년) |
| `05_loan_master.csv` | 5,000 | 여신원장(2025-12-31, 5,000건). 차주·상품·담보·등급·연체·승인정보와 회사가 산출한 Stage, PD, LGD, EAD, ECL |
| `05a_borrowers.csv` | 4,593 | 차주 기본정보(개인: 소득·신용점수·직업, 기업: 업종·업력·매출) |
| `05b_collateral_appraisals.csv` | 2,413 | 담보 감정 대장(감정일, 감정가, 감정기관, 선순위) |
| `05c_property_price_index.csv` | 440 | 담보유형·권역별 부동산 가격지수(2015=100, 2015~2025) |
| `05d_credit_review_notes.csv` | 1,300 | 기업여신 심사·사후관리 의견(한국어 텍스트, 2025년 4분기) |
| `05e_pf_projects.csv` | 40 | 부동산 PF 사업장 정보(분양률, 공정률, 시공사, 만기연장 횟수) |
| `06_loan_monthly_history.csv` | 83,785 | 현재 대출의 월말 잔액·연체일수 이력(2023-01 ~ 2025-12, 최대 36개월). 시계열·딥러닝 실습용 |
| `06a_pd_training_2024.csv` | 8,040 | PD 모형 학습용 데이터: 2024-12-31 스냅샷 특성과 이후 12개월 부도 여부(default_12m). 결측·표기 불일치·단위 오류·중복이 일부러 섞여 있음 |
| `07_borrower_financials.csv` | 2,310 | 기업차주 재무정보(2023·2024 결산) |
| `08_ecl_parameters.xlsx` |  | 회사가 ECL 산출에 실제 적용한 파라미터: PD, LGD, CCF, 시나리오 가중치, 기타 가정 |
| `08a_individual_assessment.csv` | 10 | Stage 3 기업여신 개별평가 산출 내역(회사 작성) |
| `09_macro_scenarios.csv` | 9 | 2026~2028 미래전망 시나리오(기본·낙관·비관)와 가중치(위원회 의결값, ECL 계산 적용값) |
| `09a_macro_history.csv` | 64 | 분기별 거시경제 변수와 부도율 이력(2010Q1 ~ 2025Q4). 미래전망 회귀 실습용 |
| `10_interest_income_ledger.csv` | 46,288 | 이자수익원장(대출별 월별 경과이자·수취·미수, 2025년). 은행의 매출원장 역할 |
| `11_writeoff_recovery.csv` | 814 | 대손상각 및 상각 후 회수 이력(2019~2025). 2025년분은 이사회 승인 근거 포함. 회수율·LGD 추정용 |
| `12_risk_reports.xlsx` |  | 리스크 보고서(회사 보고): 월별 BIS·LCR·고정이하여신비율·연체율, 업종 집중도, 상위 20 차주, 자산건전성 분류, BIS 산출 요약 |
| `13_loan_confirmations.csv` | 49 | 기업여신 잔액 외부조회 발송·회신 결과 |
| `14_related_parties.md` |  | 회사가 제출한 특수관계자 목록과 거래 내역, 관련 규정 요약 |
| `15_ecl_policy.md` |  | ECL 회계정책 및 관련 내규(Stage 기준, PD·LGD·EAD, overlay, 이자수익, 상각, 전결권, 대손준비금) |
| `16_ecl_provision_report.md` |  | 회사의 2025년 말 충당금 산정 결과 보고서 |

## 컬럼 설명

### `01_journal_entries.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `je_no` | str | 전표번호 | JE000001 |
| `line_no` | int64 | 전표 내 라인번호 | 1 |
| `posting_date` | str | 입력(전기)일자 | 2025-01-02 |
| `posting_time` | str | 입력 시각 | 11:02:09 |
| `effective_date` | str | 회계 귀속일자(기준일) | 2025-01-02 |
| `account_code` | int64 | 계정코드 | 5420 |
| `account_name` | str | 계정명 | 일반관리비-기타 |
| `debit` | int64 | 차변 금액(원) | 289000 |
| `credit` | int64 | 대변 금액(원) | 0 |
| `description` | str | 적요 | 전산유지보수료 - 한결통신㈜ |
| `source` | str | AUTO=계정계 일괄, MANUAL=수동 입력 | MANUAL |
| `created_by` | str | 작성자 ID | U012 |
| `approved_by` | str | 승인자 ID(AUTO는 공란) | U013 |
| `department` | str | 작성 부서 | 총무부 |
| `reference` | str | 참조(세그먼트, 대출번호 등) | LARGE |

### `01a_je_users.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `user_id` | str | 사용자 ID | SYSTEM |
| `user_name` | str | 성명 | 코어뱅킹 배치 |
| `department` | str | 작성 부서 | IT운영부 |
| `position` | str | 직위 | 시스템 |
| `approval_limit` | float64 | 해당 전결권자 한도(원) | 0.0 |
| `hire_date` | str | 입사일 | 2019-03-04 |
| `termination_date` | str | 퇴사일(공란=재직) | 2025-08-31 |

### `01b_holidays.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `date` | str | 일자 | 2025-01-01 |
| `holiday_name` | str | 공휴일명 | 신정 |

### `01c_chart_of_accounts.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `account_code` | int64 | 계정코드 | 1010 |
| `account_name` | str | 계정명 | 현금및예치금 |
| `account_class` | str | 계정 분류 | 자산 |
| `fs_line` | str | 재무제표 표시 항목 | 현금및예치금 |

### `02_trial_balance.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `account_code` | int64 | 계정코드 | 1010 |
| `account_name` | str | 계정명 | 현금및예치금 |
| `account_class` | str | 계정 분류 | 자산 |
| `fs_line` | str | 재무제표 표시 항목 | 현금및예치금 |
| `balance_2024_12_31` | float64 | 전기말 잔액(차변+ / 대변−) | 160000000000.0 |
| `debit_2025` | float64 | 당기 차변 합계 | 6331386270863.0 |
| `credit_2025` | float64 | 당기 대변 합계 | 6321386270870.0 |
| `balance_2025_12_31` | float64 | 당기말 잔액(차변+ / 대변−) | 169999999993.0 |
| `note` | str | 비고 | 결산 시스템 자동대사(근거 전표 없음) |

### `05_loan_master.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `loan_id` | str | 대출번호 | L000001 |
| `borrower_id` | str | 차주번호 | B000001 |
| `segment` | str | 세그먼트 코드(RET_MORT 주택담보, RET_JEONSE 전세, RET_CREDIT 신용, RET_OD 마이너스통장, SOHO 개인사업자, SME 중소기업, LARGE 대기업·중견, PF 부동산PF) | RET_MORT |
| `biz` | str | 가계/기업 | 가계 |
| `product` | str | 상품명 | 주택담보대출 |
| `region` | str | 권역 | 서울 |
| `industry` | str | 업종(KSIC 대분류, 가계는 공란) | 운수및창고업 |
| `origination_date` | str | 최초 실행일 | 2025-09-29 |
| `maturity_date` | str | 만기일 | 2055-09-29 |
| `remaining_years` | float64 | 잔존만기(년) | 29.74 |
| `repayment_type` | str | 상환방식 | 원리금균등 |
| `approved_amount` | float64 | 약정(승인)금액 | 292000000.0 |
| `approval_level` | str | 실제 승인 전결권자 | 지점장 |
| `approval_limit` | float64 | 해당 전결권자 한도(원) | 500000000.0 |
| `balance` | float64 | 기말 대출잔액(원) | 290734000.0 |
| `undrawn_limit` | float64 | 미사용 한도(원) | 0.0 |
| `interest_rate` | float64 | 약정금리(연) | 0.04 |
| `collateral_type` | str | 담보유형 | 아파트 |
| `appraisal_value` | float64 | 최근 감정가(원) | 630000000.0 |
| `appraisal_date` | str | 최근 감정일 | 2025-09-22 |
| `senior_lien` | float64 | 선순위 채권액(원) | 0.0 |
| `guarantee_ratio` | float64 | 보증 비율 | 0.0 |
| `guarantor` | str | 보증기관 | 주택도시보증공사 |
| `dsr` | float64 | 총부채원리금상환비율(가계) | 0.402 |
| `internal_grade_orig` | int64 | 최초 실행 시 내부등급(1 우량 ~ 9, 10 부도) | 5 |
| `internal_grade_now` | int64 | 현재 내부등급 | 7 |
| `dpd` | int64 | 기말 연체일수 | 0 |
| `watchlist` | str | 조기경보 관리대상 여부 | N |
| `restructured_flag` | str | 여신시스템 조건변경 플래그 | N |
| `bankruptcy_flag` | str | 회생·파산 신청 여부 | N |
| `related_party` | str | 특수관계자 여부 | N |
| `fss_class` | str | 감독규정상 자산건전성 분류(회사) | 정상 |
| `company_stage` | int64 | 회사 산정 Stage | 1 |
| `pd_12m_base` | float64 | 등급별 12개월 PD(기본) | 0.032 |
| `pd_12m_weighted` | float64 | 시나리오 가중 12개월 PD(회사 가중치) | 0.03144 |
| `pd_lifetime_weighted` | float64 | 시나리오 가중 전체기간 PD | 0.60952 |
| `pd_applied` | float64 | ECL 계산에 적용한 PD | 0.03144 |
| `lgd` | float64 | 회사 적용 LGD | 0.1 |
| `ccf` | float64 | 회사 적용 CCF | 0.0 |
| `ead` | float64 | 회사 산정 EAD(원) | 290734000.0 |
| `ecl_company` | float64 | 회사 산정 ECL 합계(원) | 914068.0 |
| `ecl_onbalance_company` | float64 | 그중 대출채권분(대손충당금) | 914068.0 |
| `ecl_undrawn_company` | float64 | 그중 미사용약정분(충당부채) | 0.0 |
| `ltv_at_appraisal` | float64 | 잔액/최근 감정가 | 0.4615 |

### `05a_borrowers.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `borrower_id` | str | 차주번호 | B000001 |
| `segment` | str | 세그먼트 코드(RET_MORT 주택담보, RET_JEONSE 전세, RET_CREDIT 신용, RET_OD 마이너스통장, SOHO 개인사업자, SME 중소기업, LARGE 대기업·중견, PF 부동산PF) | RET_MORT |
| `region` | str | 권역 | 서울 |
| `borrower_type` | str | 차주 유형 | 개인 |
| `borrower_name` | str | 차주명(개인은 마스킹) | 한*태 |
| `age` | float64 | 연령 | 59.0 |
| `annual_income_10k` | float64 | 연소득(만원) | 3999.0 |
| `credit_score` | float64 | 개인신용점수(300~1000) | 740.0 |
| `job_type` | str | 직업군 | 공무원 |
| `employment_years` | float64 | 근속연수 | 7.9 |
| `industry` | str | 업종(KSIC 대분류, 가계는 공란) | 운수및창고업 |
| `years_in_business` | float64 | 업력(년) | 16.0 |
| `annual_revenue` | float64 | 연매출(원) | 3110000000.0 |
| `external_audit` | str | 외부감사 대상 여부 | N |
| `internal_grade_now` | int64 | 현재 내부등급 | 7 |
| `related_party` | str | 특수관계자 여부 | N |

### `05b_collateral_appraisals.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `collateral_id` | str | 담보번호 | COL000001 |
| `loan_id` | str | 대출번호 | L000001 |
| `collateral_type` | str | 담보유형 | 아파트 |
| `region` | str | 권역 | 서울 |
| `appraisal_date` | str | 최근 감정일 | 2025-09-22 |
| `appraisal_value` | float64 | 최근 감정가(원) | 630000000.0 |
| `senior_lien` | float64 | 선순위 채권액(원) | 0.0 |
| `appraiser` | str | 감정기관 | 한결감정평가법인 |
| `appraisal_method` | str | 평가방법 | 시세(KB·한국부동산원) |
| `note` | str | 비고 | 2025.11 비공식 탁상감정: 인근 PF 무산으로 약 296억 추정(정 |

### `05c_property_price_index.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `collateral_type` | str | 담보유형 | 아파트 |
| `region` | str | 권역 | 대전 |
| `year` | int64 | 연도 | 2015 |
| `index_2015_100` | float64 | 가격지수(2015=100) | 100.0 |

### `05d_credit_review_notes.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `note_id` | str | 의견번호 | N00001 |
| `loan_id` | str | 대출번호 | L003701 |
| `borrower_id` | str | 차주번호 | B003405 |
| `review_date` | str | 작성일 | 2025-10-27 |
| `review_type` | str | 의견 구분 | 정기 신용평가 |
| `reviewer` | str | 작성자 | 여신관리부 박*연 |
| `review_text` | str | 심사·사후관리 의견(텍스트) | 주거래 기업으로 거래 이력이 안정적임. 이자보상배율 6.1배로 상환능력  |

### `05e_pf_projects.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `project_id` | str | PF 사업장 번호 | PF001 |
| `loan_id` | str | 대출번호 | L004961 |
| `borrower_id` | str | 차주번호 | B004560 |
| `project_type` | str | 사업 유형 | 지식산업센터 |
| `region` | str | 권역 | 대전 |
| `total_project_cost` | float64 | 총사업비(원) | 28200000000.0 |
| `bank_exposure` | float64 | 당행 대출잔액(원) | 6051000000.0 |
| `seniority` | str | 트랜치 순위 | 선순위 |
| `contractor` | str | 시공사 | ㈜새봄건설 |
| `contractor_rating` | str | 시공사 신용등급 | BBB |
| `completion_guarantee` | str | 책임준공 형태 | 책임준공(시공사) |
| `construction_progress` | float64 | 공정률 | 0.453 |
| `presale_rate` | float64 | 분양률 | 0.285 |
| `maturity_extensions` | int64 | 만기연장 횟수 | 2 |
| `original_maturity` | str | 최초 만기 | 2027-11-20 |
| `current_maturity` | str | 현재 만기 | 2028-11-20 |

### `06_loan_monthly_history.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `loan_id` | str | 대출번호 | L000001 |
| `year_month` | str | 연월 | 2025-09 |
| `month_end_balance` | float64 | 월말 잔액(원) | 292000000.0 |
| `dpd_month_end` | int64 | 월말 연체일수 | 0 |
| `default_flag` | int64 | 월말 부도 상태(연체 90일 초과 또는 회생) | 0 |

### `06a_pd_training_2024.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `obs_id` | str | 관측치 ID | T04590 |
| `snapshot_date` | str | 관측 기준일(표기 혼재) | 2024/12/31 |
| `segment` | str | 세그먼트 코드(RET_MORT 주택담보, RET_JEONSE 전세, RET_CREDIT 신용, RET_OD 마이너스통장, SOHO 개인사업자, SME 중소기업, LARGE 대기업·중견, PF 부동산PF) | RET_MORT |
| `biz_type` | str | 가계/기업 | 가계 |
| `region` | str | 권역 | 세종 |
| `industry` | str | 업종(KSIC 대분류, 가계는 공란) | 부동산업 |
| `age` | float64 | 연령 | 63.0 |
| `annual_income_10k` | float64 | 연소득(만원) | 5345.0 |
| `credit_score` | float64 | 개인신용점수(300~1000) | 863.0 |
| `job_type` | str | 직업군 | 계약직 |
| `internal_grade` | int64 | 내부등급 | 4 |
| `loan_amount` | float64 | 대출금액(원) | 305700000.0 |
| `interest_rate` | float64 | 약정금리(연) | 0.0398 |
| `ltv` | float64 | 담보인정비율 | 0.51 |
| `dsr` | float64 | 총부채원리금상환비율(가계) | 0.341 |
| `debt_ratio_pct` | float64 | 부채비율(%) | 112.2 |
| `interest_coverage` | float64 | 이자보상배율 | 5.68 |
| `utilization` | float64 | 한도 사용률 | 0.231 |
| `months_on_book` | int64 | 경과 개월 | 17 |
| `max_dpd_6m` | int64 | 최근 6개월 최대 연체일수 | 0 |
| `num_delinquency_12m` | int64 | 최근 12개월 연체 횟수 | 1 |
| `collateral_type` | str | 담보유형 | 무담보 |
| `default_12m` | int64 | 향후 12개월 내 부도(1) 여부 — 목표변수 | 0 |

### `07_borrower_financials.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `borrower_id` | str | 차주번호 | B003405 |
| `fiscal_year` | int64 | 결산연도 | 2023 |
| `revenue` | float64 | 매출액 | 2956000000.0 |
| `operating_income` | float64 | 영업이익 | 293000000.0 |
| `net_income` | float64 | 당기순이익 | 220000000.0 |
| `total_assets` | float64 | 자산총계 | 3624000000.0 |
| `total_liabilities` | float64 | 부채총계 | 1480000000.0 |
| `total_equity` | float64 | 자본총계 | 2144000000.0 |
| `interest_expense` | float64 | 이자비용 | 44000000.0 |
| `operating_cash_flow` | float64 | 영업활동현금흐름 | 335000000.0 |
| `debt_ratio_pct` | float64 | 부채비율(%) | 69.0 |
| `interest_coverage` | float64 | 이자보상배율 | 6.7 |
| `audit_opinion` | str | 감사의견 | 비외감 |

### `08a_individual_assessment.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `loan_id` | str | 대출번호 | L004189 |
| `borrower_id` | str | 차주번호 | B003893 |
| `ead` | int64 | 회사 산정 EAD(원) | 1516100000 |
| `collateral_type` | str | 담보유형 | 보증서 |
| `recovery_basis` | str | 회수 근거 | 보증기관 대위변제 |
| `appraisal_value` | float64 | 최근 감정가(원) | 0.0 |
| `appraisal_date` | str | 최근 감정일 | 2024-09-09 |
| `senior_lien` | float64 | 선순위 채권액(원) | 0.0 |
| `assumed_haircut` | float64 | 회사 가정 담보 할인율 | 0.15 |
| `expected_recovery` | int64 | 회수예상액 | 1288685000 |
| `recovery_years` | float64 | 회수 예상기간(년) | 1.5 |
| `discount_rate` | float64 | 할인율(약정금리) | 0.0646 |
| `pv_recovery` | int64 | 회수예상액 현재가치 | 1173186564 |
| `ecl_individual` | int64 | 개별평가 ECL | 342913436 |
| `prepared_by` | str | 작성자 | 여신관리부 송예준 |
| `reviewed_by` | str | 검토자 | 여신관리부 임수아 |

### `09_macro_scenarios.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `scenario` | str | 시나리오 | base |
| `year` | int64 | 연도 | 2026 |
| `gdp_growth_pct` | float64 | GDP 성장률(%) | 1.8 |
| `unemployment_pct` | float64 | 실업률(%) | 3.0 |
| `base_rate_pct` | float64 | 기준금리(%) | 2.5 |
| `house_price_yoy_pct` | float64 | 주택가격 상승률(%) | 1.5 |
| `weight_policy_risk_committee` | float64 | 리스크관리위원회 의결 가중치 | 0.6 |
| `weight_used_in_ecl_calc` | float64 | ECL 계산에 실제 적용한 가중치 | 0.7 |
| `pd_multiplier_retail` | float64 | 가계 PD 승수 | 1.0 |
| `pd_multiplier_corporate` | float64 | 기업 PD 승수 | 1.0 |

### `09a_macro_history.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `quarter` | str | 분기 | 2010Q1 |
| `gdp_growth_yoy_pct` | float64 | GDP 성장률(전년동기비, %) | 3.08 |
| `unemployment_pct` | float64 | 실업률(%) | 3.58 |
| `base_rate_pct` | float64 | 기준금리(%) | 2.0 |
| `house_price_yoy_pct` | float64 | 주택가격 상승률(%) | -2.39 |
| `default_rate_retail_pct` | float64 | 가계 부도율(%) | 0.945 |
| `default_rate_corporate_pct` | float64 | 기업 부도율(%) | 1.556 |

### `10_interest_income_ledger.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `loan_id` | str | 대출번호 | L000001 |
| `year_month` | str | 연월 | 2025-09 |
| `segment` | str | 세그먼트 코드(RET_MORT 주택담보, RET_JEONSE 전세, RET_CREDIT 신용, RET_OD 마이너스통장, SOHO 개인사업자, SME 중소기업, LARGE 대기업·중견, PF 부동산PF) | RET_MORT |
| `month_end_balance` | float64 | 월말 잔액(원) | 292000000.0 |
| `interest_rate` | float64 | 약정금리(연) | 0.04 |
| `accrual_days` | int64 | 경과일수 | 2 |
| `accrued_interest` | int64 | 경과이자(이자수익 인식액) | 64000 |
| `collected_interest` | int64 | 수취이자 | 64000 |
| `uncollected_interest` | int64 | 미수이자 | 0 |
| `dpd_month_end` | int64 | 월말 연체일수 | 0 |
| `accrual_basis` | str | 이자 인식 기준 | 총장부금액 |

### `11_writeoff_recovery.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `writeoff_id` | str | 상각번호 | W00001 |
| `loan_id` | str | 대출번호 | LW00001 |
| `segment` | str | 세그먼트 코드(RET_MORT 주택담보, RET_JEONSE 전세, RET_CREDIT 신용, RET_OD 마이너스통장, SOHO 개인사업자, SME 중소기업, LARGE 대기업·중견, PF 부동산PF) | RET_OD |
| `secured` | str | 담보 여부 | N |
| `default_date` | str | 부도일 | 2019-03-13 |
| `ead_at_default` | float64 | 부도 시 EAD | 13720000.0 |
| `recovered_before_writeoff` | float64 | 상각 전 회수액 | 1260000.0 |
| `writeoff_date` | str | 상각일 | 2019-12-10 |
| `writeoff_amount` | float64 | 상각액 | 12460000.0 |
| `recovered_after_writeoff` | float64 | 상각 후 회수액(관측분) | 654000.0 |
| `workout_closed` | str | 회수 종결 여부(상각 후 2.5년 경과) | Y |
| `approval_ref` | str | 승인 근거 | 2025-06-19 이사회 제2호 의안 |
| `writeoff_quarter` | str | 상각 분기 | 2019Q4 |

### `13_loan_confirmations.csv`

| 컬럼 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `confirmation_id` | str | 조회번호 | CF001 |
| `loan_id` | str | 대출번호 | L004048 |
| `borrower_id` | str | 차주번호 | B003752 |
| `sent_date` | str | 발송일 | 2026-01-06 |
| `book_balance` | float64 | 장부잔액 | 90956000.0 |
| `response_status` | str | 회신 여부 | 회신 |
| `confirmed_balance` | float64 | 회신잔액 | 90956000.0 |
| `difference` | float64 | 차이(회신−장부) | 0.0 |
| `borrower_remark` | str | 차주 회신 의견 | 차주: 12/31 원금 일부 상환(은행 2026-01-02 처리) |

### 엑셀 파일

- `03_financial_statements.xlsx`: 표지, 재무상태표, 포괄손익계산서, 주석_충당금변동, 주석_Stage별대출, 주석_특수관계자
- `08_ecl_parameters.xlsx`: PD_12M, LGD_담보, CCF, 시나리오가중치, 기타가정
- `12_risk_reports.xlsx`: 월별지표, 업종별집중도, 상위20차주, 자산건전성분류_백만원, BIS산출요약

## 파일 간 연결 키

- `loan_id`: 05 ↔ 05b ↔ 05d ↔ 05e ↔ 06 ↔ 08a ↔ 10 ↔ 13
- `borrower_id`: 05 ↔ 05a ↔ 07
- `account_code`: 01 ↔ 01c ↔ 02
- `created_by` / `approved_by`: 01 ↔ 01a (`user_id`)
- `writeoff_quarter` 2025: 11 ↔ 04_board_minutes(분기별 상각 승인) ↔ 01(대손상각 전표의 reference = loan_id)
- 대출채권 총액: 05 `balance` 합계 = 02 계정 1400 기말잔액
- 대출 이자수익: 10 `accrued_interest` 합계 ≈ 02 계정 4100 (수동 조정 전표 제외)

