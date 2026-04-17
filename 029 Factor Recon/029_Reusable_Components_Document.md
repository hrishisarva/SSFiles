# 029 – REUSABLE COMPONENTS DOCUMENT

| Field | Value |
|---|---|
| Document ID | 029-RC-001 |
| Project | BOAT.029 – COB FACTOR RECON |
| Source | 029_Technical_Design_Document.md + 058 – Market Profiling + 057 – Ben Profiling |
| Date | 2026-04-08 |
| Status | Draft v1.0 |

---

## 1. Component Classification Table

| # | Component | Type | Build Sprint | Used By Projects | Scope | 029 Build Effort |
|---|---|---|---|---|---|---|
| RC-01 | SSC.Platform.Logging | NuGet Library | 058 Sprint 1 | 057, 058, 029+ | Platform-wide | **0h** |
| RC-02 | SSC.Platform.LoginAgent | NuGet Library | 058 Sprint 1 | 057, 058, 029+ | Platform-wide | **0h** |
| RC-03 | SSC.Custom.Environment | NuGet Library | 058 Sprint 1 | 057, 058, 029+ | Platform-wide | **0h** |
| RC-04 | BOAT.COB.Excel.Utilities | NuGet Library | 058 Sprint 1 | 057, 058, 029+ | COB-wide | **0h** |
| RC-05 | REFramework Template | Framework Folder | 058/057 | 057, 058, 029+ | All UiPath projects | **0h** |
| RC-06 | Config.xlsx Template | Data File | 058/057 | 057, 058, 029+ | All UiPath projects | **0h** |
| RC-07 | Common Workflows | Workflow Folder | 057 Sprint 1 | 057, 058, 029+ | All projects | **0h** |
| RC-08 | WorkQueue Wrappers | Workflow Folder | 057 Sprint 2 | 057, 058, 029+ | All projects | **0h** |
| RC-09 | GIO.FR.MCH.Library | NuGet Library | **029 Sprint 2** | **029 only** | FR-specific | 80h (build new) |
| RC-10 | GIO.FR.Accounting_WS.Library | NuGet Library | **029 Sprint 2** | **029 only** | FR-specific | 70h (build new) |
| RC-11 | GIO.FR.DataTransformation.Library | NuGet Library | **029 Sprint 3** | **029 only** | FR-specific | 40h (build new) |
| RC-12 | GIO.FR.Database.Library | NuGet Library | **029 Sprint 2** | **029 only** | FR-specific | 12h (build new) |
| RC-13 | GIO.FR.MasterScheduler.Library | NuGet Library | **029 Sprint 2** | **029 only** | FR-specific | 35h (build new) |
| RC-14 | GIO.FR.Reporting.Library | NuGet Library | **029 Sprint 3** | **029 only** | FR-specific | 25h (build new) |
| RC-15 | GIO.FR.Posting.Library | NuGet Library | **029 Sprint 3** | **029 only** | FR-specific | 120h (build new) |

---

## 2. Detailed Specifications — Reused Components (0 Effort)

### RC-01: SSC.Platform.Logging
**Source Project:** 058 – Market Profiling  
**Import Instructions:** Add NuGet dependency in project.json; import DLL from 058 NuGet feed

| Workflow | Description | Original Complexity |
|---|---|---|
| WriteProcessLogs | Write process-level events to ELK/Splunk | S (4–8h) |
| WriteTransactionLogs | Write transaction events with queue item reference | S (4–8h) |
| InitializeLogger | Configure logger with session context | S (4–8h) |

**Original Build Effort:** 20h (058) | **029 Effort: 0h** | **Savings: 20h**

---

### RC-02: SSC.Platform.LoginAgent
**Source Project:** 058 – Market Profiling  
**Import Instructions:** Add NuGet dependency; configure CyberArk safe name and object IDs in Assets

| Workflow | Description | Original Complexity |
|---|---|---|
| GetSubIDPassword | Retrieve SubID + password from CyberArk vault | M (8–16h) |
| ValidateCredential | Test credential validity before use | S (4–8h) |

