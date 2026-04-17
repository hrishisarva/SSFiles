# 112 MDS OCFC — Job Skills

| Field | Value |
|---|---|
| Document ID | 112-JS-001 |
| Project | MDS.112 – COB MDS |
| Source | 112_Project_Plan.md + 112_Technical_Design_Document.md + 112_Development_Guide.md |
| Date | 2026-04-13 |
| Status | Draft v1.0 |

---

## Section 1 — DEV-1 – Developer 1

**Focus:** Core library development — MCH terminal screens, RKS automation (all 3 modes), process integration, PROD deployment.

### Must-Have Skills

| # | Skill | Proficiency | Context in This Project |
|---|---|---|---|
| 1 | UiPath Studio (Windows — Legacy + Modern) | Expert | All workflow development — 112 uses Windows target (S1-09 through S6-04) |
| 2 | UiPath REFramework | Advanced | Dispatcher/Performer wiring for Sentinel (S4-11), Factor Generic (S5-01), Null Factor (S5-02) |
| 3 | Terminal Emulation (TN3270 / Mainframe) | Advanced | MCH BFSE, BSEC, BGOV screen workflows (S2-01 through S2-06) — Navigate Transaction, Read/Write screen fields, handle cursor positioning |
| 4 | Citrix Automation (UiPath UIA + Image) | Advanced | RKS launch modes — UIA (S3-03), session management, selector capture in Citrix WorkSpace (S3-01 through S3-05) |
| 5 | Browser Automation (UiPath Edge/Chrome) | Advanced | RKS Edge mode (S3-02), Web objects (ECF, IMS) |
| 6 | UiPath Selectors (Dynamic, Anchors, XAML) | Expert | 35 RKS screens + MCH terminal — requires dynamic selectors, wildcards, idx attributes across sessions |
| 7 | Orchestrator Assets & Queues | Advanced | 26 queues, 272 assets — create, configure, test (S1-16 through S1-18, S6-04, S6-05) |
| 8 | VB.Net / .NET Framework | Intermediate | String manipulation, DataTable operations in business logic, date calculations for coupon types (S4-01 through S4-04) |
| 9 | Exception Handling (Business + System) | Advanced | Per exception matrix — 15 exception scenarios with retry logic (all sprints) |
| 10 | SQL / Database Queries | Intermediate | DB_Interactions library — ExecuteQuery, ExecuteNonQuery, connection strings (S1-09, S1-10) |

### Good-to-Have Skills

| # | Skill | Context |
|---|---|---|
| 1 | CyberArk Credential Provider | Robot account mapping for MCH (racf), RKS (adcorp+robmfa) — S6-08 |
| 2 | PowerShell Scripting | Orchestrator asset automation script for PROD deployment — S6-05 |
| 3 | Performance Profiling (UiPath) | Execution time comparison bp vs. UiPath — S5-15 |
| 4 | UiPath Package Publishing | NuGet feed publishing for libraries — S6-04 |

### Sprint Story Assignments

| Sprint | Stories | Primary Skills Used |
|---|---|---|
| Sprint 1 | S1-01→S1-08 (♻️ imports), S1-09, S1-10, S1-13, S1-14, S1-22, S1-23, S1-24 | REFramework, SQL, Orchestrator, Terminal, Citrix (spikes) |
| Sprint 2 | S2-01→S2-06 | Terminal Emulation (MCH), Selectors |
| Sprint 3 | S3-01→S3-05 | Citrix, Browser, RKS Selectors |
| Sprint 4 | S4-01→S4-04 | VB.Net, Business Logic, Exception Handling |
| Sprint 5 | S5-01→S5-05 | REFramework, Unit Testing, Process Integration |
| Sprint 6 | S6-01→S6-05 | UAT, Deployment, Orchestrator |

---

## Section 2 — DEV-2 – Developer 2

**Focus:** HTTP/API integration, ESP ODBC, web object automation (2FA flows), business logic (coupon/date checks), testing.

### Must-Have Skills

