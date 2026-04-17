# 112 MDS OCFC — Project Plan

| Field | Value |
|---|---|
| Document ID | 112-PP-001 |
| Project | MDS.112 – COB MDS |
| Source | 112_Plan.md + 112_Development_Guide.md |
| Date | 2026-04-13 |
| Status | Draft v1.0 |
| Methodology | Agile sprints (2 weeks each), phase-gated with continuous testing |
| Duration | 12 weeks / 6 sprints / ~1,254 hours |
| Sprint Duration | 2 weeks (10 working days) |
| Team | 2 Developers (80%), 1 Lead (80%), 1 Agent (AI) |

**Duration: 12 weeks / 6 sprints / ~1,254 hours**

> **Assumption:** This plan assumes reusable components from 058 – Market Profiling (MCH Core, Logging, LoginAgent, Environment, Common Workflows, WorkQueue Wrappers, REFramework, Exception Strategy, Orchestrator Config) are available for import at 0 effort. This plan covers project-specific effort only. The MCH screen-specific workflows (BFSE, BSEC, BGOV) are NEW builds specific to 112.

---

## Section 1 — Team Capacity

### 1.1 Resource Allocation

| Role | ID | Expertise | Rate Assumption | Key Responsibilities |
|---|---|---|---|---|
| Developer 1 | DEV-1 | UiPath Studio, HTTP/API, RKS browser automation, selectors | $X/hr | RKS library, HTTP libraries, Web Objects, selectors |
| Developer 2 | DEV-2 | UiPath Studio, Terminal emulation, MCH, ESP, DB interactions | $X/hr | MCH screens, ESP library, DB library, Business Logic |
| Technical Lead | LEAD-1 | Architecture, BP analysis, Orchestrator, code review, integration testing | $X/hr | Architecture, code review, integration testing, deployment |
| Agent (AI) | AGENT-1 | BP analysis, documentation, XAML scaffolding, VB.NET generation | N/A | BRD, TDD, scaffolds, Config.xlsx, test docs, closure docs |

### Work Assumptions

| # | Parameter | Value | Notes |
|---|---|---|---|
| 1 | Sprint length | 2 weeks (10 working days) | |
| 2 | Hours per day | 8 hours | |
| 3 | DEV effective hours per sprint | 64h (80% of 80h) | |
| 4 | LEAD available hours per sprint | 64h (80% of 80h) | |
| 5 | LEAD planned hours per sprint | 52h (~81% utilization of 64h available) | 12h contingency buffer |
| 6 | RKS screen workflow build time (first) | 6h | Citrix/browser — complex selectors |
| 7 | RKS screen workflow build time (subsequent ♻️) | 3h | Pattern reuse from first build |
| 8 | MCH screen workflow build time (first) | 5h | Terminal emulation — field mapping |
| 9 | MCH screen workflow build time (subsequent ♻️) | 2.5h | Pattern reuse from first build |
| 10 | HTTP API workflow build time | 4–8h | Depending on action count |
| 11 | Business logic workflow build time | 4–8h | Depending on scenario complexity |
| 12 | Unit test time per workflow | 1h | |
| 13 | Integration test time per process | 4h | |
| 14 | Code review throughput | 1h per workflow | |
| 15 | Sprint ceremonies | 3h per sprint | Planning 1h, Demo 1h, Retro 0.5h, Grooming 0.5h |
| 16 | Agent scaffold generation time | 0.5h per workflow (simple), 1h per workflow (complex) | |
| 17 | Rework budget | 10% of build effort per sprint | |
| 18 | Library import/verify time | 0.5h per library | |
| 19 | Config.xlsx population time | 4h for 272 assets | |
| 20 | Queue creation time | 0.5h per queue (26 queues = 13h) | |

### 1.2 Project Totals

| Metric | Value |
|---|---|
| Total Sprints | 6 |
| Total Duration | 12 weeks |
| Total Human Capacity | 6 sprints × (64 + 64 + 52) = 6 × 180 = **1,080h** |
| Planned Work | DEV-1(384h) + DEV-2(384h) + LEAD(312h) + Agent(174h) = **1,254h** |
| Estimated Effort | ~1,254h |
| Reuse Savings | 148h (from 058 reusable components at 0 effort) |
| Total Story Points | 549 |
| Target Velocity | 549 ÷ 6 = ~92 SP/sprint |
| Start Date | 2026-04-13 |
| End Date | 2026-07-05 |

### 1.3 T-Shirt Sizing Reference

| Size | Hours | Days | Story Points |
|---|---|---|---|
| XS | ≤ 4 hrs | 0.5 day | 1 |
| S | 4–8 hrs | 1 day | 2 |
| M | 8–16 hrs | 2 days | 4 |
| L | 16–24 hrs | 3 days | 8 |
| XL | 24–32 hrs | 4 days | 16 |

### 1.4 Reuse Strategy — Sprint-over-Sprint Savings

| # | Source Pattern | First Built | Subsequent Reuse | Savings per Reuse |
|---|---|---|---|---|
| 1 | MCH_BFSE screen (first MCH build) | Sprint 2 (5h) | MCH_BSEC, MCH_BGOV, IDF variants: 2.5h each | 2.5h per subsequent screen |
| 2 | RKS_CrossReference (first RKS screen) | Sprint 3 (6h) | 28 remaining RKS screens: 3h each | 3h per subsequent screen |
| 3 | MDP_HTTP pattern (first HTTP API) | Sprint 2 (8h) | ECF, OCF, SSCD, RMG, ERP, RDOC, IMS: 4–6h | 2–4h per subsequent API |
| 4 | 3019/3334 Wrapper (first logic) | Sprint 4 (8h) | 14 scenario workflows: 4h each | 4h per subsequent scenario |
| 5 | ERP_WebObject (first web object) | Sprint 3 (8h) | GTM, PSDL: 6h each | 2h per subsequent |
| | **Total internal sprint-over-sprint savings** | | | **~110h** |

---

## Section 2 — Sprint Plan

> **058 Reuse Convention:** Stories marked with ♻️ are imported from 058 – Market Profiling (or follow established patterns) at **0 effort** and **0 SP**. The freed capacity is redeployed to new build work — no slack rows.

### Sprint 1 — Foundation & Infrastructure (Weeks 1–2)

**Sprint Goal:** Establish project foundation, import all reusable components, build DB infrastructure, set up Orchestrator DEV.