**Original Build Effort:** 16h (058) | **029 Effort: 0h** | **Savings: 16h**

---

### RC-03: SSC.Custom.Environment
**Source Project:** 058 – Market Profiling  
**Import Instructions:** Add NuGet dependency

| Workflow | Description | Original Complexity |
|---|---|---|
| GetDownloadsDefaultPath | Get robot's Downloads folder path | XS (≤4h) |
| GetMachineName | Get robot machine name for logging | XS (≤4h) |

**Original Build Effort:** 8h (058) | **029 Effort: 0h** | **Savings: 8h**

---

### RC-04: BOAT.COB.Excel.Utilities
**Source Project:** 058 – Market Profiling  
**Import Instructions:** Add NuGet dependency

| Workflow | Description | Original Complexity |
|---|---|---|
| MergeExcelFiles | Merge multiple Excel files into one workbook | M (8–16h) |
| ReadWithOLEDB | Fast Excel data read via OLEDB driver | M (8–16h) |
| SetTitusClassification | Apply Titus DLP classification to Excel files | S (4–8h) |

**029 Usage:** ReadWithOLEDB used by GetFundList and ReadMasterScheduler; SetTitusClassification applied to all output reports.  
**Original Build Effort:** 24h (058) | **029 Effort: 0h** | **Savings: 24h**

---

### RC-05: REFramework Template
**Source Project:** 057 – Ben Profiling  
**Import Instructions:** Copy entire `Framework/` folder from 057 project; update Config.xlsx queue names only

| Workflow | Description |
|---|---|
| InitAllSettings.xaml | Load Config.xlsx + Orchestrator Assets into dictionary |
| InitAllApplications.xaml | Launch/initialize all required applications |
| KillAllProcesses.xaml | Force-kill all applications on error |
| CloseAllApplications.xaml | Graceful close of all applications |
| GetTransactionData.xaml | Dequeue next work item |
| Process.xaml | Invoke performer workflow |
| SetTransactionStatus.xaml | Mark transaction Complete/Exception |
| RetryCurrentTransaction.xaml | Retry with counter management |

**Original Build Effort:** 24h (057) | **029 Effort: 0h** | **Savings: 24h**

---

### RC-06: Config.xlsx Template
**Source Project:** 057 – Ben Profiling  
**Import Instructions:** Copy Config.xlsx from 057; update sheet rows for 029 asset names and queue names

**Original Build Effort:** 8h (057) | **029 Effort: 0h (customize only — 1h config, charged to setup story)** | **Savings: 8h**

---

### RC-07: Common Workflows (Reusable/Common/)
**Source Project:** 057 – Ben Profiling  
**Import Instructions:** Copy `Reusable/Common/` folder from 057; no code changes required

| Workflow | Purpose |
|---|---|
| GetCredential.xaml | Wrapper around LoginAgent for credential retrieval |
| WriteProcessLog.xaml | Wrapper around Logging.WriteProcessLogs |
| WriteTransactionLog.xaml | Wrapper around Logging.WriteTransactionLogs |
| SendEmail.xaml | SMTP email with To, CC, Subject, Body, Attachment args |
| KillExcelProcess.xaml | Terminate orphaned Excel.exe processes |
| GetUserName.xaml | Get domain\\username for audit trails |
| GetStartUpUTCDatetime.xaml | Get process start time in UTC |
| OleDbGetCollection.xaml | Execute OLEDB query and return DataTable |

**Original Build Effort:** 32h (057) | **029 Effort: 0h** | **Savings: 32h**

---

### RC-08: WorkQueue Wrapper Workflows (Reusable/WorkQueue/)
**Source Project:** 057 – Ben Profiling  
**Import Instructions:** Copy `Reusable/WorkQueue/` folder from 057; update queue names in Config only

| Workflow | Purpose |
|---|---|
| WQ_AddToQueue.xaml | Add one or more items to Orchestrator Queue |
| WQ_GetNextItem.xaml | Get next pending item from specified queue |
| WQ_MarkCompleted.xaml | Mark queue item as Successful |
| WQ_MarkException.xaml | Mark queue item as exception with reason |
| WQ_SetData.xaml | Update queue item's Specific Content |
| WQ_Defer.xaml | Defer queue item to deferred date |
| WQ_TagItem.xaml | Add custom tag to queue item |
| WQ_UntagItem.xaml | Remove tag from queue item |
| WQ_GetPendingItems.xaml | Query all pending items in a queue |