| # | Skill | Proficiency | Context in This Project |
|---|---|---|---|
| 1 | UiPath Studio (Windows — Legacy + Modern) | Expert | All workflow development — parallel track with DEV-1 (S1-11 through S6-10) |
| 2 | UiPath REFramework | Advanced | Process wiring for Coupon/Date (S5-06), Specialized processes (S5-07) |
| 3 | HTTP/REST API Integration | Expert | 9 HTTP libraries — MDP (ForgeRock OAuth2), SSCD, RMG, ERP, RDOC, GTM (S2-08 through S2-13). Must handle: token refresh, bearer auth, JSON parsing, error codes |
| 4 | OAuth2 / ForgeRock Authentication | Advanced | MDP_HTTP Core (S2-08) — multi-step auth: access token → resource server → API call. ForgeRock-specific flow |
| 5 | ODBC / Database Connectivity | Advanced | ESP Library (S3-08) — ODBC connection, parameterized queries, connection pool management |
| 6 | Browser Automation (UiPath) | Advanced | Web_ECF, Web_IMS, Web_PSDL objects (S3-09, S4-08, S4-09) — form fill, 2FA handling, session management |
| 7 | VB.Net / .NET Framework | Advanced | Business logic — 3917, 3006 check logic (S4-05, S4-06), coupon type date calculations, string operations |
| 8 | JSON / XML Parsing | Advanced | HTTP API response parsing for MDP, SSCD, RMG, ERP, RDOC, GTM — all Sprint 2 HTTP stories |
| 9 | Exception Handling (Business + System) | Advanced | HTTP timeout/retry logic, ODBC connection retry, 2FA failure handling |
| 10 | Unit Testing (UiPath Test Suite) | Advanced | Test all HTTP/API flows (S5-08), business logic scenarios (S5-09), ESP + Web objects (S5-10) |

### Good-to-Have Skills

| # | Skill | Context |
|---|---|---|
| 1 | SecurID / 2FA Token Management | ERP, GTM, MDP use SecurID tokens — timing coordination |
| 2 | CyberArk Integration | Credential retrieval for web objects (robmfa, edr domains) — S6-08 |
| 3 | Excel Interop / OLEDB | Report generation, bulk data handling if needed |
| 4 | SOAP Web Services | Some legacy system APIs may use SOAP alongside REST |

### Sprint Story Assignments

| Sprint | Stories | Primary Skills Used |
|---|---|---|
| Sprint 1 | S1-11, S1-12, S1-15, S1-16, S1-17, S1-18 | VB.Net, Orchestrator, SQL |
| Sprint 2 | S2-07→S2-13 | HTTP/REST, OAuth2/ForgeRock, JSON, Terminal (MCH) |
| Sprint 3 | S3-06, S3-07, S3-08, S3-09 | ODBC, Browser, Citrix (RKS), 2FA |
| Sprint 4 | S4-05→S4-09 | VB.Net, Business Logic, Browser |
| Sprint 5 | S5-06→S5-11 | REFramework, Unit Testing |
| Sprint 6 | S6-06→S6-10 | UAT, Deployment, CyberArk |

---

## Section 3 — LEAD – Technical Lead

**Focus:** Architecture decisions, code review (every sprint), integration testing, Orchestrator governance, deployment sign-off, stakeholder coordination.

### Must-Have Skills

| # | Skill | Proficiency | Context in This Project |
|---|---|---|---|
| 1 | UiPath Solution Architecture | Expert | REFramework Dispatcher/Performer design, 14-system integration architecture, queue topology for 26 queues (S1-19) |
| 2 | Code Review (UiPath Best Practices) | Expert | Every sprint: selector quality, naming conventions, exception handling, logging patterns (S1-20, S2-14, S2-15, S3-10, S3-11, S4-10) |
| 3 | UiPath Orchestrator Administration | Expert | Folder structure, robot assignment, schedule management, asset governance (S1-19, S4-12, S6-12) |
| 4 | Integration Testing | Expert | E2E flows: DB→Queue→MCH→RKS→Business Logic→Output (S1-21, S2-16, S2-17, S3-12, S5-12, S5-13) |
| 5 | .NET Framework / C# / VB.Net | Advanced | Complex expression review, DataTable manipulation, LINQ queries in business logic workflows |
| 6 | CyberArk Administration | Advanced | Robot identity setup, safe configuration, credential rotation procedures (S6-08 oversight) |
| 7 | Terminal Emulation Architecture | Advanced | MCH screen navigation patterns, cursor management, screen validation strategies (S2-16 review) |
| 8 | Citrix Architecture | Advanced | RKS launch mode selection (HTTP vs. Edge vs. UIA), session isolation, selector strategy for Citrix sessions (S3-10, S3-12, S3-13) |
| 9 | HTTP Security (OAuth2, TLS, Token Mgmt) | Advanced | ForgeRock OAuth2 architecture review, token lifecycle management, API retry patterns (S2-17) |
| 10 | Deployment & Go-Live Procedures | Expert | PROD checklist execution, parallel run supervision, rollback plan, go-live approval (S6-11 through S6-14) |

### Good-to-Have Skills

| # | Skill | Context |
|---|---|---|
| 1 | Blue Prism (reading/analysis) | Understanding BP source for validation during integration testing |
| 2 | NuGet Package Management | Library versioning, feed publishing, dependency management |
| 3 | ITIL / Change Management | PROD deployment change tickets, CAB approvals |
| 4 | Performance Tuning | Execution time optimization if BP parity not initially met (S5-15) |

### Sprint Story Assignments