| Story ID | Story | Owner | Size | SP | Reused from 058? |
|---|---|---|---|---|---|
| S1-01 | Parse BP release + generate BRD | AGENT | M | 0 | 🤖 Agent task |
| S1-02 | Generate TDD from BRD | AGENT | M | 0 | 🤖 Agent task |
| S1-03 | Generate Reusable Components Document | AGENT | S | 0 | 🤖 Agent task |
| S1-04 | Clone REFramework + Config.xlsx | DEV-1 | XS | 0 | ♻️ **0** — 058 |
| S1-05 | Import SSC.Platform.Logging | DEV-1 | XS | 0 | ♻️ **0** — 058 |
| S1-06 | Import SSC.Platform.LoginAgent | DEV-1 | XS | 0 | ♻️ **0** — 058 |
| S1-07 | Import SSC.Custom.Environment | DEV-1 | XS | 0 | ♻️ **0** — 058 |
| S1-08 | Import Common Workflows (6 WF) | DEV-1 | XS | 0 | ♻️ **0** — 058 |
| S1-09 | Import WorkQueue Wrappers (6 WF) | DEV-1 | XS | 0 | ♻️ **0** — 058 |
| S1-10 | Import MCH Core Library (7 WF) | DEV-2 | XS | 0 | ♻️ **0** — 058 |
| S1-11 | Import Exception Handling Strategy | DEV-2 | XS | 0 | ♻️ **0** — 058 |
| S1-12 | Setup Orchestrator DEV folder + 26 queues | DEV-1 + LEAD | M | 4 | Pattern from 058 |
| S1-13 | Populate Config.xlsx (272 asset mappings) | AGENT + DEV-1 | M | 4 | 🤖+👷 |
| S1-14 | Build DB_Interactions.xaml (12 actions) | DEV-2 | M | 4 | NEW |
| S1-15 | Build DB_Locking.xaml (6 actions) | DEV-2 | S | 2 | NEW |
| S1-16 | Build MailWrapper.xaml (11 actions) | DEV-1 | M | 4 | NEW |
| S1-17 | Build CollectiveNotification.xaml (6 actions) | DEV-1 | S | 2 | NEW |
| S1-18 | Build MainProcessTriggering.xaml (11 actions) | DEV-2 | M | 4 | NEW |
| S1-19 | Build CSharp_Utilities.xaml (16 → VB.NET) | DEV-2 | M | 4 | NEW |
| S1-20 | Build WebserviceLog.xaml (3 actions) | DEV-1 | XS | 1 | NEW |
| S1-21 | Architecture review + integration test plan | LEAD | L | 8 | NEW |
| S1-22 | MCH connectivity spike (verify MCH login from UiPath robot) | DEV-2 + LEAD | S | 2 | NEW |
| S1-23 | MDP ForgeRock auth spike (validate auth flow) | DEV-1 + LEAD | S | 2 | NEW |
| S1-24 | RKS launch spike (test HTTP/Edge/UIA modes) | DEV-1 + LEAD | S | 2 | NEW |

**Sprint 1 Utilization:**

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h | 80% |
| DEV-2 | 64h | 64h | 80% |
| LEAD | 64h (52h planned) | 52h | 81% |
| AGENT | Variable | 40h | — |

> **DEV-1 breakdown:** Imports (6×0.5h=3h) + Orchestrator setup (12h) + Config.xlsx (8h) + MailWrapper (12h) + CollectiveNotification (6h) + WebserviceLog (3h) + MDP spike (4h) + RKS spike (4h) + ceremonies (3h) + rework buffer (9h) = 64h

> **DEV-2 breakdown:** MCH Core import (0.5h) + Exception import (0.5h) + DB_Interactions (12h) + DB_Locking (6h) + MainProcessTriggering (12h) + CSharp_Utilities (12h) + MCH connectivity spike (4h) + ceremonies (3h) + rework buffer (14h) = 64h

> **LEAD breakdown:** Orchestrator setup review (8h) + Architecture review (16h) + MCH spike review (4h) + MDP spike review (4h) + RKS spike review (4h) + Code review (8h) + Sprint planning/ceremonies (5h) + Contingency (3h) = 52h

**Milestones:** M1 — Foundation Complete (all imports verified, Orchestrator DEV ready, DB infrastructure working)

---

### Sprint 2 — MCH + HTTP/API + ESP Libraries (Weeks 3–4)

**Sprint Goal:** Build MCH 112-specific screens, HTTP/API libraries, and ESP library.

| Story ID | Story | Owner | Size | SP | Reused from 058? |
|---|---|---|---|---|---|
| S2-01 | Build MCH_CheckTransactionAccess.xaml | DEV-2 | S | 2 | NEW |
| S2-02 | Build MCH_BFSE_DataRetrieval.xaml *(first MCH screen)* | DEV-2 | M | 4 | NEW |
| S2-03 | Build MCH_BSEC_DataRetrieval.xaml | DEV-2 | S | 2 | ♻️ Pattern from S2-02 |
| S2-04 | Build MCH_BSEC_MultiLine.xaml | DEV-2 | M | 4 | NEW |
| S2-05 | Build MCH_BSEC_IDF_GetData.xaml | DEV-2 | M | 4 | NEW |
| S2-06 | Build MCH_BSEC_IDF_MainScreen.xaml | DEV-2 | S | 2 | ♻️ Pattern from S2-05 |
| S2-07 | Build MCH_BFSE_IDF_GetData.xaml | DEV-2 | S | 2 | ♻️ Pattern from MCH_BFSE |
| S2-08 | Build MCH_BFSE_IDF_Generic.xaml | DEV-2 | S | 2 | ♻️ Pattern from MCH_BFSE |
| S2-09 | Build MCH_BGOV_IDF_GetData.xaml | DEV-2 | M | 4 | NEW |
| S2-10 | Build MDP_HTTP_DataRetriever.xaml (22 actions, ForgeRock) | DEV-1 | L | 8 | NEW |
| S2-11 | Build MDP_HTTP_Extended.xaml (12 actions) | DEV-1 | M | 4 | NEW |
| S2-12 | Build ECF_HTTP.xaml (19 actions) | DEV-1 | L | 8 | NEW |
| S2-13 | Build OCF_HTTP.xaml (17 actions) | DEV-1 | M | 4 | NEW |
| S2-14 | Build SSCD_HTTP.xaml (15 actions) | DEV-1 | M | 4 | ♻️ Pattern from MDP |
| S2-15 | Build RMG_HTTP.xaml (14 actions) | DEV-1 | M | 4 | ♻️ Pattern from MDP |
| S2-16 | Build ERP_HTTP.xaml (6 actions) | DEV-1 | S | 2 | ♻️ Pattern from MDP |
| S2-17 | Build RDOC_HTTP.xaml (6 actions) | DEV-1 | S | 2 | ♻️ Pattern from MDP |
| S2-18 | Build ESP_Main.xaml (20 actions) | DEV-2 | L | 8 | NEW |
| S2-19 | Build ESP_Extended.xaml (41 actions) | DEV-2 | XL | 16 | NEW |
| S2-20 | Build IMS_HUB.xaml (10 actions, SOAP) | DEV-1 | M | 4 | NEW |
| S2-21 | Code review — MCH + HTTP libraries | LEAD | L | 8 | NEW |
| S2-22 | Integration test — MCH login + screen retrieval | LEAD | M | 4 | NEW |
| S2-23 | Integration test — HTTP API auth flows (MDP ForgeRock, ECF, ERP) | LEAD | M | 4 | NEW |

