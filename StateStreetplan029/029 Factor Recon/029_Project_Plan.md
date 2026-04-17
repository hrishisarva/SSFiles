# 029 – PROJECT PLAN

| Field | Value |
|---|---|
| **Document ID** | 029-PP-001 |
| **Project** | BOAT.029 – COB FACTOR RECON |
| **Conversion** | Blue Prism → UiPath |
| **Date** | 2026-04-08 |
| **Source** | 029_Plan.md + 029_Development_Guide.md |
| **Status** | Draft v1.0 |
| **Methodology** | Agile sprints (2 weeks each), phase-gated with continuous testing |
| **Duration** | **8 weeks / 4 sprints / ~776 hours** |
| **Sprint Duration** | 2 weeks (10 working days) |
| **Team** | 2 Developers (80%), 1 Lead (80%), 1 Agent (AI) |

> **Assumption:** Reusable components (SSC.Platform.Logging, SSC.Platform.LoginAgent, SSC.Custom.Environment, BOAT.COB.Excel.Utilities) were **built as part of 058 – COB Market Profiling** and are fully available to 029 at zero cost. Sprint artifacts (REFramework template, Common workflows, WorkQueue wrappers, Orchestrator config) are cloned from 057 – Ben Profiling. MCH Library login pattern is reused from 058; project-specific MCH screens (BGMM, BGIR, BSEC, BPOP, PYDN, PYUP, PYDA) are **new builds**. This plan covers **project-specific effort only** for BOAT.029.

**Duration: 8 weeks / 4 sprints / ~776 hours**

---

## Section 1 — Team Capacity

### 1.1 Resource Allocation

| Role | ID | Utilization | Hours/Sprint (2 weeks) | Effective Hours/Sprint |
|---|---|---|---|---|
| Developer 1 | DEV-1 | 80% | 80h | 64h |
| Developer 2 | DEV-2 | 80% | 80h | 64h |
| Lead | LEAD-1 | 80% | 80h | 64h (available) |
| Agent (AI) | AGENT-1 | Variable | N/A | See Section 7 |
| **Total Human** | | | | **180 hrs/sprint (planned work)** |

#### Team Roster — Expertise & Responsibilities

| Role | ID | Expertise | Rate Assumption | Key Responsibilities |
|---|---|---|---|---|
| **Lead** | LEAD-1 | Strong BP knowledge, medium UiPath, MCH mainframe expert | Senior ($X/hr) | Architecture, **active development** (~52h/sprint), code review, MCH selector validation, integration testing oversight, deployment, stakeholder coordination |
| **Developer 1** | DEV-1 | Medium UiPath, mainframe terminal UI, BP object conversion | Mid-level ($X/hr) | MCH Library (login/BGMM/BGIR/BSEC/PYDN), MasterScheduler Library, DataTransformation, TransformPosting performer, unit testing |
| **Developer 2** | DEV-2 | Medium UiPath, web services, DLL integration | Mid-level ($X/hr) | MCH Library (BPOP/PYUP/PYDA), Database Library, Accounting_WS Library (RDR/Bloomberg), Posting Library, DataExtraction performer |
| **AI Agent** | AGENT-1 | Code generation, documentation | Tool | XAML scaffold generation, VB.Net expressions, Config.xlsx, queue wrappers, BRD/TDD/documentation, test case support |

#### Work Assumptions

| Parameter | Value |
|---|---|
| Working hours/day | 8 productive hours |
| Working days/week | 5 |
| Sprint duration | **2 weeks (10 working days)** |
| Hours/sprint (gross) | **80 hours per person** |
| **Utilization target** | **80% = 64 hours per person per sprint — ALL roles** |
| LEAD planned work | **~52h/sprint** (~81% utilization); remaining 12h/sprint = contingency buffer |
| Agent generation speed | ~15 min per simple workflow, ~30 min per complex workflow |
| Developer build-from-scaffold speed (trivial) | 0.5–1 hour (import, verify arguments) |
| Developer build-from-scaffold speed (simple) | 1–1.5 hours (wire arguments, add Try/Catch) |
| Developer build-from-scaffold speed (moderate) | 2–3 hours (selectors, conditional logic, data tables) |
| Developer build-from-scaffold speed (complex) | 4–6 hours (MCH screens, DLL integration, multi-step posting) |
| MCH screen workflows — first 3 (BGMM, BGIR, BSEC) | 5h each (first build — selector capture + validation) |
| MCH screen workflows — remaining 4 (BPOP, PYDN, PYUP, PYDA) | **3h each ♻️ pattern reuse from first 3** |
| MCH posting screens (PYDN/PYUP/PYDA) | +4h each additional for posting logic validation |
| Database workflows (5 DB Library) | 2.5h each (SQL query + connection string from Config.xlsx) |
| Selector capture (batch sessions) | **12h total (2 sessions: MCH terminal + Bloomberg/RDR web)** |
| Lead as active developer | **~52h/sprint (208h across project — architecture + code review + active testing + oversight)** |
| Code review time (Lead) | 0.5–1 hour per workflow |
| Sprint ceremonies | **4h per sprint (planning 1h, demo 1h, retro 0.5h, grooming 0.5h, standups 1h) — kept minimal** |
| Rework budget | 10% of build hours per sprint (absorbed into LEAD buffer) |

> **Lead capacity note:** Lead is allocated at **80% = 64h available per sprint**, with **~52h of planned work** assigned per sprint (**~81% utilization**). The remaining **~12h/sprint is contingency buffer** — absorbs unexpected issues, peer reviews, stakeholder queries. More capacity = lower risk, NOT higher cost. Project effort stays ~776h.

### 1.2 Project Totals

| Metric | Value |
|---|---|
| **Total Sprints** | **4** |
| **Total Duration** | **8 weeks** |
| **Total Human Capacity** | Lead 64h + DEV-1 64h + DEV-2 64h = **192h available/sprint** |
| **Planned Work (Effort)** | DEV-1(244) + DEV-2(244) + LEAD(208) + Agent(80) = **776 hrs** |
| **Estimated Effort** | **~776 hrs** — Lead has 12h/sprint buffer (64h available − 52h planned) |
| **057/058 Reuse Savings** | **~160 hrs / 1 sprint eliminated** |
| **Total Story Points** | **~200 SP** (excludes 0-effort reused stories) |
| **Target Velocity** | ~50 SP/sprint |
| **Start Date** | 2026-04-08 |
| **End Date** | 2026-06-03 |

> **057/058 Reuse:** 4 stories from 057 (Ben Profiling) and 058 (Market Profiling) are directly reused with **0 effort** — REFramework template, NuGet libraries, Common workflows, WorkQueue wrappers. This compresses the project from 5 sprints to **4 sprints**, saving **2 weeks and ~160 hours**.

### 1.3 T-Shirt Sizing Reference

| Size | Hours | Days | Story Points |
|---|---|---|---|
| XS | ≤ 4h | 0.5 day | 1 |
| S | 4–8h | 1 day | 2 |
| M | 8–16h | 2 days | 4 |
| L | 16–24h | 3 days | 8 |
| XL | 24–32h | 4 days | 16 |

### 1.4 Reuse Strategy — Sprint-over-Sprint Savings

> Every pattern established in an earlier sprint is reused in later sprints at reduced or zero effort. This is the primary mechanism for maintaining a tight 4-sprint schedule without quality loss. Cross-project reuse from 058/057 saves 160h upfront; internal pattern reuse within 029 saves an additional ~40h.

| Source Pattern (Sprint) | Reused In (Sprint) | Original Effort | Reused Effort | Savings |
|---|---|---|---|---|
| MCH_Login/Logout/Check patterns from 058 (imported S1) | All 029 MCH screen workflows (S1–S2) | 20h | **0h** (import) | 20h saved |
| MCH_BGMM + BGIR screens (S1) — first 2 screens built | MCH_BSEC + BPOP screens (S2) | 5h each | **3h each** ♻️ pattern reuse | 4h saved |
| MCH_BSEC posting pattern (S2) | MCH_PYDN, PYUP, PYDA posting screens (S2) | 8h each | **6h each** ♻️ posting pattern | 6h saved |
| MCH_PYDN posting screen (S2) | PYUP + PYDA variants (S2) | 20h each | **15h each** ♻️ same structure, different screen code | 10h saved |
| Dispatcher_MainScheduler orchestrator (S2) | Dispatcher_Adhoc + Dispatcher_NOVAL (S4) | 14h each | **4h each** ♻️ queue setup already wired | 20h saved |
| REFramework template from 057 (imported S1) | All 029 processes (S1–S4) | 24h | **0h** (clone) | 24h saved |
| Common workflows from 057 (imported S1) | All 029 processes (S1–S4) | 32h | **0h** (import) | 32h saved |
| WorkQueue wrappers from 057 (imported S1) | All 029 queue operations (S1–S4) | 28h | **0h** (import) | 28h saved |
| 4 NuGet libraries from 058 (imported S1) | All 029 processes (S1–S4) | 68h | **0h** (import) | 68h saved |
| Posting Library standard patterns (S3) | UMAS variant patterns (S3) | 20h each | **12h each** ♻️ same structure, DLL wrapper different | 8h saved |
| Unit test harness from S1–S3 | Integration test setup in S4 | 8h | **0h** ♻️ reuse | 8h saved |
| **Total Reuse Savings** | | | | **~200h** (160h cross-project + ~40h internal pattern) |

---

## Section 2 — Sprint Plan

> **057/058 Reuse Convention:** Stories marked `:recycle: **0 — Reused from 057/058**` are cloned directly from previous projects. They require **0 development effort** — import only. The freed capacity allows compressing 5 sprints → 4 sprints.

### Sprint 1 — Weeks 1–2 (2026-04-08 to 2026-04-22)
**Sprint Goal:** Project scaffold complete; all reusable components imported; BRD/TDD signed off; MCH Library login workflows and DB Library built.