| Sprint | Stories | Primary Skills Used |
|---|---|---|
| Sprint 1 | S1-19, S1-20, S1-21, S1-22/23/24 (reviews) | Architecture, Code Review, Integration Testing, Spike Validation |
| Sprint 2 | S2-14, S2-15, S2-16, S2-17, S2-18 | Code Review, Integration Testing, HTTP Security |
| Sprint 3 | S3-10, S3-11, S3-12, S3-13 | Code Review — Citrix Selectors, RKS E2E Testing |
| Sprint 4 | S4-10, S4-11, S4-12, S4-13 | Code Review — Business Logic, Sentinel, Orchestrator Config |
| Sprint 5 | S5-12, S5-13, S5-14, S5-15 | Full E2E Integration Testing, Performance |
| Sprint 6 | S6-11, S6-12, S6-13, S6-14, S6-15 | UAT, Deployment, Go-Live, Knowledge Transfer |

---

## Section 4 — Skills Comparison Summary

| Skill Area | DEV-1 | DEV-2 | LEAD |
|---|---|---|---|
| UiPath Studio (Windows) | ✅ Primary | ✅ Primary | ✅ Primary |
| REFramework | ✅ Primary | ✅ Primary | ✅ Primary |
| Terminal Emulation (TN3270) | ✅ Primary | ✅ Intermediate | ✅ Intermediate |
| Citrix Automation | ✅ Primary | ✅ Intermediate | ✅ Primary (architecture) |
| Browser Automation | ✅ Primary | ✅ Primary | ✅ Intermediate |
| UiPath Selectors | ✅ Primary | ✅ Primary | ✅ Primary (review) |
| HTTP/REST API | ⬜ Not required | ✅ Primary | ✅ Intermediate (review) |
| OAuth2 / ForgeRock | ⬜ Not required | ✅ Primary | ✅ Primary (architecture) |
| ODBC / Database | ✅ Intermediate | ✅ Primary | ✅ Intermediate |
| VB.Net / .NET | ✅ Intermediate | ✅ Primary | ✅ Primary |
| JSON / XML Parsing | ⬜ Not required | ✅ Primary | ✅ Intermediate |
| SQL Queries | ✅ Intermediate | ✅ Intermediate | ✅ Intermediate |
| CyberArk | ✅ Intermediate | ✅ Intermediate | ✅ Primary |
| Orchestrator Admin | ✅ Primary | ✅ Intermediate | ✅ Primary |
| Code Review | ⬜ Not required | ⬜ Not required | ✅ Primary |
| Integration Testing | ⬜ Not required | ⬜ Not required | ✅ Primary |
| Solution Architecture | ⬜ Not required | ⬜ Not required | ✅ Primary |
| Deployment / Go-Live | ✅ Intermediate | ✅ Intermediate | ✅ Primary |
| Unit Testing | ✅ Intermediate | ✅ Primary | ✅ Intermediate |
| 2FA / SecurID | ⬜ Not required | ✅ Intermediate | ✅ Intermediate |

---

## Section 5 — Skill Gap Risk

| # | Role | Risk Skill | If Missing | Prob | Mitigation |
|---|---|---|---|---|---|
| 1 | DEV-1 | Citrix Automation (UIA mode) | RKS UIA launch (S3-03) fails — cannot capture selectors through Citrix workspace. Sprint 3 blocked. | 30% | Sprint 1 spike (S1-24) validates Citrix approach by Week 2. If DEV-1 cannot capture UIA selectors, LEAD provides hands-on support (S3-13 contingency). |
| 2 | DEV-1 | Terminal Emulation (TN3270) | MCH screens BFSE/BSEC (S2-01 through S2-06) take 2x estimated time — cursor positioning and screen wait patterns unfamiliar. | 25% | Sprint 1 spike (S1-22) validates MCH terminal workflow. Import 058 MCH Core patterns as reference. ♻️ 058 MCH patterns reduce learning curve. |
| 3 | DEV-2 | OAuth2 / ForgeRock | MDP HTTP Core (S2-08) fails — cannot authenticate via ForgeRock. All MDP-dependent processes blocked. | 35% | Sprint 1 spike (S1-23) validates ForgeRock by Week 2. LEAD reviews architecture (S1-21). Fallback: SecurID direct login if ForgeRock auth not feasible. |
| 4 | DEV-2 | ODBC Connectivity | ESP Library (S3-08) ODBC connection fails from robot machine. ESP data retrieval blocked. | 20% | TDD includes HTTP fallback for ESP. DEV-2 tests ODBC in Sprint 2 (S2-18 connectivity validation). If ODBC fails, HTTP scaffold ready from Agent. |
| 5 | DEV-2 | 2FA Token Timing | Web_ECF (S3-09), Web_IMS (S4-08) — 2FA token expires mid-automation. Login loops or failures. | 40% | Configurable retry delay in environment variables (MDS_OCFC_2FA_WAIT_TIME). DEV-2 tests token timing window in Sprint 3. LEAD oversees retry pattern (S3-11 code review). |
| 6 | LEAD | Citrix Architecture (selector strategy) | Cannot resolve RKS selector instability — selectors break across Citrix session refreshes. Sprint 3 integration tests (S3-12) fail. | 25% | LEAD allocates dedicated troubleshooting time (S3-13, 8h). Fallback: convert unstable UIA selectors to Image-based automation for critical screens. LEAD has 12h/sprint buffer for unexpected issues. |
| 7 | DEV-1 | SQL / DB Locking | DB_Lock mechanism (S1-10) deadlocks under concurrent performers — ConcurrentAccess handler fails. | 15% | Unit test locking in Sprint 1 (S1-10). LEAD reviews lock timeout config (S1-20). Concurrent test with 2 performers in S5-13. |
| 8 | DEV-2 | JSON Complex Parsing | MDP API responses with nested JSON — deserialization fails for edge-case payloads. | 20% | Agent generates complete JSON deserialization code. DEV-2 validates against sample API responses in Sprint 2 (S2-08). LEAD reviews JSON handling patterns (S2-15). |