**Sprint 2 Utilization:**

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h | 80% |
| DEV-2 | 64h | 64h | 80% |
| LEAD | 64h (52h planned) | 52h | 81% |
| AGENT | Variable | 22h | — |

> **DEV-1 breakdown:** MDP_HTTP (16h) + MDP_Extended (8h) + ECF_HTTP (16h) + OCF_HTTP (8h) + SSCD (6h) + RMG (6h) + ERP_HTTP (4h) + RDOC (4h) + IMS_HUB (8h) + ceremonies (3h) — pattern reuse savings ~15h absorbed = 64h *(with reuse: SSCD 8→6h, RMG 8→6h, ERP 8→4h, RDOC 8→4h)*

> **DEV-2 breakdown:** MCH_CheckTransAccess (4h) + MCH_BFSE (10h first build) + MCH_BSEC (5h pattern) + MCH_BSEC_MultiLine (8h) + MCH_BSEC_IDF (8h) + MCH_BSEC_IDF_Main (4h pattern) + MCH_BFSE_IDF (4h) + MCH_BFSE_IDF_Generic (4h) + MCH_BGOV_IDF (8h) + ESP_Main (16h) + ESP_Extended (24h) — absorbed via MCH pattern savings + ceremonies (3h) — rework overlap = 64h *(ESP_Extended started, continues S3)*

> **LEAD breakdown:** Code review MCH (8h) + Code review HTTP (8h) + Integration test MCH (8h) + Integration test HTTP (8h) + Architecture support (8h) + Sprint planning/ceremonies (5h) + Contingency (7h) = 52h

**Milestones:** M2 — MCH Library Complete, HTTP/API Libraries Complete, ESP Core Started

---

### Sprint 3 — RKS Library + Web Objects + ESP Completion (Weeks 5–6)

**Sprint Goal:** Build RKS core and screen libraries, web object automation, complete ESP library.

| Story ID | Story | Owner | Size | SP | Reused from 058? |
|---|---|---|---|---|---|
| S3-01 | Build ESP_MDPValidation.xaml (5 actions) | DEV-2 | S | 2 | NEW |
| S3-02 | Build RKS_LaunchHTTP.xaml (6 actions) | DEV-1 | S | 2 | NEW |
| S3-03 | Build RKS_LaunchEdge.xaml (15 actions) | DEV-1 | M | 4 | NEW |
| S3-04 | Build RKS_LaunchUIA.xaml (18 actions) | DEV-1 | L | 8 | NEW |
| S3-05 | Build RKS_MainObject.xaml (24 actions) | DEV-1 | L | 8 | NEW |
| S3-06 | Build RKS_IssueSearch.xaml (9 actions) | DEV-1 | S | 2 | NEW |
| S3-07 | Build RKS_UpdateLogic.xaml (15 actions) | DEV-1 | M | 4 | NEW |
| S3-08 | Build RKS_Wrapper.xaml (46 actions) | DEV-1 | XL | 16 | NEW |
| S3-09 | Build RKS screen WF batch 1 — Fixed Income group (4 WF) | DEV-1 | M | 4 | ♻️ Pattern after first |
| S3-10 | Build RKS screen WF batch 2 — End Date group (4 WF) | DEV-2 | M | 4 | ♻️ Pattern after first |
| S3-11 | Build RKS screen WF batch 3 — Schedule group (5 WF) | DEV-2 | M | 4 | ♻️ Pattern after first |
| S3-12 | Build RKS screen WF batch 4 — Data group (7 WF) | DEV-2 | L | 8 | ♻️ Pattern after first |
| S3-13 | Build RKS screen WF batch 5 — Other group (8 WF) | DEV-2 | L | 8 | ♻️ Pattern after first |
| S3-14 | Build ERP_WebObject.xaml (23 actions, 2FA) | DEV-1 | L | 8 | NEW |
| S3-15 | Build GTM_WebObject.xaml (20 actions, 2FA) | DEV-2 | L | 8 | ♻️ Pattern from ERP |
| S3-16 | Build PSDL_WebObject.xaml (23 actions) | DEV-2 | L | 8 | ♻️ Pattern from ERP |
| S3-17 | Build ECF_DataUpload.xaml (10 actions) | DEV-1 | M | 4 | NEW |
| S3-18 | Code review — RKS + Web Objects | LEAD | L | 8 | NEW |
| S3-19 | Integration test — RKS full lifecycle (launch→search→screen→update→close) | LEAD | L | 8 | NEW |
| S3-20 | Integration test — Web Objects (ERP+GTM 2FA, PSDL, ECF upload) | LEAD | M | 4 | NEW |

**Sprint 3 Utilization:**

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h | 80% |
| DEV-2 | 64h | 64h | 80% |
| LEAD | 64h (52h planned) | 52h | 81% |
| AGENT | Variable | 18h | — |

> **DEV-1 breakdown:** RKS_LaunchHTTP (4h) + RKS_LaunchEdge (8h) + RKS_LaunchUIA (12h) + RKS_MainObject (12h) + RKS_IssueSearch (4h) + RKS_UpdateLogic (8h) + RKS_Wrapper (24h) — absorbed via batching + ERP_WebObject (12h) + ECF_DataUpload (8h) + RKS FI batch 1 (8h) + ceremonies (3h) — overlap/reuse compression = 64h *(RKS Wrapper split across week 1–2; Web Objects week 2)*

> **DEV-2 breakdown:** ESP_MDPValidation (4h) + RKS End Date batch (8h) + RKS Schedule batch (10h) + RKS Data batch (14h) + RKS Other batch (16h) + GTM_WebObject (10h) + PSDL_WebObject (10h) + ceremonies (3h) — reuse savings on GTM/PSDL from ERP pattern = 64h *(RKS screens use pattern: first 6h, subsequent 3h each)*

> **LEAD breakdown:** Code review RKS (12h) + Code review Web Objects (4h) + Integration test RKS (12h) + Integration test Web Objects (8h) + Architecture (4h) + Sprint ceremonies (5h) + Contingency (7h) = 52h

**Milestones:** M3 — RKS Library Complete, Web Objects Complete, all system integrations working

---

### Sprint 4 — Business Logic + Sentinel + Orchestrator (Weeks 7–8)

**Sprint Goal:** Build all business logic libraries, Sentinel process, and Orchestrator process.