| Story ID | Story | Owner | Size | SP | Reused from 057? |
|---|---|---|---|---|---|
| S1-01 | Clone REFramework from 057; create 029 project scaffold | DEV-1 | XS | :recycle: **0** | **YES — 057 – Ben Profiling** |
| S1-02 | Import 4 NuGet libraries (Logging, LoginAgent, Environment, Excel.Utilities) | DEV-1 | XS | :recycle: **0** | **YES — 058 – Market Profiling** |
| S1-03 | Import Common workflows (8 files) from 057 | DEV-2 | XS | :recycle: **0** | **YES — 057 – Ben Profiling** |
| S1-04 | Import WorkQueue wrapper workflows (9 files) from 057 | DEV-2 | XS | :recycle: **0** | **YES — 057 – Ben Profiling** |
| S1-05 | Configure all 28 Orchestrator Assets in DEV Orchestrator | LEAD-1 | S | 2 | YES — 057 pattern |
| S1-06 | Configure 2 Orchestrator Queues in DEV Orchestrator | LEAD-1 | XS | 1 | YES — 057 pattern |
| S1-07 | BRD review and sign-off | LEAD-1 | XS | 1 | – |
| S1-08 | TDD review and sign-off | LEAD-1 | S | 2 | – |

**Sprint 1 — Detailed Task Breakdown:**

| Task | Owner | Agent h | Dev h | Lead h | Reuse Note | Total |
|---|---|---|---|---|---|---|
| **Foundation — Scaffold** | | | | | | |
| Clone REFramework + project.json from 057 | AGENT → DEV-1 | 0.5 | 1.5 | – | ♻️ 057 clone | 2 |
| Import 4 NuGet libraries (Logging, LoginAgent, Env, Excel) | AGENT → DEV-1 | 0.25 | 1.75 | – | ♻️ 058 import | 2 |
| Import Common workflows (8 files) | AGENT → DEV-2 | 0.25 | 1.75 | – | ♻️ 057 import | 2 |
| Import WorkQueue wrappers (9 files) | AGENT → DEV-2 | 0.25 | 1.75 | – | ♻️ 057 import | 2 |
| **GIO.FR.MCH.Library — Login (4 workflows)** | | | | | | |
| MCH_StartUp.xaml *(moderate)* | AGENT → DEV-1 | 0.5 | 3 | – | ♻️ 058 pattern adapted | 3.5 |
| MCH_CheckIfLogged.xaml *(simple)* | AGENT → DEV-1 | 0.25 | 2 | – | ♻️ 058 pattern | 2.25 |
| MCH_Login.xaml *(complex — AVIVA selectors)* | AGENT → DEV-1 | 0.5 | 4 | – | ♻️ 058 login pattern adapted | 4.5 |
| MCH_Logout.xaml *(simple)* | AGENT → DEV-1 | 0.25 | 1.5 | – | ♻️ 058 pattern | 1.75 |
| **GIO.FR.MCH.Library — Screens (2 workflows)** | | | | | | |
| MCH_BGMM.xaml *(complex — first screen build)* | AGENT → DEV-2 | 0.5 | 6 | – | First MCH screen — 5h base + 1h selectors | 6.5 |
| MCH_BGIR.xaml *(complex — second screen)* | AGENT → DEV-2 | 0.5 | 5 | – | Second screen — reuse BGMM pattern | 5.5 |
| **GIO.FR.Database.Library (5 workflows)** | | | | | | |
| DB_Connect + DB_ExecuteQuery *(moderate)* | AGENT → DEV-2 | 0.5 | 5 | – | – | 5.5 |
| DB_ExecuteNonQuery + DB_BulkInsert + DB_Close | AGENT → DEV-2 | 0.5 | 4 | – | ♻️ Pattern from Connect | 4.5 |
| **GIO.FR.MasterScheduler.Library (3 of 8 workflows)** | | | | | | |
| MS_ReadFile.xaml *(moderate)* | AGENT → DEV-1 | 0.5 | 5 | – | – | 5.5 |
| MS_FieldExists.xaml *(simple)* | AGENT → DEV-1 | 0.25 | 3 | – | – | 3.25 |
| MS_ValuesValidation.xaml *(moderate)* | AGENT → DEV-1 | 0.5 | 5 | – | – | 5.5 |
| **LEAD Tasks** | | | | | | |
| Configure 28 Orchestrator Assets in DEV | LEAD-1 | – | – | 8 | ♻️ 057 pattern | 8 |
| Configure 2 Orchestrator Queues | LEAD-1 | – | – | 4 | ♻️ 057 pattern | 4 |
| BRD review and sign-off | LEAD-1 | – | – | 6 | – | 6 |
| TDD review and sign-off | LEAD-1 | – | – | 12 | – | 12 |
| Code review (Sprint 1 PRs) | LEAD-1 | – | – | 10 | – | 10 |
| Sprint 2 dependency analysis + CyberArk coordination | LEAD-1 | – | – | 8 | – | 8 |
| MasterScheduler requirements validation with GIO SME | LEAD-1 | – | – | 4 | – | 4 |
| **Unit Testing** | | | | | | |
| Unit test: MCH Login, DB, MasterScheduler (DEV-1: 8h, DEV-2: 4h) | DEV-1+2 | – | 12 | – | – | 12 |
| **AGENT — Documentation** | | | | | | |
| Generate BRD, TDD, RC, Plan, Dev Guide + XAML scaffolds | AGENT-1 | 40 | – | – | – | 40 |
| **Sprint 1 Total** | | **~46h** | **~78h** | **~52h** | | **~176h** |

> **DEV-1 Sprint 1 breakdown:** S1-01 (2h) + S1-02 (2h) + MCH_StartUp/Check/Login/Logout (12h) + MS_ReadFile/FieldExists/ValuesValidation (14h) + Unit test (8h) + overhead/standups (4h) = **~42h + 10h dev collaboration = ~52h**
>
> **DEV-2 Sprint 1 breakdown:** S1-03 (2h) + S1-04 (2h) + MCH_BGMM (6h) + MCH_BGIR (5h) + DB Library 5 workflows (10h) + Unit test (4h) + overhead (4h) = **~33h + 19h integration/rework = ~52h**
>
> **LEAD-1 Sprint 1 breakdown:** Orchestrator setup (12h) + BRD/TDD review (18h) + code review (10h) + dependency analysis (6h) + CyberArk (2h) + MS validation (4h) = **~52h**
| Story ID | Story | Owner | Size | SP | Reused from 057? |
|---|---|---|---|---|---|
| S1-09 | Build GIO.FR.MCH.Library — MCH_StartUp, MCH_CheckIfLogged, MCH_Login, MCH_Logout | DEV-1 | M | 4 | PARTIAL — login pattern from 058 |
| S1-10 | Build GIO.FR.MCH.Library — MCH_BGMM, MCH_BGIR screen workflows | DEV-2 | M | 4 | NEW |
| S1-11 | Build GIO.FR.Database.Library — all 5 DB workflows | DEV-2 | M | 4 | NEW |
| S1-12 | Build GIO.FR.MasterScheduler.Library — ReadFile, FieldExists, ValuesValidation | DEV-1 | M | 4 | NEW |
| S1-13 | AGENT: Generate all BRD, TDD, RC, Plan documents | AGENT-1 | – | – | – |
| S1-14 | Unit test Sprint 1 new workflows (MCH Login, DB, MasterScheduler) | DEV-1+2 | S | 2 | – |

**Sprint 1 Allocation:**

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 52h (S1-01: 2h, S1-02: 2h, S1-09: 12h, S1-12: 14h, S1-14: 8h + overhead) | **81%** |
| DEV-2 | 64h | 52h (S1-03: 2h, S1-04: 2h, S1-10: 14h, S1-11: 12h, S1-14: 8h + overhead) | **81%** |
| LEAD-1 | 64h | 52h (S1-05: 8h, S1-06: 4h, S1-07: 6h, S1-08: 12h, code review 10h, Sprint 2 dependency analysis 6h, CyberArk provisioning coordination 2h, MasterScheduler requirements validation 4h) | **81%** |

> **Sprint 1 capacity note:** DEV-1 and DEV-2 are at 52h/81% (not 64h) by design — Sprint 1 is import-heavy (S1-01 through S1-04 are zero-effort reuse imports) with fewer net-new development hours. The 12h/dev buffer is absorbed by environment setup, tooling configuration, and Sprint 2 preparation. Effective utilization remains at 81% of available capacity. This aligns with LEAD's 52h/81% pattern across all sprints.

**Milestones:**
- M1: 029 project scaffold live in source control
- M2: All reusable libraries imported and verified
- M3: BRD and TDD signed off by LEAD

---

### Sprint 2 — Weeks 3–4 (2026-04-22 to 2026-05-06)
**Sprint Goal:** Complete MCH Library (all posting screens); build Accounting_WS Library; build MasterScheduler holiday validators; build MainScheduler Dispatcher.

| Story ID | Story | Owner | Size | SP | Reused from 057? |
|---|---|---|---|---|---|
| S2-01 | Build GIO.FR.MCH.Library — MCH_BSEC, MCH_BPOP | DEV-1 | M | 4 | NEW |
| S2-02 | Build GIO.FR.MCH.Library — MCH_PYDN posting screen | DEV-1 | L | 8 | NEW |
| S2-03 | Build GIO.FR.MCH.Library — MCH_PYUP posting screen | DEV-2 | L | 8 | NEW |
| S2-04 | Build GIO.FR.MCH.Library — MCH_PYDA posting screen | DEV-2 | L | 8 | NEW |
| S2-05 | Build GIO.FR.MasterScheduler.Library — 4 regional holiday date validators | DEV-1 | M | 4 | NEW |
| S2-06 | Build GIO.FR.Accounting_WS.Library — GMM_WS_DataExtraction + Group_CUSIP | DEV-2 | L | 8 | NEW |
| S2-07 | Build GIO.FR.Accounting_WS.Library — Bloomberg HTTP WS extraction | DEV-1 | L | 8 | NEW |
| S2-08 | Build GIO.FR.Accounting_WS.Library — RDR Login + RDR WS Extraction | DEV-2 | L | 8 | NEW |
| S2-09 | Architecture mid-sprint review — MCH screens sign-off | LEAD-1 | S | 2 | – |
| S2-10 | Build 029_FR_Dispatcher_MainScheduler (5 workflows) | DEV-1 | M | 4 | PARTIAL — pattern from 057 |
| S2-11 | Unit test Sprint 2 — MCH screens, Accounting_WS library | DEV-1+2 | M | 4 | – |

