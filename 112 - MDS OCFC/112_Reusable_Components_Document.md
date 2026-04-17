# 112 MDS OCFC — Reusable Components Document

| Field | Value |
|---|---|
| Document ID | 112-RC-001 |
| Project | MDS.112 – COB MDS |
| Source | 112_Technical_Design_Document.md + comparison against 058, 057, 029 |
| Date | 2026-04-13 |
| Status | Draft v1.0 |

---

## 1. Component Classification Table

| # | Component | Type | Scope | Source Project | Used By Projects | Build-Once Reuse |
|---|---|---|---|---|---|---|
| RC-01 | BOAT.COB.MCH.Library (Core) | NuGet Library | Cross-Project | 058 – Market Profiling | 057, 058, 112 | Yes |
| RC-02 | BOAT.COB.MAP.Library | NuGet Library | Cross-Project | 058 – Market Profiling | 057, 058 | Yes (not used in 112) |
| RC-03 | SSC.Platform.Logging | NuGet Library | Cross-Project | 058 – Market Profiling | 057, 058, 112 | Yes |
| RC-04 | SSC.Platform.LoginAgent | NuGet Library | Cross-Project | 058 – Market Profiling | 057, 058, 112 | Yes |
| RC-05 | SSC.Custom.Environment | NuGet Library | Cross-Project | 058 – Market Profiling | 057, 058, 112 | Yes |
| RC-06 | BOAT.COB.Excel.Utilities | NuGet Library | Cross-Project | 058 – Market Profiling | 057, 058 | Yes (not used in 112) |
| RC-07 | REFramework + Config.xlsx | Project Template | Cross-Project | 058 – Market Profiling | 057, 058, 112 | Yes |
| RC-08 | Common Workflows | Workflow Set | Cross-Project | 058 – Market Profiling | 057, 058, 112 | Yes |
| RC-09 | WorkQueue Wrappers | Workflow Set | Cross-Project | 058 – Market Profiling | 057, 058, 112 | Yes |
| RC-10 | Exception Handling Strategy | Pattern/Template | Cross-Project | 058 – Market Profiling | 057, 058, 112 | Yes |
| RC-11 | Orchestrator Dev Config | Configuration | Cross-Project | 058 – Market Profiling | 057, 058, 112 | Yes |
| RC-12 | MCH 112-Specific Screens | Library Extension | Project-Specific | **112 – MDS OCFC (NEW)** | 112 | Potential for future |

---

## 2. Detailed Specs per Component

### RC-01: BOAT.COB.MCH.Library (Core — 7 Workflows Reusable for 112)

| # | Workflow | Size | 058 Effort | 112 Effort | Notes |
|---|---|---|---|---|---|
| 1 | MCH_Launch.xaml | XS | 4h | **0h** | Terminal launch and attach |
| 2 | MCH_Login.xaml | S | 8h | **0h** | RACF login with CyberArk |
| 3 | MCH_CheckIfLoggedIn.xaml | XS | 4h | **0h** | Session state check |
| 4 | MCH_NavigateToTransaction.xaml | XS | 4h | **0h** | Screen navigation |
| 5 | MCH_CloseApplication.xaml | XS | 4h | **0h** | Terminal close |
| 6 | MCH_CleanUp.xaml | XS | 4h | **0h** | Session cleanup |
| 7 | MCH_RetrievePassword.xaml | XS | 4h | **0h** | CyberArk password retrieval |
| | **Subtotal** | | **32h** | **0h** | |

> **Note:** 058 MCH Library has 14 workflows total (including BLAC, GGBE, BCMC, BHDR, BGTT screen workflows). Only the 7 core/infrastructure workflows apply to 112. The screen-specific workflows (BFSE, BSEC, BGOV) are NEW for 112 and require separate build effort.

### RC-02: BOAT.COB.MAP.Library (25 Workflows — Not Used in 112)

The MAP library is not required for MDS OCFC as the automation does not interact with the MAP system. **0 import effort, 0 usage.**

### RC-03: SSC.Platform.Logging (3 Workflows)