**Original Build Effort:** 28h (057) | **029 Effort: 0h** | **Savings: 28h**

---

## 3. Discovered Reusable Components Table

> Scanned: `058 – Market Profiling` (REF_FOLDER_1) and `057 – Ben Profiling` (REF_FOLDER_2)

| # | Component | Found In | File / Library | Original Build Effort | 029 Effort | Hours Saved |
|---|---|---|---|---|---|---|
| 1 | SSC.Platform.Logging | 058 – Market Profiling | `SSC.Platform.Logging` NuGet | 20h | **0** | 20h |
| 2 | SSC.Platform.LoginAgent | 058 – Market Profiling | `SSC.Platform.LoginAgent` NuGet | 16h | **0** | 16h |
| 3 | SSC.Custom.Environment | 058 – Market Profiling | `SSC.Custom.Environment` NuGet | 8h | **0** | 8h |
| 4 | BOAT.COB.Excel.Utilities | 058 – Market Profiling | `BOAT.COB.Excel.Utilities` NuGet | 24h | **0** | 24h |
| 5 | REFramework Template | 057 – Ben Profiling | `Framework/*.xaml` (8 files) | 24h | **0** | 24h |
| 6 | Config.xlsx Template | 057 – Ben Profiling | `Data/Config.xlsx` | 8h | **0** | 8h |
| 7 | Common Workflows | 057 – Ben Profiling | `Reusable/Common/*.xaml` (8 files) | 32h | **0** | 32h |
| 8 | WorkQueue Wrappers | 057 – Ben Profiling | `Reusable/WorkQueue/*.xaml` (9 files) | 28h | **0** | 28h |
| | **TOTALS** | | | **160h** | **0h** | **160h** |

**Note on BOAT.COB.MAP.Library:** Not included in savings — 029 Factor Recon has no MAP system integration. MAP library (120h, built in 058) is not applicable to this project.

**Note on BOAT.COB.MCH.Library:** The generic MCH library from 058 covers GGBE/BCMC screens. Factor Recon requires different MCH screens (BGMM, BGIR, BSEC, PYDN, PYUP, PYDA, BPOP). A **new GIO.FR.MCH.Library** must be built. The MCH Login/Logoff pattern from 058 is referenced to accelerate development (estimated 10h saving absorbed into 80h total, vs 90h from scratch).

---

## 4. Effort Summary

### 4.1 With Reuse vs. Without Reuse

| Component | Without Reuse (hrs) | 029 Effort With Reuse (hrs) | Hours Saved |
|---|---|---|---|
| SSC.Platform.Logging | 20h | 0h | 20h |
| SSC.Platform.LoginAgent | 16h | 0h | 16h |
| SSC.Custom.Environment | 8h | 0h | 8h |
| BOAT.COB.Excel.Utilities | 24h | 0h | 24h |
| REFramework Template | 24h | 0h | 24h |
| Config.xlsx Template | 8h | 0h | 8h |
| Common Workflows (8) | 32h | 0h | 32h |
| WorkQueue Wrappers (9) | 28h | 0h | 28h |
| **Subtotal — Reused** | **160h** | **0h** | **160h** |
| GIO.FR.MCH.Library (new) | – | 80h | – |
| GIO.FR.Accounting_WS.Library (new) | – | 70h | – |
| GIO.FR.DataTransformation.Library (new) | – | 40h | – |
| GIO.FR.Database.Library (new) | – | 12h | – |
| GIO.FR.MasterScheduler.Library (new) | – | 35h | – |
| GIO.FR.Reporting.Library (new) | – | 25h | – |
| GIO.FR.Posting.Library (new) | – | 120h | – |
| Process Workflows (all processes) | – | 80h | – |
| Config/Assets customization | – | 8h | – |
| Testing & Validation | – | 60h | – |
| UAT Support + Deployment | – | 36h | – |
| **Subtotal — New Build** | – | **672h** | – |
| **GRAND TOTAL** | **840h** | **672h** | **168h** |