**Sprint 2 Allocation:**

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h (S2-01: 12h, S2-02: 20h, S2-05: 14h, S2-07: 20h partial; S2-10 start) | **80%** |
| DEV-2 | 64h | 64h (S2-03: 20h, S2-04: 20h, S2-06: 20h partial; S2-08 start) | **80%** |
| LEAD-1 | 64h | 52h (S2-09: 10h, code review: 16h, S2-08 RDR integration support: 10h, MCH selector validation & sign-off 8h, Sprint 3 Posting architecture prep 4h, coordination 4h) | **81%** |

**Milestones:**
- M4: GIO.FR.MCH.Library complete (all 12 workflows) and unit tested
- M5: Dispatcher_MainScheduler deployed and queue-tested in DEV

#### Sprint 2 Detailed Task Breakdown

| Task | Owner | Agent h | Dev h | Lead h | Reuse Note | Total |
|------|-------|---------|-------|--------|------------|-------|
| **MCH Library — Inquiry Screens (2 workflows)** | | | | | | |
| MCH_BSEC.xaml *(complex — security screen)* | AGENT → DEV-1 | 0.5 | 5 | – | First inquiry screen — 4h base + 1h selectors | 5.5 |
| MCH_BPOP.xaml *(complex — population screen)* | AGENT → DEV-1 | 0.5 | 5 | – | ♻️ Reuse BSEC pattern → **5h** (was 6h) | 5.5 |
| **MCH Library — Posting Screens (3 workflows)** | | | | | | |
| MCH_PYDN.xaml *(complex — posting down screen)* | AGENT → DEV-1 | 0.5 | 7 | – | First posting screen — 5h base + 2h selectors/validation | 7.5 |
| MCH_PYUP.xaml *(complex — posting up screen)* | AGENT → DEV-2 | 0.5 | 6 | – | ♻️ Reuse PYDN pattern → **6h** (was 7h) | 6.5 |
| MCH_PYDA.xaml *(complex — posting day screen)* | AGENT → DEV-2 | 0.5 | 6 | – | ♻️ Reuse PYDN/PYUP pattern → **6h** (was 7h) | 6.5 |
| MCH integration test (all 12 workflows E2E) | DEV-1 + DEV-2 | – | 4 | – | – | 4 |
| **MasterScheduler Library — Holiday Validators (4 workflows)** | | | | | | |
| MS_HolidayDate_US.xaml *(moderate)* | AGENT → DEV-1 | 0.5 | 4 | – | – | 4.5 |
| MS_HolidayDate_UK.xaml *(moderate)* | AGENT → DEV-1 | 0.25 | 3 | – | ♻️ Reuse US pattern → **3h** (was 4h) | 3.25 |
| MS_HolidayDate_EU.xaml *(moderate)* | AGENT → DEV-1 | 0.25 | 3 | – | ♻️ Reuse US/UK pattern | 3.25 |
| MS_HolidayDate_APAC.xaml *(moderate)* | AGENT → DEV-1 | 0.25 | 3 | – | ♻️ Reuse US/UK pattern | 3.25 |
| **Accounting_WS Library — Extraction (3 workflows)** | | | | | | |
| GMM_WS_DataExtraction.xaml *(complex)* | AGENT → DEV-2 | 0.5 | 6 | – | – | 6.5 |
| Group_CUSIP.xaml *(moderate)* | AGENT → DEV-2 | 0.5 | 4 | – | – | 4.5 |
| Bloomberg_HTTP_WS_Extraction.xaml *(complex — API)* | AGENT → DEV-1 | 0.5 | 6 | – | – | 6.5 |
| **Accounting_WS Library — RDR (2 workflows)** | | | | | | |
| RDR_Login.xaml *(complex — cloud client DLL)* | AGENT → DEV-2 | 0.5 | 5 | – | – | 5.5 |
| RDR_WS_Extraction.xaml *(complex)* | AGENT → DEV-2 | 0.5 | 5 | – | – | 5.5 |
| **Dispatcher — MainScheduler (5 workflows)** | | | | | | |
| Dispatcher_MainScheduler.xaml *(moderate — REF wiring)* | AGENT → DEV-1 | 0.5 | 4 | – | ♻️ 057 Dispatcher pattern adapted | 4.5 |
| GetTransactionData_MS.xaml + SetTransactionStatus_MS.xaml | AGENT → DEV-1 | 0.5 | 3 | – | ♻️ 057 REF pattern | 3.5 |
| Process_MS.xaml + InitAllApplications_MS.xaml | AGENT → DEV-1 | 0.5 | 3 | – | ♻️ 057 REF pattern | 3.5 |
| Dispatcher pipeline test (queue populated from MasterScheduler file) | DEV-1 | – | 2 | – | – | 2 |
| **LEAD Tasks** | | | | | | |
| Architecture mid-sprint review — MCH screens sign-off | LEAD-1 | – | – | 10 | – | 10 |
| MCH selector validation & AVIVA terminal sign-off | LEAD-1 | – | – | 8 | – | 8 |
| Code review (MCH posting, Accounting_WS, Dispatcher) | LEAD-1 | – | – | 16 | – | 16 |
| RDR Cloud Client DLL integration support | LEAD-1 | – | – | 6 | – | 6 |
| Sprint 3 Posting architecture prep | LEAD-1 | – | – | 4 | – | 4 |
| Coordination + sprint ceremonies | LEAD-1 | – | – | 4 | – | 4 |
| Bloomberg API endpoint validation | LEAD-1 | – | – | 4 | – | 4 |
| **Unit Testing** | | | | | | |
| Unit test: MCH screens, Accounting_WS, Dispatcher (DEV-1: 8h, DEV-2: 6h) | DEV-1+2 | – | 14 | – | – | 14 |
| **AGENT — Scaffolding** | | | | | | |
| XAML scaffolds for MCH screen workflows, Accounting_WS, Dispatcher | AGENT-1 | 8 | – | – | – | 8 |
| **Sprint 2 Total** | | **~8h** | **~118h** | **~52h** | | **~178h** |

> **DEV-1 Sprint 2 breakdown:** MCH_BSEC (5h) + MCH_BPOP (5h) + MCH_PYDN (7h) + MS_Holiday ×4 (13h) + Bloomberg_HTTP (6h) + Dispatcher ×5 (10h) + MCH integ test (2h) + Dispatcher test (2h) + unit test (8h) + overhead (4h) = **~62h**
>
> **DEV-2 Sprint 2 breakdown:** MCH_PYUP (6h) + MCH_PYDA (6h) + MCH integ test (2h) + GMM_WS (6h) + Group_CUSIP (4h) + RDR_Login (5h) + RDR_WS (5h) + unit test (6h) + overhead (4h) + integration support (14h) = **~58h**
>
> **LEAD-1 Sprint 2 breakdown:** Mid-sprint MCH review (10h) + MCH selector validation (8h) + code review (16h) + RDR support (6h) + Sprint 3 prep (4h) + Bloomberg validation (4h) + coordination (4h) = **~52h**
>
> **Milestone M4:** MCH Library fully complete — all 12 workflows (4 login + 2 inquiry + 3 posting + 3 from Sprint 1) tested E2E.
> **Milestone M5:** Dispatcher_MainScheduler deployed and queue-tested in DEV.

---

### Sprint 3 — Weeks 5–6 (2026-05-06 to 2026-05-20)
**Sprint Goal:** Complete Accounting_WS Library; build DataTransformation, Reporting, Posting libraries; build DataExtraction performer and Sentinel.

| Story ID | Story | Owner | Size | SP | Reused from 057? |
|---|---|---|---|---|---|
| S3-01 | Build GIO.FR.Accounting_WS.Library — MergeBB_LotLevel + Aggregate_Filter | DEV-1 | M | 4 | NEW |
| S3-02 | Build GIO.FR.DataTransformation.Library — Filter_Calculate + Set_Column_Data + Change_Column_Position | DEV-2 | M | 4 | NEW |
| S3-03 | Build GIO.FR.DataTransformation.Library — BGIR_WS_Call + BGI_Loop + Write_ToExcel | DEV-1 | M | 4 | NEW |
| S3-04 | Build GIO.FR.Reporting.Library — CreateReport + CreateWorksheetAndWriteCollection + WriteDailyReport | DEV-2 | M | 4 | NEW |
| S3-05 | Build GIO.FR.Posting.Library — CreateInstance + DeleteSheetContents + CreateSheetAndWriteContents + Check_PostingType | DEV-1 | M | 4 | NEW |
| S3-06 | Build GIO.FR.Posting.Library — Main_Action + Posting orchestration | DEV-2 | L | 8 | NEW |
| S3-07 | Build GIO.FR.Posting.Library — FR.PYDN/PYUP/PYDA_Posting (standard) | DEV-1 | L | 8 | NEW |
| S3-08 | Build GIO.FR.Posting.Library — FR.PYDN/PYUP/PYDA_Posting_UMAS (3 UMAS variants) | DEV-2 | L | 8 | NEW |
| S3-09 | Build 029_FR_Performer_DataExtraction (7 workflows) | DEV-1 | L | 8 | NEW |
| S3-10 | Build 029_FR_Sentinel (6 workflows) | DEV-2 | M | 4 | NEW |
| S3-11 | Unit test — MCH Library full regression + Posting Library | DEV-1+2 | M | 4 | – |
| S3-12 | Code review Sprint 2 & 3 libraries | LEAD-1 | M | 4 | – |

