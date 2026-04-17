# 029 – PLAN

| Field | Value |
|---|---|
| Document ID | 029-PLAN-001 |
| Project | BOAT.029 – COB FACTOR RECON |
| Source | 029_Technical_Design_Document.md |
| Date | 2026-04-08 |
| Status | Draft v1.0 |

---

## Phase 1 – Assessment

**Goal:** Parse and understand the full BP 7.3 release; inventory all components; identify external dependencies; validate reuse opportunities.

| # | Task | Deliverable | Owner | Status |
|---|---|---|---|---|
| 1.1 | Parse BP7.3_029_Factor_Recon_Full_Package_Release_May28_2025_001.bprelease XML | Component inventory (9 processes, 8 objects, 2 queues, 22 env vars) | AGENT | Complete |
| 1.2 | Document all BP Process subsheets with narratives | BRD Section 3.1 (65 subsheets catalogued) | AGENT | Complete |
| 1.3 | Document all BP Object actions with complexity | BRD Section 3.2 (54 actions catalogued) | AGENT | Complete |
| 1.4 | Map BP Credentials to CyberArk and UiPath Assets | BRD Section 3.4 (3 credentials mapped) | AGENT | Complete |
| 1.5 | Map BP Environment Variables to Orchestrator Assets | BRD Section 3.5 (22 env vars mapped to 28 assets) | AGENT | Complete |
| 1.6 | Scan 058 and 057 reference folders for reusable components | RC-001 Document (160h savings confirmed) | AGENT | Complete |
| 1.7 | Confirm external systems: MCH/AVIVA, Bloomberg HTTP WS, RDR Cloud Client, SQL, SMTP | BRD Section 5 (11 systems identified) | AGENT + LEAD | Complete |
| 1.8 | Confirm BOAT.COB.MAP.Library NOT required for 029 (no MAP integration) | Architecture decision logged | AGENT | Complete |
| 1.9 | Confirm new libraries required: MCH (FR-specific), Accounting_WS, DataTransformation, Database, MasterScheduler, Reporting, Posting | TDD Section 3 | AGENT | Complete |

---

## Phase 2 – Design

**Goal:** Define complete UiPath architecture; queue schemas; exception strategy; Config.xlsx design; naming conventions.

| # | Task | Deliverable | Owner | Status |
|---|---|---|---|---|
| 2.1 | Define UiPath project folder structure (8 processes, 7 libraries) | TDD Section 1.1 | AGENT | Complete |
| 2.2 | Define queue schemas for FundListFile and AccountingDataFile queues | TDD Section 4 | AGENT | Complete |
| 2.3 | Define all 28 Orchestrator Assets with names, types, descriptions | TDD Section 5 | AGENT | Complete |
| 2.4 | Define exception handling matrix (12 exception types) | TDD Section 6 | AGENT | Complete |
| 2.5 | Define Config.xlsx structure (Settings, Constants, Assets sheets) | TDD Section 9 | AGENT | Complete |
| 2.6 | Define naming conventions (processes, libraries, workflows, assets, queues) | TDD Section 10 | AGENT | Complete |
| 2.7 | Define REFramework process flow for each performer process | TDD Section 2 | AGENT | Complete |
| 2.8 | Architecture review — LEAD approval of library structure and queue model | Architecture sign-off | LEAD-1 | Pending |
| 2.9 | Orchestrator folder structure design and approval | TDD Section 1.3 | LEAD-1 | Pending |

---

## Phase 3 – Build

**Goal:** Import reusable components, build all 7 new libraries, wire 8 UiPath processes, validate end-to-end.

### Sub-Phase 3A – Foundation (Sprint 1, Week 1–2)

| # | Task | Deliverable | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|---|
| S1-01 | Clone REFramework from 057 – Ben Profiling, update Config.xlsx for 029 | 029 project scaffold | DEV-1 | XS | 1 | YES – 057 |
| S1-02 | Import SSC.Platform.Logging, LoginAgent, Environment, Excel.Utilities NuGet packages | 4 libraries available | DEV-1 | XS | 1 | YES – 058 |
| S1-03 | Import Common workflows from 057 (GetCredential, SendEmail, KillExcel, etc.) | Common/ folder populated | DEV-2 | XS | 1 | YES – 057 |
| S1-04 | Import WorkQueue wrapper workflows from 057 | WorkQueue/ folder populated | DEV-2 | XS | 1 | YES – 057 |
| S1-05 | Configure all 28 Orchestrator Assets in DEV Orchestrator folder | Assets deployed | LEAD-1 | S | 2 | YES – 057 pattern |
| S1-06 | Configure 2 Orchestrator Queues (FundListFile, AccountingDataFile) | Queues deployed | LEAD-1 | XS | 1 | YES – 057 pattern |
| S1-07 | BRD review and sign-off | Approved 029-BRD-001 | LEAD-1 | XS | 1 | – |
| S1-08 | TDD review and sign-off | Approved 029-TDD-001 | LEAD-1 | S | 2 | – |
| S1-09 | Build GIO.FR.MCH.Library — Login/Logoff/CheckIfLogged/Startup actions | 4 MCH workflows | DEV-1 | M | 4 | PARTIAL – 058 login pattern |
| S1-10 | Build GIO.FR.MCH.Library — BGMM and BGIR screen workflows | 2 MCH screen workflows | DEV-2 | M | 4 | NEW |
| S1-11 | Build GIO.FR.Database.Library — all 5 DB workflows | Database library | DEV-2 | M | 4 | NEW |
| S1-12 | Build GIO.FR.MasterScheduler.Library — ReadFile + FieldExists + ValuesValidation | 3 validation workflows | DEV-1 | M | 4 | NEW |