---

## Section 6 — Unique Project Skills vs. Previous Projects

| Skill Area | 058 — Market Profiling | 057 — Ben Profiling | 029 — Factor Recon | **112 — MDS OCFC (NEW)** |
|---|---|---|---|---|
| Terminal Emulation (MCH) | ✅ MCH Core (14 WF) | ⬜ Not used | ✅ MCH extended (BGMM, BGIR) | ✅ MCH BFSE, BSEC, BGOV — **NEW screens** |
| Citrix Automation (RKS) | ⬜ Not used | ⬜ Not used | ⬜ Not used | ✅ **NEW — 3 launch modes (HTTP, Edge, UIA), 35 workflows** |
| ForgeRock OAuth2 (MDP) | ⬜ Not used | ⬜ Not used | ⬜ Not used | ✅ **NEW — Multi-step OAuth2 with token refresh** |
| ODBC Connectivity (ESP) | ⬜ Not used | ⬜ Not used | ⬜ Not used | ✅ **NEW — ODBC driver, parameterized queries** |
| 2FA Web Automation (ECF/IMS) | ⬜ Not used | ⬜ Not used | ⬜ Not used | ✅ **NEW — Browser 2FA with token timing** |
| Business Logic (16 coupon types) | ⬜ Not used | ⬜ Not used | ✅ Factor types (partial) | ✅ **EXPANDED — 16 scenarios (CORP/GOVT, MUNI, MTGE, Float, Step)** |
| DB Locking Mechanism | ⬜ Not used | ⬜ Not used | ⬜ Not used | ✅ **NEW — Custom concurrent access handler** |
| 14-System Integration | MAP + MCH (2) | MCH (1) | MCH + MAP (2) | ✅ **14 systems — highest integration complexity** |
| 26 Work Queues | 4 queues | 3 queues | 5 queues | ✅ **26 queues — highest queue count** |
| MAP Library | ✅ Built (25 WF) | ⬜ Not used | ♻️ Imported from 058 | ⬜ Not needed for 112 |
| Excel Utilities | ✅ Built (3 WF) | ⬜ Not used | ♻️ Imported from 058 | ⬜ Not needed for 112 |

### Hard Hiring Requirements (Skills NEW to 112)

The following skills have **not** been required by any previous project. They must be verified during candidate screening:

| # | Skill | Why It's Critical | Hiring Impact |
|---|---|---|---|
| 1 | **Citrix Automation (3 modes)** | 35 RKS workflows — largest library in this project (28% of build effort) | DEV-1 MUST demonstrate Citrix selector capture experience. No substitute. |
| 2 | **ForgeRock OAuth2** | MDP is the gateway to multiple dependent systems | DEV-2 MUST demonstrate REST API with OAuth2 token management. ForgeRock-specific is ideal; generic OAuth2 is acceptable. |
| 3 | **ODBC / Direct DB** | ESP Library — ODBC connection is the primary data path | DEV-2 MUST have ODBC experience. Fallback to HTTP exists but is secondary. |
| 4 | **2FA Web Flow Timing** | ECF, IMS, PSDL web objects require 2FA with token timing | DEV-2 should demonstrate browser automation with MFA handling. |
| 5 | **RKS Domain Knowledge** | Understanding RKS screen layouts, navigation, data entry patterns | LEAD SHOULD have some familiarity with RKS — or plan for 4h learning time in Sprint 1 spikes. |

> **Note:** All skills related to MCH Core (Launch, Login, Navigate, Close), Logging, LoginAgent, REFramework, Common workflows, and WorkQueue wrappers are **♻️ from 058 — import only, no build skill needed.** These are proven patterns that require only validation, not construction.