| # | Workflow | Size | 058 Effort | 112 Effort |
|---|---|---|---|---|
| 1 | WriteProcessLogs.xaml | S | 8h | **0h** |
| 2 | WriteTransactionLogs.xaml | S | 8h | **0h** |
| 3 | InitializeLogger.xaml | XS | 4h | **0h** |
| | **Subtotal** | | **20h** | **0h** |

### RC-04: SSC.Platform.LoginAgent (2 Workflows)

| # | Workflow | Size | 058 Effort | 112 Effort |
|---|---|---|---|---|
| 1 | GetSubIDPassword.xaml | S | 8h | **0h** |
| 2 | ValidateCredential.xaml | S | 8h | **0h** |
| | **Subtotal** | | **16h** | **0h** |

### RC-05: SSC.Custom.Environment (2 Workflows)

| # | Workflow | Size | 058 Effort | 112 Effort |
|---|---|---|---|---|
| 1 | GetDownloadsDefaultPath.xaml | XS | 4h | **0h** |
| 2 | GetMachineName.xaml | XS | 4h | **0h** |
| | **Subtotal** | | **8h** | **0h** |

### RC-06: BOAT.COB.Excel.Utilities (Not Used in 112)

Not required for MDS OCFC. **0 import effort, 0 usage.**

### RC-07: REFramework + Config.xlsx

| Item | 058 Effort | 112 Effort |
|---|---|---|
| REFramework template clone | 16h | **0h** |
| Config.xlsx structure (Settings, Constants, Assets sheets) | 8h | **0h** |
| **Subtotal** | **24h** | **0h** |

### RC-08: Common Workflows (6 Workflows)

| # | Workflow | Size | 058 Effort | 112 Effort |
|---|---|---|---|---|
| 1 | GetUserName.xaml | XS | 2h | **0h** |
| 2 | GetCredential.xaml | XS | 2h | **0h** |
| 3 | GetStartUpUTCDatetime.xaml | XS | 2h | **0h** |
| 4 | WriteProcessLog.xaml | XS | 2h | **0h** |
| 5 | WriteTransactionLog.xaml | XS | 2h | **0h** |
| 6 | KillExcelProcess.xaml | XS | 2h | **0h** |
| | **Subtotal** | | **12h** | **0h** |

### RC-09: WorkQueue Wrappers (6 Workflows)

| # | Workflow | Size | 058 Effort | 112 Effort |
|---|---|---|---|---|
| 1 | TagItem.xaml | XS | 2h | **0h** |
| 2 | UntagItem.xaml | XS | 2h | **0h** |
| 3 | DeferItem.xaml | S | 4h | **0h** |
| 4 | CompleteItem.xaml | XS | 2h | **0h** |
| 5 | ExceptionItem.xaml | XS | 2h | **0h** |
| 6 | AddToQueue.xaml | S | 4h | **0h** |
| | **Subtotal** | | **16h** | **0h** |

### RC-10: Exception Handling Strategy

| Item | 058 Effort | 112 Effort |
|---|---|---|
| Exception matrix design | 8h | **0h** |
| Business/System exception patterns | 4h | **0h** |
| **Subtotal** | **12h** | **0h** |

### RC-11: Orchestrator Dev Config

| Item | 058 Effort | 112 Effort |
|---|---|---|
| Folder structure setup | 4h | **0h** |
| Queue naming convention | 2h | **0h** |
| Asset naming convention | 2h | **0h** |
| **Subtotal** | **8h** | **0h** |

---

## 3. Effort Summary

### 3.1 Reusable Components — Zero Effort for 112

| # | Component | Workflows | Build Effort (058) | 112 Effort | Hours Saved |
|---|---|---|---|---|---|
| RC-01 | MCH Core (7 WF) | 7 | 32h | **0h** | 32h |
| RC-03 | Logging | 3 | 20h | **0h** | 20h |
| RC-04 | LoginAgent | 2 | 16h | **0h** | 16h |
| RC-05 | Environment | 2 | 8h | **0h** | 8h |
| RC-07 | REFramework + Config | — | 24h | **0h** | 24h |
| RC-08 | Common Workflows | 6 | 12h | **0h** | 12h |
| RC-09 | WorkQueue Wrappers | 6 | 16h | **0h** | 16h |
| RC-10 | Exception Strategy | — | 12h | **0h** | 12h |
| RC-11 | Orchestrator Config | — | 8h | **0h** | 8h |
| | **TOTAL** | **26** | **148h** | **0h** | **148h** |