### Sub-Phase 3B – Core Libraries (Sprint 2, Week 3–4)

| # | Task | Deliverable | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|---|
| S2-01 | Build GIO.FR.MCH.Library — BSEC, BPOP screens | 2 MCH screen workflows | DEV-1 | M | 4 | NEW |
| S2-02 | Build GIO.FR.MCH.Library — PYDN screen (standard posting) | PYDN workflow | DEV-1 | L | 8 | NEW |
| S2-03 | Build GIO.FR.MCH.Library — PYUP screen | PYUP workflow | DEV-2 | L | 8 | NEW |
| S2-04 | Build GIO.FR.MCH.Library — PYDA screen | PYDA workflow | DEV-2 | L | 8 | NEW |
| S2-05 | Build GIO.FR.MasterScheduler.Library — 4 regional holiday date format validators | 4 holiday workflows | DEV-1 | M | 4 | NEW |
| S2-06 | Build GIO.FR.Accounting_WS.Library — GMM_WS + Group_CUSIP | 2 extraction workflows | DEV-2 | L | 8 | NEW |
| S2-07 | Build GIO.FR.Accounting_WS.Library — Bloomberg HTTP WS extraction | Bloomberg workflow | DEV-1 | L | 8 | NEW |
| S2-08 | Build GIO.FR.Accounting_WS.Library — RDR Login + RDR WS Extraction | 2 RDR workflows | DEV-2 | L | 8 | NEW |
| S2-09 | Architecture review mid-sprint — MCH screens sign-off by LEAD | Review complete | LEAD-1 | S | 2 | – |
| S2-10 | Build 029_FR_Dispatcher_MainScheduler process (Main + 5 workflows) | Dispatcher process | DEV-1 | M | 4 | PARTIAL – pattern from 057 |

### Sub-Phase 3C – Performers & Remaining Libraries (Sprint 3, Week 5–6)

| # | Task | Deliverable | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|---|
| S3-01 | Build GIO.FR.Accounting_WS.Library — MergeBB_LotLevel + Aggregate_Filter | 2 workflows | DEV-1 | M | 4 | NEW |
| S3-02 | Build GIO.FR.DataTransformation.Library — Filter_Calculate + Set_Column_Data + Change_Column_Position | 3 workflows | DEV-2 | M | 4 | NEW |
| S3-03 | Build GIO.FR.DataTransformation.Library — BGIR_WS_Call + BGI_Loop + Write_ToExcel | 3 workflows | DEV-1 | M | 4 | NEW |
| S3-04 | Build GIO.FR.Reporting.Library — CreateReport + CreateWorksheetAndWriteCollection + WriteDailyReport | 3 workflows | DEV-2 | M | 4 | NEW |
| S3-05 | Build GIO.FR.Posting.Library — CreateInstance + DeleteSheetContents + CreateSheetAndWriteContents + Check_PostingType | 4 workflows | DEV-1 | M | 4 | NEW |
| S3-06 | Build GIO.FR.Posting.Library — Main_Action + Posting | 2 workflows | DEV-2 | L | 8 | NEW |
| S3-07 | Build GIO.FR.Posting.Library — FR.PYDN/PYUP/PYDA_Posting | 3 standard posting workflows | DEV-1 | L | 8 | NEW (PYDN pattern, then reuse for PYUP/PYDA) |
| S3-08 | Build GIO.FR.Posting.Library — FR.PYDN/PYUP/PYDA_Posting_UMAS | 3 UMAS posting workflows | DEV-2 | L | 8 | NEW (UMAS variant of above) |
| S3-09 | Build 029_FR_Performer_DataExtraction — all 7 workflows | Data Extraction process | DEV-1 | L | 8 | NEW |
| S3-10 | Build 029_FR_Sentinel — all 6 workflows | Sentinel process | DEV-2 | M | 4 | NEW |
| S3-11 | Unit testing — MCH Library (all screens) | Test results | DEV-1+2 | M | 4 | – |
| S3-12 | Code review Sprint 2 & 3 libraries | Review complete | LEAD-1 | M | 4 | – |

### Sub-Phase 3D – Integration (Sprint 4, Week 7–8)