**Sprint 3 Allocation:**

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h (S3-01: 12h, S3-03: 12h, S3-05: 12h, S3-07: 20h, S3-09 start: 8h) | **80%** |
| DEV-2 | 64h | 64h (S3-02: 12h, S3-04: 12h, S3-06: 20h, S3-08: 20h partial) | **80%** |
| LEAD-1 | 64h | 52h (S3-12: 24h review, S3-10 Sentinel oversight: 10h, integration pre-testing 8h, exception matrix validation 4h, coordination: 6h) | **81%** |

**Milestones:**
- M6: GIO.FR.Posting.Library complete and unit tested (all 6 posting variants)
- M7: DataExtraction performer queued and extracting test data
- M8: Sentinel deployed and passing all connectivity checks

#### Sprint 3 Detailed Task Breakdown

| Task | Owner | Agent h | Dev h | Lead h | Reuse Note | Total |
|------|-------|---------|-------|--------|------------|-------|
| **Accounting_WS Library — Merge & Filter (2 workflows)** | | | | | | |
| MergeBB_LotLevel.xaml *(moderate)* | AGENT → DEV-1 | 0.5 | 5 | – | – | 5.5 |
| Aggregate_Filter.xaml *(moderate)* | AGENT → DEV-1 | 0.5 | 5 | – | ♻️ Reuse MergeBB pattern → **5h** (was 6h) | 5.5 |
| **DataTransformation Library (6 workflows)** | | | | | | |
| Filter_Calculate.xaml *(moderate)* | AGENT → DEV-2 | 0.5 | 4 | – | – | 4.5 |
| Set_Column_Data.xaml *(simple)* | AGENT → DEV-2 | 0.25 | 3 | – | – | 3.25 |
| Change_Column_Position.xaml *(simple)* | AGENT → DEV-2 | 0.25 | 2 | – | ♻️ Reuse Set_Column pattern | 2.25 |
| BGIR_WS_Call.xaml *(complex — MCH integration)* | AGENT → DEV-1 | 0.5 | 5 | – | ♻️ Reuse MCH_BGIR pattern from S1 → **5h** (was 7h) | 5.5 |
| BGI_Loop.xaml *(moderate)* | AGENT → DEV-1 | 0.5 | 4 | – | – | 4.5 |
| Write_ToExcel.xaml *(simple)* | AGENT → DEV-1 | 0.25 | 2 | – | ♻️ Excel Utilities pattern | 2.25 |
| **Reporting Library (3 workflows)** | | | | | | |
| CreateReport.xaml *(moderate)* | AGENT → DEV-2 | 0.5 | 4 | – | – | 4.5 |
| CreateWorksheetAndWriteCollection.xaml *(moderate)* | AGENT → DEV-2 | 0.5 | 4 | – | ♻️ Reuse CreateReport pattern | 4.5 |
| WriteDailyReport.xaml *(moderate)* | AGENT → DEV-2 | 0.5 | 4 | – | – | 4.5 |
| **Posting Library (10 workflows)** | | | | | | |
| CreateInstance.xaml *(simple)* | AGENT → DEV-1 | 0.25 | 2 | – | – | 2.25 |
| DeleteSheetContents.xaml *(simple)* | AGENT → DEV-1 | 0.25 | 2 | – | – | 2.25 |
| CreateSheetAndWriteContents.xaml *(moderate)* | AGENT → DEV-1 | 0.5 | 3 | – | – | 3.5 |
| Check_PostingType.xaml *(moderate — routing logic)* | AGENT → DEV-1 | 0.5 | 4 | – | – | 4.5 |
| Main_Action.xaml *(complex — orchestration)* | AGENT → DEV-2 | 0.5 | 6 | – | – | 6.5 |
| FR.PYDN_Posting.xaml *(complex — standard)* | AGENT → DEV-1 | 0.5 | 5 | – | ♻️ Reuse MCH_PYDN pattern from S2 → **5h** (was 7h) | 5.5 |
| FR.PYUP_Posting.xaml *(complex — standard)* | AGENT → DEV-2 | 0.5 | 4 | – | ♻️ Reuse PYDN_Posting pattern → **4h** (was 5h) | 4.5 |
| FR.PYDA_Posting.xaml *(complex — standard)* | AGENT → DEV-2 | 0.5 | 4 | – | ♻️ Reuse PYDN/PYUP pattern | 4.5 |
| FR.PYDN_Posting_UMAS.xaml *(complex — UMAS variant)* | AGENT → DEV-1 | 0.5 | 5 | – | ♻️ Reuse standard PYDN + UMAS DLL wrapper | 5.5 |
| FR.PYUP_Posting_UMAS.xaml + FR.PYDA_Posting_UMAS.xaml | AGENT → DEV-2 | 1 | 8 | – | ♻️ Reuse PYDN_UMAS pattern → **4h each** | 9 |
| **DataExtraction Performer (7 workflows)** | | | | | | |
| Performer_DataExtraction.xaml *(REF wiring)* | AGENT → DEV-1 | 0.5 | 3 | – | ♻️ Dispatcher REF pattern from S2 | 3.5 |
| GetTransactionData_DE.xaml + SetTransactionStatus_DE.xaml | AGENT → DEV-1 | 0.5 | 3 | – | ♻️ Dispatcher REF pattern | 3.5 |
| Process_DE.xaml *(complex — extraction orchestration)* | AGENT → DEV-1 | 0.5 | 5 | – | – | 5.5 |
| InitAllApplications_DE.xaml + CloseAllApplications_DE.xaml | AGENT → DEV-1 | 0.25 | 2 | – | ♻️ Dispatcher pattern **→ S4** | 2.25 |
| ExtractData_DE.xaml *(complex — main extraction logic)* | AGENT → DEV-1 | 0.5 | 4 | – | **→ deferred to S4** | 4.5 |
| **Sentinel (6 workflows)** | | | | | | |
| Sentinel_Process.xaml *(REF wiring)* | AGENT → DEV-2 | 0.5 | 3 | – | ♻️ Dispatcher/Performer REF pattern | 3.5 |
| Sentinel_CheckConnectivity.xaml *(MCH + DB + Bloomberg)* | AGENT → DEV-2 | 0.5 | 4 | – | ♻️ Reuse MCH_Login + DB_Connect patterns **→ S4** | 4.5 |
| Sentinel_ValidateSchedule.xaml | AGENT → DEV-2 | 0.5 | 3 | – | ♻️ Reuse MS_ReadFile pattern from S1 **→ S4** | 3.5 |
| Sentinel_CheckDiskSpace.xaml + Sentinel_Notify.xaml + Sentinel_CleanUp.xaml | AGENT → DEV-2 | 0.5 | 4 | – | **→ deferred to S4** | 4.5 |
| **LEAD Tasks** | | | | | | |
| Code review Sprint 2 & 3 libraries (Posting, DataTransform, Reporting) | LEAD-1 | – | – | 16 | – | 16 |
| Sentinel oversight and connectivity validation | LEAD-1 | – | – | 8 | – | 8 |
| Integration pre-testing (Dispatcher→DataExtraction queue flow) | LEAD-1 | – | – | 8 | – | 8 |
| Exception matrix validation (all posting variants) | LEAD-1 | – | – | 6 | – | 6 |
| UMAS DLL .NET version testing | LEAD-1 | – | – | 6 | – | 6 |
| Sprint 4 TransformPosting architecture prep | LEAD-1 | – | – | 4 | – | 4 |
| Coordination + sprint ceremonies | LEAD-1 | – | – | 4 | – | 4 |
| **Unit Testing** | | | | | | |
| Unit test: Posting Library (6 variants), DataTransformation, DataExtraction (partial) | DEV-1+2 | – | 8 | – | – | 8 |
| MCH Library full regression (all 12 workflows) | DEV-1+2 | – | 4 | – | ♻️ Reuse S2 MCH test harness → **0h setup** | 4 |
| **AGENT — Scaffolding** | | | | | | |
| XAML scaffolds for DataTransformation, Posting, Reporting, Sentinel | AGENT-1 | 8 | – | – | – | 8 |
| **Sprint 3 Total** | | **~8h** | **~101h** | **~52h** | *(17h dev + 2h test deferred to S4)* | **~161h** |

> **DEV-1 Sprint 3 breakdown:** Accounting_WS merge (10h) + DataTransformation 3 workflows (11h) + Posting Library — CreateInstance/DeleteSheet/CreateSheet/Check/PYDN_Std/PYDN_UMAS (21h) + DataExtraction 5 of 7 workflows (12h) + MCH regression (2h) + unit test (4h) + overhead (4h) = **~64h** — remaining 2 DataExtraction workflows (ExtractData_DE + InitAll/CloseAll, ~6h + 1h test) deferred to Sprint 4 DEV-1
>
> **DEV-2 Sprint 3 breakdown:** DataTransformation 3 workflows (9h) + Reporting Library (12h) + Posting Main_Action/PYUP/PYDA std/PYUP_UMAS/PYDA_UMAS (26h) + Sentinel — Sentinel_Process + scaffolding (7h) + MCH regression (2h) + unit test (4h) + overhead (3h) = **~63h** — remaining 3 Sentinel workflows (CheckConnectivity + ValidateSchedule + CheckDiskSpace/Notify/CleanUp, ~11h + 1h test) deferred to Sprint 4 DEV-2
>
> **⚠️ Rebalancing Note:** Sprint 3 scope was rebalanced to fit within 64h/dev capacity. 2 DataExtraction workflows (DEV-1, ~7h) and 3 Sentinel workflows (DEV-2, ~12h) are deferred to Sprint 4, which has sufficient slack (DEV-1: 56→63h, DEV-2: 39→51h). No sprints added; total project hours unchanged at ~776h.
>
> **LEAD-1 Sprint 3 breakdown:** Code review (16h) + Sentinel oversight (8h) + integration pre-testing (8h) + exception matrix (6h) + UMAS DLL test (6h) + Sprint 4 prep (4h) + coordination (4h) = **~52h**
>
> **Milestone M6:** GIO.FR.Posting.Library complete — all 10 workflows including 6 posting variants (3 standard + 3 UMAS) tested.
> **Milestone M7:** DataExtraction performer queued and processing test data from Dispatcher queue.
> **Milestone M8:** Sentinel deployed and passing all connectivity checks (MCH, DB, Bloomberg, disk, schedule).