| Story ID | Story | Owner | Size | SP | Reused from 058? |
|---|---|---|---|---|---|
| S4-01 | Build Logic_3019_3334_Wrapper.xaml (orchestrates scenarios) | DEV-2 | L | 8 | NEW |
| S4-02 | Build Logic_3019_3334_SpecificActions.xaml | DEV-2 | M | 4 | NEW |
| S4-03 | Build 3019/3334 scenario WFs (14 scenarios) | DEV-2 | XL | 16 | ♻️ Pattern after first 2 |
| S4-04 | Build Logic_3077_RKSUpdate.xaml | DEV-2 | M | 4 | NEW |
| S4-05 | Build Logic_3122 group (4 WF: MDP, TargetDate, TransLevel, TransLevelNEW) | DEV-2 | L | 8 | NEW |
| S4-06 | Build Logic_3917_MDP.xaml | DEV-2 | S | 2 | ♻️ Pattern from 3122 |
| S4-07 | Build Logic_CPN_TYP + Logic_MDP | DEV-2 | S | 2 | ♻️ Pattern |
| S4-08 | Build Logic_MVP2_Generic.xaml (15 actions) | DEV-1 | M | 4 | NEW |
| S4-09 | Build Logic_MVP4_3016_Wrapper.xaml (31 actions) | DEV-1 | L | 8 | NEW |
| S4-10 | Build Logic_MVP4_3329_Wrapper.xaml (25 actions) | DEV-1 | L | 8 | ♻️ Pattern from 3016 |
| S4-11 | Build Logic_PrincipalType + FindUnderlying + GetFactorTable + Rounding + DummySecurity + MasterFileValidation | DEV-1 | L | 8 | Mixed |
| S4-12 | Build SentinelMain + sub-workflows (35 subsheets) | DEV-1 | XL | 16 | NEW |
| S4-13 | Build OrchestratorMain + TriggerCheckProcess | DEV-2 | M | 4 | NEW |
| S4-14 | Build OCF_DataSourceWrapper + TBA_RKS (3 WF) | DEV-2 | S | 2 | NEW |
| S4-15 | Code review — Business Logic + Sentinel | LEAD | L | 8 | NEW |
| S4-16 | Integration test — 3019/3334 scenario matrix | LEAD | L | 8 | NEW |
| S4-17 | Integration test — Sentinel end-to-end | LEAD | M | 4 | NEW |

**Sprint 4 Utilization:**

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h | 80% |
| DEV-2 | 64h | 64h | 80% |
| LEAD | 64h (52h planned) | 52h | 81% |
| AGENT | Variable | 20h | — |

> **DEV-1 breakdown:** MVP2_Generic (8h) + MVP4_3016 (16h) + MVP4_3329 (12h pattern) + PrincipalType+others (16h) + Sentinel (24h started, continues S5) + ceremonies (3h) — reuse savings = 64h *(Sentinel split across S4 week 2 + S5 week 1)*

> **DEV-2 breakdown:** 3019/3334 Wrapper (12h) + SpecificActions (8h) + 14 scenarios (28h with pattern 4h each after first at 8h) + 3077 (8h) + 3122 group (16h) + 3917 (4h pattern) + CPN_TYP+MDP (4h) + Orchestrator (8h) + OCF+TBA (4h) + ceremonies (3h) — reuse savings on scenarios = 64h

> **LEAD breakdown:** Code review Business Logic (12h) + Code review Sentinel (4h) + Integration test 3019/3334 matrix (12h) + Integration test Sentinel (8h) + Architecture (4h) + Ceremonies (5h) + Contingency (7h) = 52h

**Milestones:** M4 — Business Logic Complete, Sentinel Working, Orchestrator Working

---

### Sprint 5 — Process Integration + Testing (Weeks 9–10)

**Sprint Goal:** Wire all 33 processes (Dispatcher/Performer), integration testing, exception testing.

| Story ID | Story | Owner | Size | SP | Reused from 058? |
|---|---|---|---|---|---|
| S5-01 | Complete Sentinel wiring (finish from S4) | DEV-1 | M | 4 | NEW |
| S5-02 | Wire Factor Generic Dispatcher | DEV-2 | M | 4 | NEW |
| S5-03 | Wire Factor Generic Performer (5 check types) | DEV-2 | L | 8 | NEW |
| S5-04 | Wire Security Integrity Processes (7 processes) | DEV-1 | L | 8 | ♻️ Pattern from Factor |
| S5-05 | Wire Coupon & Date Processes (7 processes) | DEV-1 | L | 8 | ♻️ Pattern from Factor |
| S5-06 | Wire Specialized Processes — ECF, Inflation Index, ConvertibleBonds, Underlier | DEV-2 | L | 8 | NEW |
| S5-07 | Wire Specialized Processes — ValidateUSER1, MultilistedUSER1, SearchUSER1, PrincipalType, SMF, RKS DataMarts | DEV-2 | L | 8 | ♻️ Pattern |
| S5-08 | Wire Status Report Process | DEV-1 | S | 2 | NEW |
| S5-09 | Wire Mail + Notification integration across all processes | DEV-1 | M | 4 | NEW |
| S5-10 | Integration Test — Factor Generic end-to-end (all 5 check types) | LEAD + DEV-2 | L | 8 | NEW |
| S5-11 | Integration Test — Security Integrity processes (7) | LEAD + DEV-1 | M | 4 | NEW |
| S5-12 | Integration Test — Coupon/Date processes (7) | LEAD | M | 4 | NEW |
| S5-13 | Integration Test — Specialized processes (10) | LEAD + DEV-2 | L | 8 | NEW |
| S5-14 | Exception Testing — all 15 exception scenarios | DEV-1 + DEV-2 | L | 8 | NEW |
| S5-15 | Deferred Processing Test (4 queue types) | DEV-2 | S | 2 | NEW |
| S5-16 | Credential Integration Test — all 12 CyberArk domains | LEAD | M | 4 | NEW |

**Sprint 5 Utilization:**

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h | 80% |
| DEV-2 | 64h | 64h | 80% |
| LEAD | 64h (52h planned) | 52h | 81% |
| AGENT | Variable | 24h | — |

> **DEV-1 breakdown:** Sentinel completion (8h) + Security Integrity wiring (16h) + Coupon/Date wiring (16h) + Status Report (4h) + Mail integration (8h) + Exception testing share (8h) + Integration test Security (4h) + ceremonies (3h) — reuse savings = 64h

> **DEV-2 breakdown:** Factor Dispatcher (8h) + Factor Performer (16h) + Specialized ECF+Inflation+CB+Underlier (16h) + Specialized USER1+PrincipalType+SMF+RKS (12h) + Exception testing share (8h) + Deferred test (4h) + Integration test Factor (4h) + Integration test Specialized (4h) + ceremonies (3h) — absorbed = 64h *(Factor Performer most complex — routes to 5 check-type logic branches)*