| # | Task | Deliverable | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|---|
| S4-01 | Build 029_FR_Performer_TransformPosting — all 5 workflows | TransformPosting process | DEV-1 | L | 8 | NEW |
| S4-02 | Build 029_FR_Dispatcher_Adhoc + Dispatcher_NOVAL | 2 dispatcher processes | DEV-2 | S | 2 | PARTIAL – MainScheduler pattern |
| S4-03 | Build 029_FR_SLAMonitor process | SLA Monitor process | DEV-1 | S | 2 | NEW |
| S4-04 | Build 029_FR_DatabaseCleanup process | DB Cleanup process | DEV-2 | S | 2 | NEW |
| S4-05 | Integration testing — Dispatcher → DataExtraction queue flow | Test results | DEV-1+2 | M | 4 | – |
| S4-06 | Integration testing — DataExtraction → TransformPosting queue flow | Test results | DEV-1+2 | M | 4 | – |
| S4-07 | End-to-end test with sample production data (3 fund lists) | E2E test report | LEAD-1 + DEV | L | 8 | – |
| S4-08 | UAT support — business validation of output data | UAT sign-off | LEAD-1 | M | 4 | – |
| S4-09 | Package all processes for Orchestrator deployment | NuPkg files published | DEV-1 | S | 2 | – |

---

## Phase 4 – Testing

**Goal:** Unit test every library workflow; integration test all queue flows; business validate with sample data.

| Test Type | Scope | Owner | Pass Criteria |
|---|---|---|---|
| Unit Test – MCH Library | All 11 MCH workflows in isolation | DEV-1 | Each workflow executes without exception; output arguments populated correctly |
| Unit Test – Accounting_WS Library | All 8 extraction workflows | DEV-2 | Response data collection non-empty for known test inputs |
| Unit Test – DataTransformation Library | All 6 transform workflows | DEV-1 | Output DataTable matches expected schema and row count |
| Unit Test – Posting Library | All 13 posting workflows | DEV-2 | No MCH posting errors on test environment; posting confirmed on MCH screen |
| Integration Test – Dispatcher | MainScheduler→FundListFile→Queue | DEV-1+2 | Queue populated with correct items per test scheduler file |
| Integration Test – DataExtraction | Queue→Extraction→AccountingQueue | DEV-1+2 | All queue items processed; AccountingDataFile queue populated |
| Integration Test – Transform/Post | AccountingQueue→Transform→MCH Post | DEV-1+2 | MCH shows correct posting entries; lot-level report generated |
| Business Validation | Full COB run on 3 fund list samples | LEAD-1 + Business | Output matches BP baseline (record count, values, MCH screen state) |
| Performance Test | Batch of 50 fund items end-to-end | LEAD-1 | Completes within SLA threshold (GIO_FR_SLA_Threshold) |
| Exception Test – Business | Invalid scheduler, missing fund | DEV-1 | Queue marked Business Exception; email sent; no unhandled crash |
| Exception Test – System | MCH timeout simulation | DEV-2 | Retry fires 3x; screenshot captured; alert email sent |
| Security Test | Credential log scan | LEAD-1 | Zero credential values in any log output |

---

## Phase 5 – Deployment

**Goal:** Deploy to production Orchestrator; configure schedules; verify assets and queues.

| # | Checklist Item | Owner |
|---|---|---|
| 5.1 | Publish all 7 libraries to internal NuGet feed | DEV-1 |
| 5.2 | Publish all 8 process packages to Orchestrator | DEV-1 |
| 5.3 | Deploy Orchestrator folder structure (SSC/GIO/029_Factor_Recon) | LEAD-1 |
| 5.4 | Create/update 28 assets in production Orchestrator | LEAD-1 |
| 5.5 | Create/verify 2 Orchestrator Queues in production | LEAD-1 |
| 5.6 | Register robot machine and assign unattended license | LEAD-1 + Platform |
| 5.7 | Configure CyberArk safe and account objects for robot | Platform Engineering |
| 5.8 | Configure Orchestrator schedules (Sentinel, Dispatchers, SLAMonitor, DBCleanup) | LEAD-1 |
| 5.9 | Smoke test: execute Sentinel in production environment | LEAD-1 + DEV-1 |
| 5.10 | Smoke test: queue one item manually via Adhoc dispatcher; run DataExtraction | LEAD-1 + DEV-2 |
| 5.11 | Verify all logs appearing in ELK/Splunk dashboard | LEAD-1 |
| 5.12 | Conduct walkthrough with Business team and RPA Support L1 | LEAD-1 |
| 5.13 | Handover support documentation (SOP, runbook, escalation matrix) | AGENT |

---

## Phase 6 – Cutover

| # | Checklist Item | Owner |
|---|---|---|
| 6.1 | Coordinate parallel run: BP and UiPath run simultaneously on non-critical data (1–2 days) | LEAD-1 + Business |
| 6.2 | Business signs off that UiPath output matches BP output | Business Owner |
| 6.3 | Disable BP schedules for Factor Recon | RPA Support L1 |
| 6.4 | Enable full UiPath production schedules | LEAD-1 |
| 6.5 | Monitor for 3 consecutive production COB runs | LEAD-1 + L1 |
| 6.6 | Document and resolve any post-go-live issues within 48h SLA | DEV-1 or DEV-2 |
| 6.7 | Post-implementation review: log issue count, exception rate, SLA adherence | LEAD-1 |
| **Rollback Plan** | If 2+ consecutive COB failures: re-enable BP schedule; disable UiPath; L1 triage | LEAD-1 |