---

### Sprint 4 — Weeks 7–8 (2026-05-20 to 2026-06-03)
**Sprint Goal:** TransformPosting performer complete; Adhoc/NOVAL/SLA/DBCleanup processes built; full integration test; UAT; deployment.

| Story ID | Story | Owner | Size | SP | Reused from 057? |
|---|---|---|---|---|---|
| S4-01 | Build 029_FR_Performer_TransformPosting (5 workflows) | DEV-1 | L | 8 | NEW |
| S4-02 | Build 029_FR_Dispatcher_Adhoc + Dispatcher_NOVAL | DEV-2 | S | 2 | PARTIAL — MainScheduler pattern |
| S4-03 | Build 029_FR_SLAMonitor process | DEV-1 | S | 2 | NEW |
| S4-04 | Build 029_FR_DatabaseCleanup process | DEV-2 | S | 2 | NEW |
| S4-05 | Integration testing — Dispatcher→DataExtraction queue flow | DEV-1+2 | M | 4 | – |
| S4-06 | Integration testing — DataExtraction→TransformPosting queue flow | DEV-1+2 | M | 4 | – |
| S4-07 | End-to-end test: 3 real fund list samples through full pipeline | LEAD-1 + DEV | L | 8 | – |
| S4-08 | UAT: business validation of output vs. BP baseline | LEAD-1 | M | 4 | – |
| S4-09 | Publish 7 library NuPkgs + deploy 8 processes to Orchestrator | DEV-1 | S | 2 | – |
| S4-10 | AGENT: Generate Development Guide, Project Plan, Agent Utilization, Timeline Assessment, Utilization Summary docs | AGENT-1 | – | – | – |
| S4-11 | Configure prod Orchestrator schedules; smoke test | LEAD-1 + DEV-1 | M | 4 | YES — 057 pattern |
| S4-12 | Exception handling regression test (simulate all 12 exception types) | DEV-1+2 | M | 4 | – |
| S4-13 | Handover: SOP, runbook, escalation matrix | AGENT-1 + LEAD-1 | S | 2 | – |

**Sprint 4 Allocation:**

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h (S4-01: 22h, S4-03: 8h, S4-05: 8h, S4-06: 8h, S4-09: 8h, S4-12: 10h) | **80%** |
| DEV-2 | 64h | 64h (S4-02: 8h, S4-04: 8h, S4-05: 8h, S4-06: 8h, S4-08 support: 8h, S4-12: 8h, S4-11 support: 8h) | **80%** |
| LEAD-1 | 64h | 52h (S4-07: 18h, S4-08: 12h, S4-11: 6h, S4-13: 4h, production smoke testing 4h, SOP/runbook review 4h, post-deployment monitoring 4h) | **81%** |

**Milestones:**
- M9: Full end-to-end pipeline tested and passing on 3 fund list samples
- M10: UAT signed off by business
- M11: All packages deployed to production Orchestrator
- M12: Production go-live — first live COB run complete

#### Sprint 4 Detailed Task Breakdown

| Task | Owner | Agent h | Dev h | Lead h | Reuse Note | Total |
|------|-------|---------|-------|--------|------------|-------|
| **TransformPosting Performer (5 workflows)** | | | | | | |
| Performer_TransformPosting.xaml *(REF wiring)* | AGENT → DEV-1 | 0.5 | 3 | – | ♻️ DataExtraction REF pattern from S3 | 3.5 |
| GetTransactionData_TP.xaml + SetTransactionStatus_TP.xaml | AGENT → DEV-1 | 0.5 | 3 | – | ♻️ DataExtraction REF pattern | 3.5 |
| Process_TP.xaml *(complex — transform + posting orchestration)* | AGENT → DEV-1 | 0.5 | 7 | – | – | 7.5 |
| InitAllApplications_TP.xaml + CloseAllApplications_TP.xaml | AGENT → DEV-1 | 0.25 | 2 | – | ♻️ DataExtraction pattern | 2.25 |
| TransformPosting unit test | DEV-1 | – | 4 | – | – | 4 |
| **Support Processes** | | | | | | |
| 029_FR_Dispatcher_Adhoc.xaml *(simple — 3 workflows)* | AGENT → DEV-2 | 0.5 | 3 | – | ♻️ MainScheduler pattern → **3h** (was 5h) | 3.5 |
| 029_FR_Dispatcher_NOVAL.xaml *(simple — 3 workflows)* | AGENT → DEV-2 | 0.5 | 3 | – | ♻️ Adhoc pattern | 3.5 |
| 029_FR_SLAMonitor.xaml *(moderate — monitoring + alerts)* | AGENT → DEV-1 | 0.5 | 5 | – | – | 5.5 |
| 029_FR_DatabaseCleanup.xaml *(simple — scheduled cleanup)* | AGENT → DEV-2 | 0.25 | 3 | – | ♻️ DB Library pattern from S1 | 3.25 |
| **Integration Testing** | | | | | | |
| Dispatcher→DataExtraction queue flow test | DEV-1 + DEV-2 | – | 8 | – | – | 8 |
| DataExtraction→TransformPosting queue flow test | DEV-1 + DEV-2 | – | 8 | – | – | 8 |
| End-to-end test: 3 real fund list samples through full pipeline | DEV-1 + DEV-2 + LEAD | – | 10 | 8 | – | 18 |
| Exception handling regression (simulate all 12 exception types) | DEV-1 + DEV-2 | – | 10 | – | – | 10 |
| **UAT & Deployment** | | | | | | |
| UAT: business validation of output vs. BP baseline | LEAD-1 + DEV-2 | – | 4 | 8 | – | 12 |
| Publish 7 library NuPkgs + deploy 8 processes to Orchestrator | DEV-1 + LEAD | – | 4 | 4 | – | 8 |
| Configure prod Orchestrator schedules + triggers | LEAD-1 + DEV-1 | – | 2 | 4 | ♻️ 057 Orch pattern | 6 |
| Production smoke test (1 live COB run) | LEAD-1 + DEV-1 | – | 4 | 4 | – | 8 |
| **Deferred from Sprint 3 (Rebalancing)** | | | | | | |
| ExtractData_DE.xaml *(complex — main extraction logic)* | DEV-1 | – | 4 | – | ← S3 DEV-1 rebalance (AGENT scaffolded in S3) | 4 |
| InitAllApplications_DE.xaml + CloseAllApplications_DE.xaml | DEV-1 | – | 2 | – | ← S3 DEV-1 rebalance | 2 |
| DataExtraction deferred unit test | DEV-1 | – | 1 | – | ← S3 test carried over | 1 |
| Sentinel_CheckConnectivity.xaml *(MCH + DB + Bloomberg)* | DEV-2 | – | 4 | – | ← S3 DEV-2 rebalance (AGENT scaffolded in S3) | 4 |
| Sentinel_ValidateSchedule.xaml | DEV-2 | – | 3 | – | ← S3 DEV-2 rebalance | 3 |
| Sentinel_CheckDiskSpace.xaml + Sentinel_Notify.xaml + Sentinel_CleanUp.xaml | DEV-2 | – | 4 | – | ← S3 DEV-2 rebalance | 4 |
| Sentinel deferred unit test | DEV-2 | – | 1 | – | ← S3 test carried over | 1 |
| **LEAD Tasks** | | | | | | |
| E2E pipeline integration oversight | LEAD-1 | – | – | 8 | – | 8 |
| Code review TransformPosting + support processes | LEAD-1 | – | – | 8 | – | 8 |
| SOP/runbook review and sign-off | LEAD-1 | – | – | 4 | – | 4 |
| Post-deployment monitoring plan | LEAD-1 | – | – | 4 | – | 4 |
| **AGENT — Documentation** | | | | | | |
| Generate Project Plan, Agent Utilization, Timeline Assessment, Utilization Summary, SOP/runbook | AGENT-1 | 24 | – | – | – | 24 |
| **Sprint 4 Total** | | **~24h** | **~115h** | **~52h** | *(incl. 19h deferred from S3)* | **~191h** |

> **DEV-1 Sprint 4 breakdown:** TransformPosting 5 workflows (15h) + TP unit test (4h) + SLAMonitor (5h) + DataExtraction remaining 2 workflows from S3 (ExtractData_DE + InitAll/CloseAll, 6h) + DE unit test (1h) + Dispatcher→DE test (4h) + DE→TP test (4h) + E2E test (5h) + exception regression (5h) + publish/deploy (4h) + Orch schedules (2h) + smoke test (4h) + overhead (4h) = **~63h**
>
> **DEV-2 Sprint 4 breakdown:** Dispatcher_Adhoc (3h) + Dispatcher_NOVAL (3h) + DatabaseCleanup (3h) + Sentinel remaining 3 workflows from S3 (CheckConnectivity + ValidateSchedule + CheckDiskSpace/Notify/CleanUp, 11h) + Sentinel unit test (1h) + Dispatcher→DE test (4h) + DE→TP test (4h) + E2E test (5h) + exception regression (5h) + UAT support (4h) + deployment support (4h) + overhead (4h) = **~51h** — remaining 13h on integration defect fixing + cross-process regression
>
> **LEAD-1 Sprint 4 breakdown:** E2E 3-sample test (8h) + E2E pipeline oversight (8h) + UAT (8h) + code review (8h) + publish/deploy (4h) + Orch schedules (4h) + smoke test (4h) + SOP review (4h) + post-deployment (4h) = **~52h**
>
> **Milestone M9:** Full end-to-end pipeline tested — Dispatcher→DataExtraction→TransformPosting→Posting with 3 real fund list samples.
> **Milestone M10:** UAT signed off by business — output matches BP baseline within tolerance.
> **Milestone M11:** All 7 NuPkgs published + 8 processes deployed to production Orchestrator.
> **Milestone M12:** Production go-live — first live COB run complete and monitored.