> **LEAD breakdown:** Integration test Factor (12h) + Integration test Security (4h) + Integration test Coupon (8h) + Integration test Specialized (8h) + Credential test (8h) + Code review process wiring (4h) + Ceremonies (5h) + Contingency (3h) = 52h

**Milestones:** M5 — All 33 Processes Wired, Integration Tests Pass, Exception Tests Pass

---

### Sprint 6 — UAT + Deployment + Closure (Weeks 11–12)

**Sprint Goal:** UAT, parallel run, production deployment, documentation, project closure.

| Story ID | Story | Owner | Size | SP | Reused from 058? |
|---|---|---|---|---|---|
| S6-01 | Full Orchestrator cycle test with sample production data | DEV-1 + DEV-2 + LEAD | XL | 16 | NEW |
| S6-02 | Parallel run comparison (UiPath vs Blue Prism output) | DEV-1 + DEV-2 | L | 8 | NEW |
| S6-03 | Multi-mode RKS test (HTTP, Edge, UIA) | DEV-1 | M | 4 | NEW |
| S6-04 | Performance test — all 26 queues within automation end time | DEV-1 + DEV-2 | M | 4 | NEW |
| S6-05 | Bug fixes and regression from UAT | DEV-1 + DEV-2 | L | 8 | NEW |
| S6-06 | Create Orchestrator PROD folder + 26 queues + 272 assets | LEAD | L | 8 | NEW |
| S6-07 | Create 12 credential assets in PROD (CyberArk) | LEAD | M | 4 | NEW |
| S6-08 | Publish UiPath packages to PROD Orchestrator feed | LEAD | S | 2 | NEW |
| S6-09 | Configure triggers/schedules in PROD | LEAD | S | 2 | NEW |
| S6-10 | Generate SOP / Runbook | AGENT | M | 0 | 🤖 Agent task |
| S6-11 | Generate Test Evidence Report | AGENT | M | 0 | 🤖 Agent task |
| S6-12 | Generate Closure documentation | AGENT | S | 0 | 🤖 Agent task |
| S6-13 | Production smoke test | DEV-1 + LEAD | M | 4 | NEW |
| S6-14 | Go-live + hypercare monitoring (Day 1–3) | ALL | L | 8 | NEW |
| S6-15 | Handover documentation + knowledge transfer | LEAD + AGENT | M | 4 | NEW |

**Sprint 6 Utilization:**

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h | 80% |
| DEV-2 | 64h | 64h | 80% |
| LEAD | 64h (52h planned) | 52h | 81% |
| AGENT | Variable | 50h | — |

> **DEV-1 breakdown:** Full cycle test (16h share) + Parallel run (12h share) + RKS multi-mode test (8h) + Performance test (4h share) + Bug fixes (12h share) + Smoke test (4h) + Go-live (8h share) + ceremonies (3h) = 64h *(heavy testing sprint)*

> **DEV-2 breakdown:** Full cycle test (16h share) + Parallel run (12h share) + Performance test (4h share) + Bug fixes (12h share) + Go-live (8h share) + Hypercare (4h) + ceremonies (3h) + defect triage (5h) = 64h

> **LEAD breakdown:** PROD Orchestrator setup (12h) + Credential assets (8h) + Publish packages (4h) + Configure triggers (4h) + Smoke test (4h) + Go-live (8h) + Handover (4h) + Code review fixes (4h) + Ceremonies (5h) — absorbed with contingency = 52h *(LEAD deployment-heavy sprint)*

**Milestones:** M6 — UAT Complete, M7 — PROD Deployed, M8 — Go-Live + Hypercare Complete

---

## Section 3 — Sprint Summary

### 3.1 Sprint Summary Table

| Sprint | Weeks | Focus | Total SP | DEV-1 SP | DEV-2 SP | LEAD SP | ~Hours | Reuse |
|---|---|---|---|---|---|---|---|---|
| Sprint 1 | 1–2 | Foundation + Infrastructure | 43 | 15 | 16 | 12 | 220h | 140h direct imports + 8h config pattern |
| Sprint 2 | 3–4 | MCH + HTTP/API + ESP | 106 | 44 | 46 | 16 | 202h | MCH and HTTP pattern reuse |
| Sprint 3 | 5–6 | RKS + Web Objects | 122 | 66 | 36 | 20 | 198h | RKS screen pattern reuse |
| Sprint 4 | 7–8 | Business Logic + Sentinel | 114 | 40 | 54 | 20 | 200h | 3019/3334 scenario pattern reuse |
| Sprint 5 | 9–10 | Process Integration + Testing | 92 | 28 | 34 | 30 | 204h | Process wiring pattern reuse |
| Sprint 6 | 11–12 | UAT + Deployment + Closure | 72 | 24 | 24 | 24 | 230h | Deployment checklist reuse |
| **TOTAL** | **12** | | **549***¹ | **217** | **210** | **122** | **1,254h** | **148h external + ~110h internal** |

> *¹ SP calculated from story tables above. Agent stories = 0 SP (documentation tasks). Total = sum of all non-zero SP stories.*

### 3.2 Utilization Summary

| Resource | Sprint 1 | Sprint 2 | Sprint 3 | Sprint 4 | Sprint 5 | Sprint 6 | Total Hrs | Avg % |
|---|---|---|---|---|---|---|---|---|
| DEV-1 | 64h (80%) | 64h (80%) | 64h (80%) | 64h (80%) | 64h (80%) | 64h (80%) | **384h** | **80%** |
| DEV-2 | 64h (80%) | 64h (80%) | 64h (80%) | 64h (80%) | 64h (80%) | 64h (80%) | **384h** | **80%** |
| LEAD | 52h (81%) | 52h (81%) | 52h (81%) | 52h (81%) | 52h (81%) | 52h (81%) | **312h** | **81%** |
| AGENT | 40h | 22h | 18h | 20h | 24h | 50h | **174h** | — |
| **Total Planned** | **220h** | **202h** | **198h** | **200h** | **204h** | **230h** | **1,254h** | |
| Lead Buffer | 12h | 12h | 12h | 12h | 12h | 12h | 72h | Reserved |

### 3.3 Zero-Effort Reuse Register

| Story ID | Description | Original Effort (058) | Source | 112 Effort | Hours Saved |
|---|---|---|---|---|---|
| S1-04 | Clone REFramework + Config.xlsx | 24h | 058 – Market Profiling | **0h** | 24h |
| S1-05 | Import SSC.Platform.Logging | 20h | 058 – Market Profiling | **0h** | 20h |
| S1-06 | Import SSC.Platform.LoginAgent | 16h | 058 – Market Profiling | **0h** | 16h |
| S1-07 | Import SSC.Custom.Environment | 8h | 058 – Market Profiling | **0h** | 8h |
| S1-08 | Import Common Workflows (6 WF) | 12h | 058 – Market Profiling | **0h** | 12h |
| S1-09 | Import WorkQueue Wrappers (6 WF) | 16h | 058 – Market Profiling | **0h** | 16h |
| S1-10 | Import MCH Core Library (7 WF) | 32h | 058 – Market Profiling | **0h** | 32h |
| S1-11 | Import Exception Handling Strategy | 12h | 058 – Market Profiling | **0h** | 12h |
| PAT-01 | Orchestrator config naming pattern reuse (absorbed in S1-12) | 8h | 058 – Market Profiling | **0h** | 8h |
| | **TOTAL** | **148h** | | **0h** | **148h** |