### 4.2 Savings at a Glance

| Metric | Without Reuse | With Reuse | Saved |
|---|---|---|---|
| Total Hours | 840h | 672h | 168h (20%) |
| Sprints (team capacity 168h deliverable/sprint) | 5 sprints | 4 sprints | ~1 sprint |
| Duration | ~10 weeks | ~8 weeks | ~2 weeks |
| Developer Cost (@ $X/hr) | 840 × $X | 672 × $X | **168 × $X** |

---

## 5. New Reusable Components Built in 029

> These components are built for the first time in project 029. They will save effort in future GIO Factor Recon-related projects.

### 5.1 Pioneer Value Table

| Component | 029 Build Effort | Future Project Savings (per project) | Projected Future Uses |
|---|---|---|---|
| GIO.FR.MCH.Library | 80h | 80h per project | Any future GIO FR/MCH automation |
| GIO.FR.Accounting_WS.Library | 70h | 60h per project (minor adaptation) | Any project using Bloomberg or RDR |
| GIO.FR.DataTransformation.Library | 40h | 35h per project | GIO factor/pricing projects |
| GIO.FR.Database.Library | 12h | 12h per project | Any SQL-integrated GIO project |
| GIO.FR.MasterScheduler.Library | 35h | 30h per project | Any COB scheduler-driven project |
| GIO.FR.Reporting.Library | 25h | 20h per project | Any Excel reporting GIO project |
| GIO.FR.Posting.Library | 120h | 100h per project | Any MCH posting project |

**Total investment in new reusable components: 382h**  
**Projected savings for next 3 uses of each library: ~1,127h over 3 projects**  
**ROI: 1 investment delivers 3x return within 3 subsequent projects**

---

## 6. Assumptions for Reusable Components

| # | Assumption |
|---|---|
| AS-01 | SSC.Platform.Logging, LoginAgent, Environment, and Excel.Utilities NuGet packages are available on the internal NuGet feed from 058 publication |
| AS-02 | REFramework and Common workflows from 057 require no code changes for use in 029 |
| AS-03 | WorkQueue wrapper queue names are driven by Config.xlsx (no hardcoded names in workflow files) |
| AS-04 | GIO.FR.MCH.Library will share login/logoff pattern with BOAT.COB.MCH.Library from 058 for code reference, but is published as a separate library |
| AS-05 | All new libraries built in 029 will be published to the internal NuGet feed for downstream project reuse |
| AS-06 | Config.xlsx customization (updating asset names, queue names) is charged as 1h to the project setup story, not as library build effort |

---

## 7. Recommendations

1. **Publish all 029 libraries to NuGet.** After 029 completes, publish GIO.FR.MCH.Library, GIO.FR.Accounting_WS.Library, GIO.FR.DataTransformation.Library, GIO.FR.Database.Library, GIO.FR.MasterScheduler.Library, GIO.FR.Reporting.Library, and GIO.FR.Posting.Library to the SSC internal NuGet feed with semantic versioning (1.0.0).

2. **Version control all reusable libraries in a dedicated repository.** Create a `GIO.FR.Libraries` Git repository separate from the process repository for independent versioning and reuse.

3. **Document library dependencies clearly.** Each library's README should specify: system dependencies (MCH terminal version, Bloomberg API version, SQL schema version), configuration prerequisites (asset names), and known breaking changes.

4. **Share GIO.FR.Reporting.Library with other COB projects.** The Excel worksheet creation and distribution pattern is generic enough for COB-wide use.

5. **Plan GIO.FR.Database.Library expansion.** Current scope covers basic CRUD. Future projects may need stored procedure invocation, transaction management, and connection pooling — design extension points now.

6. **Consider GIO.FR.MasterScheduler.Library for COB Platform Library.** The holiday calendar validation pattern (APAC/EMEA/USIS/IIS) is common across GIO COB automations. Elevate this to platform-level reuse after 029 validates it.