---

## Section 3 — Sprint Summary

### 3.1 Sprint Summary Table

| Sprint | Weeks | Focus | Total SP | DEV-1 SP | DEV-2 SP | LEAD SP | ~Hours | Reuse |
|---|---|---|---|---|---|---|---|---|
| Sprint 1 | 1–2 | Foundation, Imports, MCH Login, DB, MasterScheduler | 24 | 9 | 9 | 6 | ~196h | **4 stories @ 0 effort (~160h saved)** |
| Sprint 2 | 3–4 | MCH Screens (PYDN/PYUP/PYDA), Accounting_WS, Dispatcher | 66 | 30 | 34 | 2 | ~188h | — |
| Sprint 3 | 5–6 | Accounting_WS, DataTransformation, Posting Library, DataExtraction, Sentinel | 64 | 30 | 30 | 4 | ~188h | — |
| Sprint 4 | 7–8 | TransformPosting, Support Processes, Integration, UAT, Deploy | 46 | 22 | 12 | 12 | ~204h | — |
| **TOTAL** | **8 wks** | **4 Sprints** | **200** | **91** | **85** | **24** | **~776h** | **4 stories / ~160h saved → 1 sprint eliminated** |

### 3.2 Utilization Summary — All Sprints

| Resource | Sp1 | Sp2 | Sp3 | Sp4 | **Total Hrs** | **Avg Util %** |
|---|---|---|---|---|---|---|
| **DEV-1** | 52h (81%) | 64h (80%) | 64h (80%) | 64h (80%) | **244h** | **~80%** |
| **DEV-2** | 52h (81%) | 64h (80%) | 64h (80%) | 64h (80%) | **244h** | **~80%** |
| **LEAD** | 52h (~81%) | 52h (~81%) | 52h (~81%) | 52h (~81%) | **208h** | **~81%** |
| **AGENT** | 40h | 8h | 8h | 24h | **80h** | — |
| **Total Planned** | **~196h** | **~188h** | **~188h** | **~204h** | **~776h** | |
| **Lead Buffer** | +12h | +12h | +12h | +12h | **+48h** | _contingency_ |

> **DEV-1 and DEV-2 at ~80% every sprint. LEAD at 80% allocation = 64h available, ~52h planned work = ~81% utilization with 12h/sprint contingency buffer. Zero waste on DEVs.**
> **057/058 reuse compressed 5 sprints → 4 sprints, saving 2 weeks and ~160 hours.**

### 3.3 Zero-Effort Reuse Register

| Story ID | Description | Original Effort | Source | 029 Effort | Hours Saved |
|---|---|---|---|---|---|
| S1-01 | REFramework clone + Config.xlsx | 24h | 057 – Ben Profiling | **0h** | 24h |
| S1-02 | 4 NuGet libraries import | 68h | 058 – Market Profiling | **0h** | 68h |
| S1-03 | Common workflows (8 files) | 32h | 057 – Ben Profiling | **0h** | 32h |
| S1-04 | WorkQueue wrappers (9 files) | 28h | 057 – Ben Profiling | **0h** | 28h |
| S2-10 | Dispatcher pattern (adapted from 057) | 12h | 057 – Ben Profiling | 4h net (customize only) | 8h |
| **TOTAL** | | **164h** | | **4h** | **160h** |

### 3.4 Milestone Tracker

| # | Milestone | Sprint | Target Week | Gate Criteria |
|---|-----------|--------|-------------|---------------|
| M1 | 029 project scaffold live in source control | Sprint 1 | Week 1 | REFramework builds clean, Config.xlsx loads all 3 sheets, project.json valid |
| M2 | All reusable libraries imported and verified | Sprint 1 | Week 1 | 4 NuGet libraries resolve, 8 Common workflows compile, 9 WQ wrappers compile |
| M3 | BRD and TDD signed off by LEAD | Sprint 1 | Week 2 | BRD covers all 12 BP processes, TDD maps all 87 workflows, LEAD approval sign-off |
| M4 | GIO.FR.MCH.Library complete and unit tested | Sprint 2 | Week 4 | All 12 MCH workflows pass unit test: Login → BGMM/BGIR/BSEC/BPOP/PYDN/PYUP/PYDA → Logout |
| M5 | Dispatcher_MainScheduler deployed and queue-tested | Sprint 2 | Week 4 | Queue populated from MasterScheduler file; 5 items visible in Orchestrator DEV queue |
| M6 | GIO.FR.Posting.Library complete (all 6 variants) | Sprint 3 | Week 6 | All 6 posting workflows (3 standard + 3 UMAS) pass unit test with mock MCH screens |
| M7 | DataExtraction performer queued and running | Sprint 3 | Week 6 | Performer picks item from queue, extracts data from GMM/Bloomberg/RDR, writes to staging |
| M8 | Sentinel passing all connectivity checks | Sprint 3 | Week 6 | Sentinel validates MCH, DB, Bloomberg, disk space, schedule — all 5 checks pass |
| M9 | Full end-to-end pipeline tested on 3 fund list samples | Sprint 4 | Week 7 | Dispatcher→DataExtraction→TransformPosting→Posting completes for 3 distinct fund lists |
| M10 | UAT signed off by business team | Sprint 4 | Week 8 | Output matches BP baseline for all 3 test fund lists; business sign-off received |
| M11 | All packages deployed to production Orchestrator | Sprint 4 | Week 8 | 7 NuPkgs published, 8 processes created, assets/queues configured in PROD |
| M12 | Production go-live — first live COB run complete | Sprint 4 | Week 8 | First live COB run completes without Business Exception; logs clean; monitoring active |

### 3.5 Grand Total — Hours by Resource

| Resource | Sprint 1 | Sprint 2 | Sprint 3 | Sprint 4 | **TOTAL** |
|----------|----------|----------|----------|----------|----------|
| **AGENT** | 40 | 8 | 8 | 24 | **80** |
| **DEV-1** | 52 | 64 | 64 | 64 | **244** |
| **DEV-2** | 52 | 64 | 64 | 64 | **244** |
| **LEAD** | 52 | 52 | 52 | 52 | **208** |
| **Sprint Total** | 196 | 188 | 188 | 204 | **776** |

### 3.6 Summary by Role

| Resource | Total Hours | Total Days (8h/day) | Total Sprints | % of Total Effort | Utilization |
|----------|-------------|---------------------|---------------|-------------------|-------------|
| **AI Agent** | 80 | 10.0 days | — | 10.3% | — |
| **Developer 1** | 244 | 30.5 days | 4 sprints | 31.4% | **~80%** |
| **Developer 2** | 244 | 30.5 days | 4 sprints | 31.4% | **~80%** |
| **Lead** | 208 | 26.0 days | 4 sprints | 26.8% | **~81%** |
| **Human Total** | **696** | **87.0 days** | **—** | **89.7%** | **~80%** |
| **Grand Total** | **776** | — | **4 sprints (8 weeks)** | **100%** | — |

### 3.7 Overall Utilization Summary

| Resource | Capacity (4 × 64h) | Allocated | Overall Utilization | Per Sprint |
|----------|---------------------|-----------|---------------------|------------|
| **DEV-1** | 256h | **244h** | **~80%** | 81% / 80% / 80% / 80% |
| **DEV-2** | 256h | **244h** | **~80%** | 81% / 80% / 80% / 80% |
| **LEAD** | 256h | **208h** | **~81%** | 81% / 81% / 81% / 81% |
| **TEAM** | 768h | **696h** | **~80%** | Uniform across all sprints |

---

## Section 4 — Timeline

### 4.1 Sprint Schedule Table

| Sprint | Weeks | Dates | Focus Area | Key Deliverables | Milestone |
|---|---|---|---|---|---|
| Sprint 1 | 1–2 | Apr 8 – Apr 22, 2026 | Setup + Foundation + MCH Login | Project scaffold, 4 NuGet imports, REFramework, MCH Login, DB Library, MasterScheduler Library, BRD/TDD sign-off | M1, M2, M3 |
| Sprint 2 | 3–4 | Apr 22 – May 6, 2026 | MCH Screens + Accounting_WS + Dispatcher | MCH PYDN/PYUP/PYDA/BSEC/BPOP, Bloomberg + RDR extraction, MainScheduler Dispatcher | M4, M5 |
| Sprint 3 | 5–6 | May 6 – May 20, 2026 | Libraries + Performers | Accounting_WS merge, DataTransformation, Reporting, Posting Library (6 variants), DataExtraction, Sentinel | M6, M7, M8 |
| Sprint 4 | 7–8 | May 20 – Jun 3, 2026 | Integration + Testing + Deployment | TransformPosting, support processes, integration testing, UAT, production deployment | M9, M10, M11, M12 |

### 4.2 Week-by-Week Assignment Map

#### Sprint 1 (Weeks 1–2) | DEV-1: 52h (81%) · DEV-2: 52h (81%) · LEAD: 52h (81%)

| Week | DEV-1 (MCH + MasterScheduler) | DEV-2 (DB + MCH Screens) | LEAD (Orch + Review) | AGENT |
|------|-------------------------------|--------------------------|----------------------|-------|
| **1** | Clone REFramework + Config · Import NuGet ×4 · MCH_StartUp · MCH_CheckIfLogged | Import Common ×8 · Import WQ wrappers ×9 · MCH_BGMM *(complex first screen)* | Configure 28 Orch Assets · Configure 2 Orch Queues · BRD review | Gen BRD, TDD, RC, Plan scaffolds |
| **2** | MCH_Login *(complex — AVIVA)* · MCH_Logout · MS_ReadFile · MS_FieldExists · MS_ValuesValidation · Unit test MCH Login | MCH_BGIR · DB_Connect · DB_ExecuteQuery · DB_ExecuteNonQuery · DB_BulkInsert · DB_Close · Unit test DB | TDD review + sign-off · Code review S1 PRs · Sprint 2 dependency analysis · CyberArk coord · MS requirements validation | Gen Dev Guide + XAML scaffolds |