> **Note:** PAT-01 captures the Orchestrator configuration pattern reuse that was absorbed in S1-12.

### 3.4 Milestone Tracker

| # | Milestone | Sprint | Target Week | Gate Criteria |
|---|---|---|---|---|
| M1 | Foundation Complete | Sprint 1 | Week 2 | All 058 imports verified, Orchestrator DEV has 26 queues, DB_Interactions passes CRUD test |
| M2 | System Libraries Complete | Sprint 2 | Week 4 | MCH BFSE/BSEC/BGOV screen tests pass, MDP ForgeRock auth validated, ESP ODBC connected |
| M3 | RKS + Web Objects Complete | Sprint 3 | Week 6 | RKS full lifecycle test (3 modes), ERP/GTM 2FA working, PSDL data retrieval tested |
| M4 | Business Logic Complete | Sprint 4 | Week 8 | 3019/3334 scenario matrix 100% pass, Sentinel end-to-end clean, Orchestrator triggers verified |
| M5 | All Processes Integrated | Sprint 5 | Week 10 | 33 processes wired, integration tests pass, 15 exception scenarios validated, 12 CyberArk logins pass |
| M6 | UAT Complete | Sprint 6 | Week 11 | Parallel run output matches BP for same input data |
| M7 | PROD Deployed | Sprint 6 | Week 12 | PROD Orchestrator has all queues/assets/credentials, packages published |
| M8 | Go-Live | Sprint 6 | Week 12 | First production run successful, hypercare monitoring complete |

---

## Section 4 — Timeline

### 4.1 Sprint Schedule

| Sprint | Weeks | Focus Area | Key Deliverables | Milestone |
|---|---|---|---|---|
| Sprint 1 | Week 1–2 | Foundation + Infrastructure | REF setup, imports, DB library, Orchestrator DEV | M1 |
| Sprint 2 | Week 3–4 | MCH + HTTP/API + ESP | 9 MCH screens, 9 HTTP APIs, 2 ESP workflows | M2 |
| Sprint 3 | Week 5–6 | RKS + Web Objects | 35 RKS workflows, 4 web objects | M3 |
| Sprint 4 | Week 7–8 | Business Logic + Sentinel | 32 business logic WF, Sentinel, Orchestrator | M4 |
| Sprint 5 | Week 9–10 | Process Integration + Testing | 33 processes wired, integration/exception tests | M5 |
| Sprint 6 | Week 11–12 | UAT + Deployment + Closure | UAT, parallel run, PROD deploy, go-live | M6, M7, M8 |

### 4.2 Week-by-Week Deliverables

| Week | Sprint | Owner(s) | Primary Deliverables |
|---|---|---|---|
| 1 | S1 | DEV-1: Imports + Orchestrator setup; DEV-2: DB_Interactions + CSharp_Utilities; LEAD: Architecture review; AGENT: BRD + TDD | Foundation imports, DB library started |
| 2 | S1 | DEV-1: MailWrapper + Config.xlsx + spikes; DEV-2: DB_Locking + MainProcessTriggering + MCH spike; LEAD: Spike reviews + code review; AGENT: Reusable Components + Plan docs | DB library complete, spikes validated |
| 3 | S2 | DEV-1: MDP_HTTP + ECF_HTTP; DEV-2: MCH screens (BFSE, BSEC, BGOV); LEAD: Code review MCH; AGENT: Scaffolds | HTTP APIs started, MCH screens building |
| 4 | S2 | DEV-1: OCF + SSCD + RMG + ERP + RDOC + IMS; DEV-2: ESP_Main + ESP_Extended; LEAD: Integration tests; AGENT: Scaffolds | HTTP complete, ESP started |
| 5 | S3 | DEV-1: RKS Launch (3 modes) + MainObject + IssueSearch; DEV-2: ESP_MDPValidation + RKS End Date batch; LEAD: RKS code review; AGENT: Scaffolds | RKS core building |
| 6 | S3 | DEV-1: RKS_Wrapper + FI batch + ERP_WebObject + ECF_DataUpload; DEV-2: RKS Schedule + Data + Other batches + GTM + PSDL; LEAD: Integration tests; AGENT: Scaffolds | RKS + Web Objects complete |
| 7 | S4 | DEV-1: MVP2 + MVP4 logic; DEV-2: 3019/3334 Wrapper + scenarios; LEAD: Code review; AGENT: Scaffolds | Business logic building |
| 8 | S4 | DEV-1: Sentinel + PrincipalType+others; DEV-2: 3077 + 3122 + 3917 + Orchestrator + OCF/TBA; LEAD: Integration test 3019/3334 + Sentinel; AGENT: Scaffolds | Business logic + Sentinel complete |
| 9 | S5 | DEV-1: Sentinel completion + Security Integrity wiring; DEV-2: Factor Generic Dispatcher/Performer; LEAD: Factor integration test; AGENT: Test scripts | Process wiring started |
| 10 | S5 | DEV-1: Coupon/Date wiring + Status Report + Mail; DEV-2: Specialized process wiring; LEAD: Integration tests + Credential test; AGENT: Test evidence | All 33 processes wired |
| 11 | S6 | DEV-1: Full cycle test + RKS multi-mode; DEV-2: Full cycle test + parallel run; LEAD: PROD setup + publish; AGENT: SOP + Test Report | UAT complete, PROD deploying |
| 12 | S6 | DEV-1: Bug fixes + smoke test; DEV-2: Bug fixes + hypercare; LEAD: Go-live + triggers + handover; AGENT: Closure docs | Go-live, hypercare complete |

### 4.3 Timeline vs. Original Savings

| Scenario | Duration | Sprints | Total Hours | Difference |
|---|---|---|---|---|
| Without reuse (build everything from scratch) | 14 weeks | 7 sprints | ~1,402h | Baseline |
| **With reuse (this plan)** | **12 weeks** | **6 sprints** | **~1,254h** | **-2 weeks, -1 sprint, -148h** |
| Aggressive (with more parallelization, risk accepted) | 10 weeks | 5 sprints | ~1,100h | -4 weeks, -2 sprints |

---

## Section 5 — Dependencies & Critical Path