### 3.2 Not-Used Components (available but not needed in 112)

| Component | 058 Effort | 112 Usage | Reason |
|---|---|---|---|
| MAP Library (25 WF) | 120h | Not used | MDS OCFC does not interact with MAP |
| Excel Utilities (3 WF) | 24h | Not used | No Excel merge/OLEDB operations in OCFC |

### 3.3 Overall Savings

| Metric | Value |
|---|---|
| Components reused | 9 (26 workflows + patterns/config) |
| Hours paid by 058 | 148h |
| 112 Effort for reused components | **0h** |
| **Total hours saved for 112** | **148h** |
| Equivalent sprint savings | ~0.8 sprint (148h ÷ 180h/sprint) |

---

## 4. Savings for Subsequent Projects

### 4.1 New Reusable Components Introduced by 112

Project 112 introduces these components that future projects can reuse:

| # | Component | Workflows | Build Effort (112) | Future Project Effort | Savings per Reuse |
|---|---|---|---|---|---|
| 1 | RKS Library | 35 | ~160h | 0h | 160h |
| 2 | HTTP/API Library Pattern | 9 | ~80h | ~20h (adapt endpoints) | 60h |
| 3 | ESP Library | 3 | ~40h | 0h | 40h |
| 4 | DB Interactions + Locking | 2 | ~24h | 0h | 24h |
| 5 | Business Logic Patterns (3019/3334/3077/3122/3917) | 32 | ~160h | ~40h (new check codes) | 120h |
| 6 | Collective Notification + Mail Wrapper | 2 | ~16h | 0h | 16h |
| | **TOTAL** | **83** | **~480h** | | **~420h per project** |

### 4.2 ROI Table

| Project | Builds | Reuses | Build Cost | Reuse Cost | Cumulative Savings |
|---|---|---|---|---|---|
| 058 (Pioneer) | MCH, MAP, Logging, LoginAgent, Environment, Excel, REF, Common, WQ | — | 268h | 0 | 0 |
| 057 | — | All from 058 | 0 | 0 | 268h |
| **112 (This Project)** | RKS, HTTP, ESP, DB, BusinessLogic, Notification | MCH Core, Logging, LoginAgent, Environment, REF, Common, WQ | ~480h new | 0h (148h reused) | 148h + 268h = 416h total |
| Future Project A | — | All from 058 + 112 | 0 | ~60h adapt | 148h + 420h = 568h |
| Future Project B | — | Same | 0 | ~60h adapt | 568h × 2 = 1,136h |

---

## 5. Assumptions for Reusable Components

1. 058 MCH core workflows (Launch, Login, Navigate, Close, CleanUp, CheckIfLoggedIn, RetrievePassword) are functionally compatible with 112's MCH mainframe environment (same RACF authentication, same terminal emulator)
2. SSC.Platform.Logging format is compatible with 112's Splunk integration requirements
3. REFramework template from 058 requires only Config.xlsx customization for 112
4. WorkQueue wrapper patterns apply to all 26 queues in 112
5. CyberArk integration approach from 058 (LoginAgent) works for all 12 credential domains in 112
6. Future projects using RKS library from 112 will have similar Citrix/browser environment

---

## 6. Recommendations

1. **Publish MCH Core as NuGet Package** — The 7 core MCH workflows should be published as a versioned NuGet package (`BOAT.COB.MCH.Core`) separate from project-specific screen workflows
2. **Publish RKS Library as NuGet** — After 112 is stable, publish RKS Library as `BOAT.COB.RKS.Library` for any future MDS project
3. **Create HTTP Template Library** — The 9 HTTP workflows share a common pattern (login → session → query → parse). Extract the pattern into a template for rapid API integration
4. **Version Control** — All reusable libraries should be in separate Git repositories with semantic versioning
5. **DB Interactions as Platform Library** — DB_Interactions and DB_Locking are generic enough to become `SSC.Platform.Database` for cross-project reuse
6. **Business Logic Pattern Library** — The check code pattern (3019/3334/3077/3122/3917) could become a configurable framework for any securities validation project