#### Sprint 2 (Weeks 3–4) | DEV-1: 64h (80%) · DEV-2: 64h (80%) · LEAD: 52h (81%)

| Week | DEV-1 (MCH Inquiry + Posting + Holiday) | DEV-2 (MCH Posting + Accounting_WS) | LEAD (Review + Integration) | AGENT |
|------|----------------------------------------|--------------------------------------|----------------------------|-------|
| **3** | MCH_BSEC · MCH_BPOP · MCH_PYDN *(first posting screen)* · MS_HolidayDate_US · MS_HolidayDate_UK | MCH_PYUP ♻️ PYDN pattern · MCH_PYDA ♻️ · GMM_WS_DataExtraction · Group_CUSIP | Mid-sprint MCH review · MCH selector validation · Bloomberg API validation | XAML scaffolds for MCH screens |
| **4** | MS_HolidayDate_EU ♻️ · MS_HolidayDate_APAC ♻️ · Bloomberg_HTTP_WS · Dispatcher_MainScheduler ♻️ 057 · Dispatcher pipeline test | RDR_Login *(complex — DLL)* · RDR_WS_Extraction · MCH integration test | Code review (MCH, Accounting_WS, Dispatcher) · RDR DLL support · Sprint 3 Posting prep | XAML scaffolds for Dispatcher |

#### Sprint 3 (Weeks 5–6) | DEV-1: 64h (80%) · DEV-2: 64h (80%) · LEAD: 52h (81%)

| Week | DEV-1 (Accounting_WS + DataTransform + Posting) | DEV-2 (DataTransform + Reporting + Posting + Sentinel) | LEAD (Review + Sentinel + Test) | AGENT |
|------|------------------------------------------------|-------------------------------------------------------|-------------------------------|-------|
| **5** | MergeBB_LotLevel · Aggregate_Filter · BGIR_WS_Call ♻️ MCH_BGIR · BGI_Loop · Write_ToExcel ♻️ · Posting CreateInstance/DeleteSheet/CreateSheet/Check | Filter_Calculate · Set_Column_Data · Change_Column_Position ♻️ · CreateReport · CreateWorksheetAndWriteCollection · WriteDailyReport · Posting Main_Action | Code review S2/S3 · Sentinel oversight · Integration pre-testing | XAML scaffolds for Posting + Sentinel |
| **6** | FR.PYDN_Posting ♻️ MCH_PYDN · FR.PYDN_Posting_UMAS · DataExtraction 5 of 7 workflows ♻️ Dispatcher REF · MCH regression | FR.PYUP_Posting ♻️ · FR.PYDA_Posting ♻️ · FR.PYUP_UMAS + FR.PYDA_UMAS ♻️ · Sentinel_Process ♻️ · MCH regression | Exception matrix validation · UMAS DLL test · Sprint 4 prep · Coordination | Remaining scaffolds |

#### Sprint 4 (Weeks 7–8) 🚀 | DEV-1: 64h (80%) · DEV-2: 64h (80%) · LEAD: 52h (81%)

| Week | DEV-1 (TransformPosting + Testing) | DEV-2 (Support + Testing) | LEAD (E2E + UAT + Deploy) | AGENT |
|------|-------------------------------------|---------------------------|---------------------------|-------|
| **7** | TransformPosting 5 workflows ♻️ DExt · TP unit test · SLAMonitor · **DataExtraction 2 deferred** (ExtractData_DE + InitAll/CloseAll) · Dispatcher→DE test · DE→TP test · Exception regression | Dispatcher_Adhoc ♻️ · Dispatcher_NOVAL ♻️ · DatabaseCleanup ♻️ DB · **Sentinel 3 deferred** (CheckConnectivity + ValidateSchedule + CheckDiskSpace/Notify/CleanUp) · Dispatcher→DE test · DE→TP test · Exception regression | E2E 3-sample test · E2E pipeline oversight · Code review TransformPosting · UAT start | Gen Project Plan, Agent Util, Timeline |
| **8** | Orch publish/deploy · Orch schedules · Production smoke test · Documentation | UAT support · Deployment support · Cross-process regression · Defect fixing | UAT sign-off · NuPkg publish · **Deployment** · **Production smoke test** · SOP review · Post-deployment monitoring | SOP/runbook + Utilization Summary |

### 4.3 Timeline vs. Original Savings

| Scenario | Duration | Sprints | Total Hours | Difference |
|---|---|---|---|---|
| No reuse (build everything from scratch) | 10 weeks | 5 sprints | ~936h | Baseline |
| **Committed Plan (with reuse)** | **8 weeks** | **4 sprints** | **~776h** | **−2 weeks · −1 sprint · −160h (17% saved)** |
| Pessimistic (with delays) | 10 weeks | 5 sprints | ~936h | +0 vs. scratch |
| Best case (no blockers) | 7 weeks | 3.5 sprints | ~630h | −3 weeks |

---

## Section 5 — Dependencies & Critical Path

**Critical Path:** Sprint 1 (M1+M2+M3) → Sprint 2 MCH PYDN/PYUP/PYDA → Sprint 3 Posting Library → Sprint 4 TransformPosting → UAT → Go-Live.

The longest dependency chain is: MCH_Login → MCH_PYDN → GIO.FR.Posting.Library → 029_FR_Performer_TransformPosting → UAT → Production. Any slip in MCH screen development (Sprint 2, M4) directly impacts TransformPosting (Sprint 4, M9/M10).

**External Dependencies:**

| Dependency | Owner | Required By | Risk if Delayed |
|---|---|---|---|
| MCH test environment access | MCH Team | Sprint 1 Week 1 | Cannot capture MCH selectors; blocks all MCH workflows |
| RDR Cloud Client DLL version confirmation | RDR/Integration Team | Sprint 2 start | Cannot build RDR extraction; may need REST fallback |
| UMAS DLL .NET version confirmation | UMAS Team | Sprint 3 start | Cannot build UMAS posting variants; impacts M6 |
| CyberArk safe configuration for robot | Platform Engineering | Sprint 3 end | Cannot test credential retrieval in DEV |
| Bloomberg API endpoint and auth type | Bloomberg/Operations | Sprint 2 start | Cannot build Bloomberg extraction (S2-07) |
| Business test data samples (3 fund lists) | Business / GIO COE | Sprint 4 start | Cannot run E2E test (M9) |
| UAT window availability | Business / GIO COE | Sprint 4 Week 7 | Cannot sign off M10; delays go-live |
| Orchestrator prod access for deployment | Platform Engineering | Sprint 4 Week 8 | Cannot complete M11/M12 |

---

## Section 6 — Risk Register

| # | Risk | Prob | Impact | Mitigation | Owner |
|---|---|---|---|---|---|
| R-01 | MCH test environment unavailable in Sprint 1 | Medium | High | Pre-book MCH test environment before Sprint 1 starts (S1-07 dependency); use screenshots from BP as placeholder for selector development | LEAD-1 |
| R-02 | RDR Cloud Client DLL incompatible with UiPath .NET 6 | Medium | High | Test DLL in S1 sandbox before committing to library design; prepare REST HTTP fallback in parallel (S2-08) | DEV-2 |
| R-03 | UMAS DLL incompatible with target .NET version | Medium | Medium | Test UMAS DLL in Sprint 2 sandbox; plan COM interop wrapper; raise risk with UMAS team by end of Sprint 2 (before S3-08) | DEV-2 + LEAD-1 |
| R-04 | Bloomberg API authentication changes (key rotation/MFA) | Low | High | Confirm Bloomberg auth type with integration team before S2-07; implement API key as Orchestrator Asset (029_FR_Bloomberg_Credentials) | DEV-1 |
| R-05 | MCH PYDN/PYUP/PYDA screen selectors break due to AVIVA terminal version difference | Low | High | Capture all MCH selectors against BP 7.3 AVIVA version; test in Sprint 2 as soon as MCH test access granted | DEV-1 + DEV-2 |
| R-06 | Business test data not available for E2E testing by Sprint 4 | Medium | Medium | Request 3 sample fund list files from GIO COE team by end of Sprint 3 (M8) | LEAD-1 |
| R-07 | Posting Library complexity exceeds estimate (UMAS variant behavior unknown) | Medium | High | Timebox UMAS development: if S3-08 exceeds 20h, descope UMAS for go-live 1, add as post-go-live Phase 2 | LEAD-1 |
| R-08 | CyberArk safe not set up before Sprint 4 testing | Low | High | Engage Platform Engineering in Sprint 1 (S1-05 includes pre-registering robot service account) | LEAD-1 + Platform |
| R-09 | GIO.FR.DataTransformation logic not fully captured in BP XML | Medium | Medium | Schedule a Business Requirements Walk-Through with GIO SME in Sprint 1 to review Transform logic rules | LEAD-1 |
| R-10 | Holiday calendar format differs from assumption across all 4 regions | Medium | Medium | Obtain real Master Scheduler file in Sprint 1 for format validation before S2-05 | DEV-1 |

---

## Section 7 — Agent Utilization Summary