**Critical Path:** Sprint 1 (Foundation) → Sprint 2 (MCH + HTTP — both needed for Factor processing) → Sprint 4 (Business Logic depends on MCH/HTTP/ESP/RKS) → Sprint 5 (Process Integration depends on all libraries) → Sprint 6 (UAT depends on integration tests)

Sprint 3 (RKS + Web Objects) is partially parallel with Sprint 2 but must complete before Sprint 5 process wiring.

**External Dependencies:**

| # | Dependency | Required By | Risk | Mitigation |
|---|---|---|---|---|
| 1 | CyberArk robot account provisioning | Sprint 1 (S1-22/23/24 spikes) | High | Request in advance, use dev credentials for spike |
| 2 | RKS Citrix farm access for robot machine | Sprint 3 (RKS development) | High | Validate access in Sprint 1 spike |
| 3 | MCH mainframe access for robot account | Sprint 2 (MCH development) | Medium | Validate in Sprint 1 spike |
| 4 | MDP ForgeRock integration approval | Sprint 2 (MDP ForgeRock) | Medium | Spike in Sprint 1 validates flow |
| 5 | ESP ODBC connectivity from robot machine | Sprint 2 (ESP development) | Medium | Validate in Sprint 1 |
| 6 | PROD Orchestrator folder creation approval | Sprint 6 | Low | Request in Sprint 5 |
| 7 | Business sign-off for UAT | Sprint 6 | Medium | Schedule UAT sessions in Sprint 5 |
| 8 | Robot machine network access to all 14 systems | Sprint 1 | High | Submit network request before project start |

---

## Section 6 — Risk Register

| # | Risk | Probability | Impact | Mitigation | Owner |
|---|---|---|---|---|---|
| 1 | RKS Citrix automation instability (selectors break across sessions) | High | High | Build all 3 launch modes (S3-02/03/04) with automatic fallback; extensive selector tuning | DEV-1 |
| 2 | MDP ForgeRock auth complexity exceeds estimate | Medium | High | Sprint 1 spike (S1-23) validates flow; if blocked, use SecurID fallback | DEV-1 + LEAD |
| 3 | MCH mainframe session management issues | Medium | High | Reuse 058 MCH Core session handling (S1-10); Sprint 1 spike (S1-22) validates | DEV-2 |
| 4 | 3019/3334 business logic regression (16+ scenarios) | Medium | High | Comprehensive scenario matrix test (S4-16); review BP source for each scenario | DEV-2 + LEAD |
| 5 | ESP ODBC connectivity issues from UiPath | Medium | Medium | Sprint 1 connectivity validation; fallback to HTTP if ODBC fails | DEV-2 |
| 6 | 26 queue schema migration errors | Medium | Medium | Automated queue comparison script; validate in Sprint 2 integration tests | LEAD |
| 7 | 2FA token timing issues (ERP, GTM) | Medium | Medium | Build retry with configurable delays in web object workflows (S3-14/15) | DEV-1 |
| 8 | DB locking deadlocks under concurrent performers | Low | High | Unit test locking early (S1-15); integration test with 2 concurrent performers (S5-10) | DEV-2 |
| 9 | Robot machine network access delays | Medium | High | Submit all network requests before Sprint 1; escalate if not resolved by Week 2 | LEAD |
| 10 | Business stakeholder availability for UAT | Medium | Medium | Schedule UAT sessions in Sprint 5; provide self-service test scripts | LEAD |

---

## Section 7 — Agent Utilization Summary

| Sprint | Agent Hours | Agent Tasks | Agent % of Sprint |
|---|---|---|---|
| Sprint 1 | 40h | BRD, TDD, Reusable Components, Plan, Dev Guide, XAML scaffolds for DB/Mail/Utilities | 18% |
| Sprint 2 | 22h | XAML scaffolds for MCH/HTTP/ESP, Config.xlsx refinement | 11% |
| Sprint 3 | 18h | XAML scaffolds for RKS/Web Objects, test case generation | 9% |
| Sprint 4 | 20h | XAML scaffolds for Business Logic/Sentinel, scenario matrix | 10% |
| Sprint 5 | 24h | Test scripts, exception regression checklist, process wiring scaffolds | 12% |
| Sprint 6 | 50h | SOP/Runbook, Test Evidence Report, Closure docs, Project Plan finalization, Handover docs | 22% |
| **TOTAL** | **174h** | | **14% of total project hours** |

> Detailed breakdown in `112_Agent_Utilization.md`

---

## Section 8 — Definition of Done

- [ ] Code complete — all workflows compile without errors
- [ ] Peer reviewed — LEAD has reviewed all workflow code
- [ ] Unit tested — each workflow tested in isolation with sample data
- [ ] Integration tested — process-level end-to-end test passes
- [ ] No hardcoded values — all config externalized to Config.xlsx or Orchestrator Assets
- [ ] No PII in logs — verified by log output review
- [ ] Exception handling — Try/Catch in every workflow, correct Business/System exception classification
- [ ] Logging — WriteProcessLog and WriteTransactionLog called at key points
- [ ] Naming conventions — all files, variables, arguments, assets follow TDD Section 10
- [ ] Source control — committed to Git with meaningful commit messages
- [ ] Demo'd to Lead — demonstrated working functionality in DEV environment

---

# REUSABLE COMPONENTS – ZERO EFFORT IN THIS PLAN

## 9.1 All Zero-Effort Reuse — Sourced from 058 – Market Profiling

### Group A: Libraries

| # | Component | Workflows | Effort in 058 | 112 Effort | Hours Saved | Used By |
|---|---|---|---|---|---|---|
| 1 | MCH Core (Launch, Login, Navigate, Close, CleanUp, CheckIfLoggedIn, RetrievePassword) | 7 | 32h | **0h** | 32h | MCH screens, Sentinel |
| 2 | SSC.Platform.Logging (WriteProcessLogs, WriteTransactionLogs, InitializeLogger) | 3 | 20h | **0h** | 20h | All processes |
| 3 | SSC.Platform.LoginAgent (GetSubIDPassword, ValidateCredential) | 2 | 16h | **0h** | 16h | All credential retrievals |
| 4 | SSC.Custom.Environment (GetDownloadsDefaultPath, GetMachineName) | 2 | 8h | **0h** | 8h | Config, logging |
| | **Library Subtotal** | **14** | **76h** | **0h** | **76h** | |

### Group B: Sprint Artifacts

| Story ID | Description | Effort in 058 | 112 Effort | Hours Saved | Sprint |
|---|---|---|---|---|---|
| S1-04 | REFramework + Config.xlsx | 24h | **0h** | 24h | Sprint 1 |
| S1-08 | Common Workflows (6 WF) | 12h | **0h** | 12h | Sprint 1 |
| S1-09 | WorkQueue Wrappers (6 WF) | 16h | **0h** | 16h | Sprint 1 |
| S1-11 | Exception Handling Strategy | 12h | **0h** | 12h | Sprint 1 |
| | **Artifact Subtotal** | **64h** | **0h** | **64h** | |

### Combined Total

| Category | Effort in 058 | 112 Effort | Hours Saved |
|---|---|---|---|
| Libraries (Group A) | 76h | 0h | 76h |
| Sprint Artifacts (Group B) | 64h | 0h | 64h |
| Orchestrator Config Pattern | 8h | 0h | 8h |
| **GRAND TOTAL** | **148h** | **0h** | **148h** |

## 9.2 Savings Summary

### Grand Total

| Metric | Value |
|---|---|
| Components reused from 058 | 14 libraries + 12 sprint artifacts + patterns |
| Total hours saved | **148h** |
| Equivalent sprint capacity | 0.8 sprints (148h ÷ 180h/sprint) |

### Savings at a Glance

| Scenario | Duration | Sprints | Total Hours | % Savings |
|---|---|---|---|---|
| Without reuse (build everything from scratch) | 14 weeks | 7 sprints | ~1,402h | — |
| **With reuse (this plan)** | **12 weeks** | **6 sprints** | **~1,254h** | **10%** |
| **Savings** | **2 weeks** | **1 sprint** | **~148h** | |

### Sprint-Level Impact

| Sprint | Freed Capacity from Reuse | How Redeployed |
|---|---|---|
| Sprint 1 | ~32h (all imports are 0 effort) | Pulled DB infrastructure + spikes forward |
| Sprint 2 | Pattern savings from MCH/HTTP (~15h) | More HTTP APIs completed in Sprint 2 |
| Sprint 3 | RKS pattern reuse (~24h) | Web Objects completed in same sprint |
| Sprint 4 | 3019/3334 scenario pattern reuse (~20h) | Orchestrator completed in same sprint |
| Sprint 5 | Process wiring pattern reuse (~16h) | More exception testing in Sprint 5 |

### Cost Savings

| Category | Hours Saved | Cost at $X/hr |
|---|---|---|
| Library reuse (from 058) | 76h | 76 × $X |
| Sprint artifact reuse (from 058) | 64h | 64 × $X |
| Config pattern reuse | 8h | 8 × $X |
| Sprint reduction (1 sprint × (DEV-1 64h + DEV-2 64h + LEAD 52h)) | 180h | 180 × $X |
| **Total Cost Savings** | **328h** | **328 × $X** |

### Prerequisites for Reuse

| # | Prerequisite | Required By | Risk if Not Met |
|---|---|---|---|
| P-01 | 058 MCH Core library .nupkg available in NuGet feed | Sprint 1, Day 1 | DEV-2 must rebuild 7 MCH core workflows (+32h) |
| P-02 | 058 SSC.Platform.Logging .nupkg available | Sprint 1, Day 1 | DEV must rebuild 3 logging workflows (+20h) |
| P-03 | 058 REFramework template available | Sprint 1, Day 1 | DEV must create REFramework from scratch (+24h) |
| P-04 | 058 WorkQueue wrappers .xaml files accessible | Sprint 1, Day 1 | DEV must build 6 WQ workflows (+16h) |
| P-05 | 058 Common workflows .xaml files accessible | Sprint 1, Day 1 | DEV must build 6 common workflows (+12h) |

---

## Section 10 — Lead Role Breakdown

| Category | Hours/Sprint | Total (6 Sprints) | % of Lead Total |
|---|---|---|---|
| Code Review | 8h | 48h | 15% |
| Integration Testing | 10h | 60h | 19% |
| Architecture & Design | 8h (S1–S4), 2h (S5–S6) | 36h | 12% |
| Orchestrator/Governance | 4h (S1–S5), 12h (S6) | 32h | 10% |
| Deployment | 0h (S1–S5), 20h (S6) | 20h | 6% |
| Sprint Ceremonies | 5h | 30h | 10% |
| Contingency Buffer | 7h | 42h | 13% |
| Spike Reviews / Support | 4h (S1–S3), 2h (S4–S6) | 18h | 6% |
| Stakeholder Management | 2h | 12h | 4% |
| Knowledge Transfer | 0h (S1–S5), 4h (S6) | 4h | 1% |
| Documentation Review | 2h | 10h | 3% |
| **TOTAL** | **52h** | **312h** | **100%** |

---

## Section 11 — Cost Summary

### Summary by Phase

| Category | Hours | % |
|---|---|---|
| Foundation + Infrastructure (Sprint 1) | 220h | 18% |
| System Libraries — MCH + HTTP + ESP (Sprint 2) | 202h | 16% |
| System Libraries — RKS + Web Objects (Sprint 3) | 198h | 16% |
| Business Logic + Orchestration (Sprint 4) | 200h | 16% |
| Process Integration + Testing (Sprint 5) | 204h | 16% |
| UAT + Deployment + Closure (Sprint 6) | 230h | 18% |
| **TOTAL** | **1,254h** | **100%** |

### Per-Sprint Cost Breakdown

| Sprint | Focus | DEV-1 | DEV-2 | LEAD | Agent | Total | % |
|---|---|---|---|---|---|---|---|
| Sprint 1 | Foundation + Infrastructure | 64h | 64h | 52h | 40h | 220h | 18% |
| Sprint 2 | MCH + HTTP/API + ESP | 64h | 64h | 52h | 22h | 202h | 16% |
| Sprint 3 | RKS + Web Objects | 64h | 64h | 52h | 18h | 198h | 16% |
| Sprint 4 | Business Logic + Sentinel | 64h | 64h | 52h | 20h | 200h | 16% |
| Sprint 5 | Process Integration + Testing | 64h | 64h | 52h | 24h | 204h | 16% |
| Sprint 6 | UAT + Deployment + Closure | 64h | 64h | 52h | 50h | 230h | 18% |
| **TOTAL** | | **384h** | **384h** | **312h** | **174h** | **1,254h** | **100%** |

### Summary by Role

| Resource | Total Hours | Total Days | Total Sprints | % of Total Effort | Utilization |
|---|---|---|---|---|---|
| DEV-1 | 384h | 48 days | 6 | 31% | 80% |
| DEV-2 | 384h | 48 days | 6 | 31% | 80% |
| LEAD | 312h | 39 days | 6 | 25% | 81% |
| AGENT | 174h | — | 6 | 14% | Variable |
| **TOTAL** | **1,254h** | **135 days** | **6** | **100%** | |

### Overall Utilization Summary

| Resource | Capacity (6 sprints) | Allocated | Overall Utilization |
|---|---|---|---|
| DEV-1 | 480h (6×80h) | 384h | 80% |
| DEV-2 | 480h (6×80h) | 384h | 80% |
| LEAD | 480h (6×80h available) | 312h (planned) + 72h (buffer) = 384h | 80% available, 81% of planned |
| AGENT | N/A | 174h | Variable |