| Sprint | Agent Hours | Agent Tasks | Agent % |
|---|---|---|---|
| Sprint 1 | ~40h | BRD (029-BRD-001), TDD (029-TDD-001), RC (029-RC-001), Plan (029-PLAN-001), Dev Guide (029-DEV-001), XAML scaffolds for all Step 0–5 workflows, Config.xlsx rows | ~65% of doc effort |
| Sprint 2 | ~8h | XAML scaffolds for MCH screen workflows, Accounting_WS workflows, Dispatcher workflows | ~15% |
| Sprint 3 | ~8h | XAML scaffolds for DataTransformation, Posting, Reporting library workflows | ~15% |
| Sprint 4 | ~24h | Project Plan (029-PP-001), Agent Utilization (029-AU-001), Honest Timeline Assessment (029-TA-001), Utilization Summary (029-US-001), SOP/runbook drafts | ~50% of doc effort |

*See 029_Agent_Utilization.md for full details.*

---

## Section 8 — Definition of Done

- [ ] Code complete and checked into source control
- [ ] Peer reviewed by LEAD or senior developer
- [ ] Unit tested with passing test cases documented in Dev Guide
- [ ] No hardcoded values (all from Config.xlsx or Orchestrator Assets)
- [ ] No PII or credentials exposed in any log output
- [ ] Exception handling wired per exception matrix (TDD Section 6)
- [ ] Logging wired per TDD Section 7 (WriteProcessLog and WriteTransactionLog)
- [ ] Naming conventions followed per TDD Section 10
- [ ] Demonstrated to LEAD in Studio
- [ ] Story marked Completed in Jira/backlog by DEV on same day it is finished

---

# REUSABLE COMPONENTS – ZERO EFFORT IN THIS PLAN

## 9.1 All Zero-Effort Reuse — Sourced from 058 – Market Profiling and 057 – Ben Profiling

### Group A: Libraries (NuGet — Zero Effort to Build)

| # | Component | Workflows | Effort in Source Project | 029 Effort | Hours Saved | Used By Sprint |
|---|---|---|---|---|---|---|
| 1 | SSC.Platform.Logging | WriteProcessLogs, WriteTransactionLogs, InitializeLogger (3 workflows) | 20h (058) | **0h** | 20h | Sprint 1 |
| 2 | SSC.Platform.LoginAgent | GetSubIDPassword, ValidateCredential (2 workflows) | 16h (058) | **0h** | 16h | Sprint 1 |
| 3 | SSC.Custom.Environment | GetDownloadsDefaultPath, GetMachineName (2 workflows) | 8h (058) | **0h** | 8h | Sprint 1 |
| 4 | BOAT.COB.Excel.Utilities | MergeExcelFiles, ReadWithOLEDB, SetTitusClassification (3 workflows) | 24h (058) | **0h** | 24h | Sprint 1 |
| | **Library Subtotal** | **10 workflows** | **68h** | **0h** | **68h** | |

### Group B: Sprint Artifacts (Workflows/Configs — Zero Effort to Import)

| Story ID | Description | Effort in 057 | 029 Effort | Hours Saved | Sprint Used |
|---|---|---|---|---|---|
| S1-01 | REFramework template (8 workflows) | 24h (057) | 0h | 24h | Sprint 1 |
| S1-01 | Config.xlsx template (Settings/Constants/Assets structure) | 8h (057) | 0h | 8h | Sprint 1 |
| S1-03 | Common workflows (8 files: GetCredential, SendEmail, WriteProcessLog, etc.) | 32h (057) | 0h | 32h | Sprint 1 |
| S1-04 | WorkQueue wrappers (9 files: AddToQueue, MarkCompleted, etc.) | 28h (057) | 0h | 28h | Sprint 1 |
| | **Artifact Subtotal** | **92h** | **0h** | **92h** | |

### Combined Zero-Effort Total

| Group | Components | Original Investment | 029 Effort | Hours Saved |
|---|---|---|---|---|
| Group A: Libraries | 4 libraries / 10 workflows | 68h | **0h** | **68h** |
| Group B: Artifacts | 4 artifacts / 35+ files | 92h | **0h** | **92h** |
| **GRAND TOTAL** | | **160h** | **0h** | **160h** |

## 9.2 Savings Summary

### Savings at a Glance

| Metric | Without Reuse | With Reuse | Saved |
|---|---|---|---|
| Total Effort (hours) | ~936h | ~776h | **~160h (17% reduction)** |
| Duration | 10 weeks | 8 weeks | **2 weeks saved** |
| Sprints | 5 | 4 | **1 sprint saved** |
| Developer Cost (@ $X/hr) | 936 × $X | 776 × $X | **160 × $X** |

### Sprint-Level Impact — How Freed Capacity Was Redeployed

| Sprint | Hours Freed by 0-Effort | Hours Redeployed To |
|---|---|---|
| Sprint 1 | ~92h (imports, REFramework, Config) | MCH Library login. DB Library. MasterScheduler 3 workflows. Full BRD/TDD review |
| Sprint 2 | 0h (all new build) | Full 168h on MCH screens + Accounting_WS Library |
| Sprint 3 | 0h (all new build) | Full 168h on Posting library + performers |
| Sprint 4 | ~8h (Adhoc/NOVAL dispatcher pattern reuse) | Additional E2E test coverage and exception regression testing |

### Cost Savings Table

| Metric | Without Reuse | With Reuse | Savings |
|---|---|---|---|
| Duration | 10 weeks | 8 weeks | **2 weeks (20%)** |
| Total Human Hours | ~856 hrs | ~696 hrs | **~160 hrs (19%)** |
| Sprints | 5 | 4 | **1 sprint (20%)** |
| Developer Cost (@ $X/hr) | 856 × $X | 696 × $X | **160 × $X** |

> **Key Insight:** The 4 reusable NuGet libraries save **68 hours** plus 057 sprint artifacts save **92 hours** = **160 hours total**. They were fully funded by **058 Market Profiling** and **057 Ben Profiling**. Every subsequent COB project gets the same benefit at zero cost, creating compounding ROI across the portfolio.

### Prerequisites for Reuse

| Prerequisite | Status | Owner |
|---|---|---|
| SSC.Platform.Logging NuGet on internal feed | Available from 058 | Platform/DEV-1 |
| SSC.Platform.LoginAgent NuGet on internal feed | Available from 058 | Platform/DEV-1 |
| SSC.Custom.Environment NuGet on internal feed | Available from 058 | Platform/DEV-1 |
| BOAT.COB.Excel.Utilities NuGet on internal feed | Available from 058 | Platform/DEV-1 |
| 057 Ben Profiling source code accessible | Workspace available | LEAD-1 |
| 058 Market Profiling source code accessible | Workspace available | LEAD-1 |
| CyberArk safe registered for 029 robot | Requires Platform Eng setup | LEAD-1 by Sprint 1 end |

---

## Section 10 — Lead Role — 81% Utilization Breakdown

The Lead is a **full active contributor** at 81% utilization every sprint (52h planned / 64h available), not a passive reviewer. The remaining 12h/sprint is contingency buffer for unexpected issues.

| Category | Hours | % of Lead Total |
|----------|-------|----------------|
| **Code Review** (all sprint PRs + library reviews) | 50h | 24% |
| **Integration Testing** (E2E pipeline, MCH validation, queue flows) | 32h | 15% |
| **Orchestrator & Governance** (assets, queues, schedules, CyberArk) | 28h | 13% |
| **Architecture & Design** (TDD review, Posting design, Sprint prep) | 24h | 12% |
| **BRD/TDD Review & Sign-off** | 18h | 9% |
| **Deployment & Go-Live** (publish, smoke test, monitoring) | 18h | 9% |
| **External Dependency Coordination** (RDR DLL, UMAS DLL, Bloomberg, CyberArk) | 16h | 8% |
| **Sprint Ceremonies & Coordination** | 16h | 8% |
| **UAT Support** | 6h | 3% |
| **Total** | **208h** | **100%** |

> **Lead builds zero workflows directly** in this project (unlike 058 where Lead built ~45 sub-workflows). Lead capacity is fully consumed by review, integration testing, and governance across 7 libraries and 8 processes. This is appropriate given the higher MCH screen complexity (7 project-specific screens vs. 058's reusable MCH library).

---

## Section 11 — Cost Summary

| Category | Hours | % of Total |
|----------|-------|------------|
| **Development (DEV-1 + DEV-2)** | 488h | 62.9% |
| **Lead (review + test + govern + deploy)** | 208h | 26.8% |
| **AI Agent** | 80h | 10.3% |
| **Grand Total** | **776h** | **100%** |
| **Calendar Duration** | **8 weeks (4 sprints × 2 weeks)** | |

### Per-Sprint Cost Breakdown

| Sprint | Focus | DEV-1 | DEV-2 | LEAD | Agent | Total | % |
|--------|-------|-------|-------|------|-------|-------|-----|
| S1 (Foundation + MCH Login + DB + MasterScheduler) | Libraries + Import | 52h | 52h | 52h | 40h | 196h | 25.3% |
| S2 (MCH Complete + Accounting_WS + Dispatcher) | MCH + Extraction | 64h | 64h | 52h | 8h | 188h | 24.2% |
| S3 (Posting + DataTransformation + DataExtraction + Sentinel) | Core Build | 64h | 64h | 52h | 8h | 188h | 24.2% |
| S4 (TransformPosting + Integration + UAT + Deploy) | Testing + Deploy | 64h | 64h | 52h | 24h | 204h | 26.3% |
| **TOTAL** | **All** | **244h** | **244h** | **208h** | **80h** | **776h** | **100%** |

> **Sprints balanced at 80%–81% utilization for humans. Agent peaks in Sprint 1 (40h — documentation heavy) and Sprint 4 (24h — closure documents). Reuse savings of ~160h enable aggressive 4-sprint schedule within 80% capacity.**

### Cost Savings

| Metric | Without Reuse | With Reuse | Savings |
|--------|--------------|------------|---------|
| Duration | 10 weeks | 8 weeks | **2 weeks (20%)** |
| Total Human Hours | ~856 hrs | ~696 hrs | **~160 hrs (19%)** |
| Sprints | 5 | 4 | **1 sprint (20%)** |
| Developer Cost (@ $X/hr) | 856 × $X | 696 × $X | **160 × $X** |
