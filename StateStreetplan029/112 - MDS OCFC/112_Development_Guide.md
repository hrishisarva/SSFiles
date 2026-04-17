# 112 MDS OCFC — Development Guide

| Field | Value |
|---|---|
| Document ID | 112-DEV-001 |
| Project | MDS.112 – COB MDS |
| Source | 112_Plan.md + 112_Technical_Design_Document.md |
| Date | 2026-04-13 |
| Status | Draft v1.0 |

---

## Step 0: Project Setup ♻️ Import from 058 — 0 effort

**Tag:** 🤖+👷 BOTH

**Purpose:** Clone REFramework, folder structure, and Config.xlsx from 058 – Market Profiling.

**Agent Prompt (copy-paste into AutoPilot/Claude Ops):**
```
Create a UiPath REFramework project named "MDS.112.OCFC" with the standard folder structure:
- Framework/ (InitAllSettings, InitAllApplications, GetTransactionData, Process, SetTransactionStatus, CloseAllApplications, KillAllProcesses, RetryCurrentTransaction)
- Data/Config.xlsx (3 sheets: Settings, Constants, Assets)
- Common/ (GetUserName, GetCredential, GetStartUpUTCDatetime, WriteProcessLog, WriteTransactionLog, KillExcelProcess)
- Processes/ (subfolders: Orchestrator, Sentinel, FactorGeneric, SecurityIntegrity, CouponDate, Specialized, Support)
- Reusable/ (subfolders: MCH, RKS, HTTP, WebObjects, ESP, BusinessLogic, Infrastructure, Utilities, Logging, LoginAgent, Environment)
- WorkQueue/ (TagItem, UntagItem, DeferItem, CompleteItem, ExceptionItem, AddToQueue)
- Exceptions/ (ScreenshotOnException)
Settings sheet rows: OrchestratorURL, ProcessName=MDS_OCFC, LogLevel=Info, MaxRetryNumber=3
Constants sheet rows: MaxConsecutiveSystemExceptions=3, QueueFolder=MDS-OCFC-112, ScreenshotOnException=True
```

**Developer Actions:**
1. Open UiPath Studio → Create new project from REFramework template
2. Copy folder structure from 058 project
3. Import Config.xlsx and update Settings/Constants/Assets sheets
4. Verify project compiles without errors

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | Project opens in UiPath Studio without errors | Clean build | |
| 2 | Config.xlsx Settings sheet loads correctly | All keys present | |
| 3 | REFramework Main.xaml state machine structure intact | Init → GetTransaction → Process → End | |

---

## Step 1: Import Reusable Libraries ♻️ 0 effort

**Tag:** 🤖+👷 BOTH

**Purpose:** Import Logging, LoginAgent, Environment, Common Workflows, WorkQueue Wrappers, and MCH Core from 058.

**Agent Prompt:**
```
Generate the import manifest for MDS.112.OCFC reusable libraries:
1. SSC.Platform.Logging: WriteProcessLogs.xaml, WriteTransactionLogs.xaml, InitializeLogger.xaml
2. SSC.Platform.LoginAgent: GetSubIDPassword.xaml, ValidateCredential.xaml
3. SSC.Custom.Environment: GetDownloadsDefaultPath.xaml, GetMachineName.xaml
4. Common: GetUserName.xaml, GetCredential.xaml, GetStartUpUTCDatetime.xaml, WriteProcessLog.xaml, WriteTransactionLog.xaml, KillExcelProcess.xaml
5. WorkQueue: TagItem.xaml, UntagItem.xaml, DeferItem.xaml, CompleteItem.xaml, ExceptionItem.xaml, AddToQueue.xaml
6. MCH Core (7 WF): MCH_Launch, MCH_Login, MCH_CheckIfLoggedIn, MCH_NavigateToTransaction, MCH_CloseApplication, MCH_CleanUp, MCH_RetrievePassword
For each file, list: source path in 058, destination path in 112, arguments to verify.
```

**Developer Actions:**
1. Copy each .xaml file from 058 project to 112 project at correct path
2. Update any project-specific references in Config.xlsx (e.g., queue folder name)
3. Verify MCH_Login uses CyberArk_MCH_Credential asset name
4. Compile project — resolve any namespace/dependency issues
5. Run quick smoke test: InitAllSettings → verify Config loads

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | WriteProcessLog.xaml invokes without error | Log entry written | |
| 2 | GetCredential.xaml retrieves CyberArk credential | Returns non-null credential | |
| 3 | MCH_Launch.xaml opens terminal session | Terminal emulator window appears | |
| 4 | MCH_Login.xaml authenticates via RACF | Login successful, session active | |
| 5 | AddToQueue.xaml adds test item to MDS_OCFC_ConvertibleBonds queue | Queue item visible in Orchestrator | |
| 6 | DeferItem.xaml defers a queue item | Item status = Deferred in Orchestrator | |

---

## Step 2: Orchestrator Setup

**Tag:** 👷 DEVELOPER + LEAD

**Purpose:** Create Orchestrator DEV environment — folder, 26 queues, initial assets, robot assignment.

**Developer Actions:**
1. Create Orchestrator folder: `MDS-OCFC-112`
2. Create all 26 queues with exact names and key fields from TDD Section 4
3. Set ECF Checks max retry = 3, Mails max retry = 10, all others = 1
4. Create initial Orchestrator assets for critical config (RootDirectory, TestingFlag, AutomationEndTime)
5. Assign Unattended Robot to folder
6. Verify robot can connect and fetch assets

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | All 26 queues visible in Orchestrator | Queue list matches TDD Section 4 | |
| 2 | Robot can read MDS_OCFC_TestingFlag asset | Returns True/False | |
| 3 | Robot can add item to any queue | Item appears in queue | |

---

## Step 3: DB Infrastructure (DB_Interactions + DB_Locking)

**Tag:** 🤖+👷 BOTH

**Purpose:** Build database interaction and locking workflows — foundation for all data-driven processes.

**Agent Prompt:**
```
Generate UiPath XAML scaffold for DB_Interactions.xaml with these 12 actions:
1. ExecuteQuery — in_Query(String), in_Config(Dictionary), out_DataTable(DataTable)
   - Use Database Connect + Execute Query activities
   - Wrap in Try/Catch with retry logic (in_RetryAttempts from config, in_RetrySleep from config)
2. ExecuteNonQuery — in_Query(String), in_Config, out_AffectedRows(Int32)
3. ExecuteScalar — in_Query(String), in_Config, out_Result(Object)
4. GetItemDetails — in_ItemKey(String), in_DBTable(String), in_Config, out_ItemRow(DataRow)
5. UpdateItemStatus — in_ItemKey, in_Status, in_DBTable, in_Config
6. InsertItem — in_DataRow, in_DBTable, in_Config
7. DeleteItem — in_ItemKey, in_DBTable, in_Config
8. GetPreviousDayItems — in_DBTable, in_Config, out_PrevDayTable(DataTable)
9. GetExceptionSecurities — in_Config, out_ExceptionList(DataTable)
10. CreateDBSnapshot — in_Config
11. CheckDBConnection — in_Config, out_IsConnected(Boolean)
12. ResetIndex — in_DBTable, in_Config

Each action must:
- Have Try/Catch with configurable retry (MDS_OCFC_DBInteractionsRetryAttemptAmount, MDS_OCFC_DBInteractionsRetrySleep)
- Log operation via WriteProcessLog
- Use parameterized queries to prevent SQL injection
- Not expose connection strings in logs

Also generate DB_Locking.xaml with 6 actions:
1. AcquireLock — in_LockName, in_Timeout, in_Config, out_LockAcquired(Boolean)
2. ReleaseLock — in_LockName, in_Config
3. CheckLock — in_LockName, in_Config, out_IsLocked(Boolean)
4. AcquireFactorLock — uses MDS_OCFC_FACTOR LOCK TIMEOUT
5. AcquireExceptionSecurityLock — uses MDS_OCFC_EXCEPTION SECURITY LOCK TIMEOUT
6. ReleaseAllLocks — in_Config
```

**Developer Actions:**
1. Import Agent-generated XAML scaffolds into Studio
2. Configure actual DB connection string (from Config.xlsx Assets sheet)
3. Wire up retry loop using Do While + Try/Catch
4. Test with DEV database connection
5. Verify parameterized queries work for SELECT, INSERT, UPDATE, DELETE

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | ExecuteQuery with valid SELECT | Returns DataTable with rows | |
| 2 | ExecuteQuery with invalid SQL | Caught by Try/Catch, retries N times, then SystemException | |
| 3 | AcquireLock + ReleaseLock | Lock acquired, then released — no deadlock | |
| 4 | AcquireLock when already locked | Waits up to timeout, returns False if timeout exceeded | |
| 5 | Connection string not in logs | Grep log output for connection string — not found | |

---

## Step 4: MCH 112-Specific Screens (9 NEW Workflows)

**Tag:** 🤖+👷 BOTH

**Purpose:** Build 9 MCH screen-specific workflows for BFSE, BSEC, and BGOV mainframe screens.

**Agent Prompt:**
```
Generate UiPath XAML scaffolds for MCH 112-specific screen workflows. Each workflow must:
- Accept in_SecurityID (String), in_Config (Dictionary)
- Return out_ScreenData (DataTable) or similar
- Call MCH_NavigateToTransaction to reach the screen
- Use Terminal Get Text/Set Text activities for field interaction
- Include Try/Catch with session recovery (MCH_CleanUp → MCH_Login → retry once)
- Log entry/exit via WriteProcessLog

Workflows:
1. MCH_CheckTransactionAccess.xaml — Navigate to transaction, check if access granted, return Boolean
2. MCH_BFSE_DataRetrieval.xaml — Navigate to BFSE, enter security ID, read all fields into DataTable
3. MCH_BSEC_DataRetrieval.xaml — Navigate to BSEC, enter security ID, read security detail fields
4. MCH_BSEC_MultiLine.xaml — Navigate to BSEC, handle multi-line data retrieval (page down loop)
5. MCH_BSEC_IDF_GetData.xaml — Navigate to BSEC IDF screen, read internal data feed fields
6. MCH_BSEC_IDF_MainScreen.xaml — Read BSEC IDF main screen fields
7. MCH_BFSE_IDF_GetData.xaml — Navigate to BFSE IDF, read fields (pattern from MCH_BFSE)
8. MCH_BFSE_IDF_Generic.xaml — Generic BFSE IDF retrieval for configurable field list
9. MCH_BGOV_IDF_GetData.xaml — Navigate to BGOV IDF, read government securities fields
```

**Developer Actions:**
1. Import Agent-generated XAML scaffolds
2. Capture terminal selectors for each MCH screen (row/column positions for BFSE, BSEC, BGOV)
3. Map field positions from Blue Prism terminal navigation to UiPath Terminal Activities
4. Test MCH_BFSE_DataRetrieval with a known security ID — verify all fields retrieved
5. Test MCH_BSEC_DataRetrieval — verify single-line and multi-line variants
6. Test MCH_BGOV_IDF_GetData with government security
7. Verify session recovery: kill terminal mid-operation → check auto-reconnect

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | MCH_BFSE_DataRetrieval with valid security ID | DataTable with correct field values | |
| 2 | MCH_BFSE_DataRetrieval with invalid security ID | BusinessRuleException — "Security not found" | |
| 3 | MCH_BSEC_MultiLine with multi-page result | All pages retrieved, complete DataTable | |
| 4 | MCH_BGOV_IDF_GetData with government security | BGOV-specific fields populated | |
| 5 | Session recovery after terminal disconnect | Auto-reconnect and retry succeeds | |
| 6 | MCH_CheckTransactionAccess with unauthorized transaction | Returns False, no crash | |

---

## Step 5: HTTP/API Libraries (9 Workflows)

**Tag:** 🤖+👷 BOTH

**Purpose:** Build HTTP/API interaction workflows for MDP, ECF, OCF, SSCD, RMG, ERP, RDOC, IMS Hub.

**Agent Prompt:**
```
Generate UiPath XAML scaffolds for HTTP API workflows. Common pattern per API:
1. Retrieve credentials from CyberArk via GetCredential
2. HTTP POST to login URL with configured payload → get session token
3. HTTP GET/POST to API endpoint with session headers
4. Deserialize JSON/XML response
5. Handle HTTP errors (401 → re-login, 500 → retry, timeout → retry)
6. Return structured data

Generate for each:
- MDP_HTTP_DataRetriever.xaml — 22 actions. Support ForgeRock auth (MDS_OCFC_MDP FORGEROCK flag). Login modes: standard (adcorp), ForgeRock, SecurID fallback. Actions: Login, GetSecurityData, GetFactorData, GetPaydownReport, QueryByField, etc.
- MDP_HTTP_Extended.xaml — 12 actions. Extended queries beyond standard retriever.
- ECF_HTTP.xaml — 19 actions. ECF API login, data queries, status updates, exception detail retrieval.
- OCF_HTTP.xaml — 17 actions. OCF data source queries.
- SSCD_HTTP.xaml — 15 actions. Securities classification data.
- RMG_HTTP.xaml — 14 actions. Template upload, approve processing, report retrieval.
- ERP_HTTP.xaml — 6 actions. ERP API login with optional 2FA.
- RDOC_HTTP.xaml — 6 actions. Document retrieval.
- IMS_HUB.xaml — 10 actions. SOAP web service for IMS Hub.

Each must use parameterized URLs from Config assets, never hardcode URLs.
```

**Developer Actions:**
1. Import scaffolds into project
2. Configure actual API URLs and payloads from BP environment variable values
3. Test MDP login first — validate ForgeRock auth flow
4. Test each API endpoint with a known security ID
5. Verify error handling: simulate 401, 500, timeout
6. Verify no credentials or tokens logged

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | MDP_HTTP login (standard) | Session token returned | |
| 2 | MDP_HTTP login (ForgeRock) | ForgeRock auth complete, token returned | |
| 3 | ECF_HTTP GetExceptionDetails | Returns exception detail JSON | |
| 4 | RMG_HTTP TemplateUpload | File uploaded successfully | |
| 5 | IMS_HUB SOAP call | Valid SOAP response | |
| 6 | HTTP 401 → auto re-login | Retry succeeds after re-authentication | |
| 7 | HTTP 500 → retry 2x then fail | SystemException after 2 retries | |
| 8 | No credentials in logs | Grep logs for password/token — not found | |

---

## Step 6: RKS Library (35 Workflows)

**Tag:** 🤖+👷 BOTH

**Purpose:** Build RKS automation library — launch (3 modes), core operations, and 29 screen-specific workflows.

**Agent Prompt:**
```
Generate UiPath XAML scaffold for RKS library:

Launch workflows (3):
1. RKS_LaunchHTTP.xaml — HTTP-based launch: POST to RKS launch URL, wait for RKS window
2. RKS_LaunchEdge.xaml — Edge browser launch: navigate to RKS URL, handle DaaS login, wait for application
3. RKS_LaunchUIA.xaml — UIA mode launch: start Citrix application, attach to RKS window

Core workflows (4):
4. RKS_MainObject.xaml — 24 actions: navigation, window management, data retrieval, status checking
5. RKS_Wrapper.xaml — 46 actions: high-level operations orchestrating sub-objects
6. RKS_IssueSearch.xaml — 9 actions: search by security ID, ISIN, SEDOL
7. RKS_UpdateLogic.xaml — 15 actions: field updates, save, confirm

Screen workflows (28) — generate scaffold for each:
[RKS_CrossReference(21), RKS_FactorHistory(17), RKS_FixedIncome(15), RKS_FixedIncomeMaturityDate(11), 
RKS_FloatingInterest(17), RKS_FuturesLastTradingDate(11), RKS_OptionsExpirationDate(11), 
RKS_RightsExpirationDate(10), RKS_WarrantsExpirationDate(11), RKS_CashFlowSchedule(9), 
RKS_CashFlowScheduleData(8), RKS_ScheduledPrincipalPayments(24), RKS_ScheduledPrincipalPaymentsEntry(11),
RKS_StepTable(7), RKS_AdjustableTable(7), RKS_BaseRateName(11), RKS_CreditAdjustments(6),
RKS_CreditInfoStatusWindow(5), RKS_DetailAnalysis(7), RKS_MarketData(5), RKS_PortfolioBrowser(9),
RKS_Schedules(6), RKS_SecurityPaydown(5), RKS_TransactionHistory(12), RKS_ImportExternalData(10),
RKS_AddNewFactorLine(13), RKS_AdditionalFixedIncomeInfo(5), RKS_AdditionalIssueTypeInfo(10)]

Each screen workflow must: accept in_RKSSession, in_SecurityID, in_Config; return data in DataTable; include retry on session timeout.
```

**Developer Actions:**
1. Import scaffolds — start with RKS_LaunchHTTP (simplest launch mode)
2. Capture RKS selectors in UiPath Explorer — window title, menu items, table cells
3. Build Launch workflows first — verify all 3 launch modes work
4. Build IssueSearch — verify search by security ID works
5. Build screen workflows in batches (Fixed Income group → End Date group → Schedule group → Data group)
6. Test RKS_Wrapper with a complete lifecycle (launch → search → navigate to screen → read data → close)

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | RKS_LaunchHTTP | RKS window opens via HTTP | |
| 2 | RKS_LaunchEdge | RKS opens in Edge, DaaS login succeeds | |
| 3 | RKS_LaunchUIA | RKS opens via Citrix UIA | |
| 4 | RKS_IssueSearch with valid ID | Security found, details shown | |
| 5 | RKS_CrossReference data retrieval | Cross-reference table returned | |
| 6 | RKS_FixedIncome screen read | Fixed income fields populated | |
| 7 | RKS_UpdateLogic field update | Field updated, save confirmed | |
| 8 | RKS session timeout recovery | Auto-relaunch and retry | |
| 9 | Fallback: HTTP fail → Edge → UIA | Falls back through 3 modes | |

---

## Step 7: ESP Library (3 Workflows)

**Tag:** 🤖+👷 BOTH

**Purpose:** Build ESP database/HTTP integration.

**Agent Prompt:**
```
Generate UiPath XAML scaffolds for ESP library:
1. ESP_Main.xaml — 20 actions:
   - Connect to ESP via ODBC (connection string from config)
   - ExecuteQuery patterns for: cross-reference, factor tables, transaction data, position data
   - Each query uses parameterized SQL
   - Handle connection errors with retry

2. ESP_Extended.xaml — 41 actions:
   - Extended queries: GetCrossReference, GetFactorTable (MVP1/MVP3 clients), GetTransactions (MVP3 clients)
   - Accrual data retrieval, credit status checkboxes, dated date queries
   - Log file creation for RKS-vs-ESP comparisons (configurable via check code flags)

3. ESP_MDPValidation.xaml — 5 actions:
   - Cross-validate ESP data against MDP data
   - Compare factor tables, identify mismatches
   - Return validation results DataTable
```

**Developer Actions:**
1. Import scaffolds
2. Configure ESP connection string from Config asset
3. Test ODBC connectivity to ESP DEV environment
4. Validate each query returns expected data shape
5. Test ESP_Extended with MVP1 and MVP3 client variations

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | ESP ODBC connection | Connects successfully | |
| 2 | GetCrossReference for known security | DataTable with cross-ref data | |
| 3 | GetFactorTable for MVP1 client | Factor table returned | |
| 4 | ESP_MDPValidation with matching data | No mismatches reported | |
| 5 | ESP_MDPValidation with mismatch | Mismatch details in result DataTable | |

---

## Step 8: Web Object Libraries (4 Workflows)

**Tag:** 🤖+👷 BOTH

**Purpose:** Build browser automation for ERP, GTM, PSDL, ECF Data Upload.

**Agent Prompt:**
```
Generate UiPath XAML scaffolds for web object workflows:
1. ERP_WebObject.xaml — 23 actions: Navigate to ERP URL, login with CyberArk (robmfa), handle 2FA (edr domain if MDS_OCFC_ERP_2FA_FLAG), perform 23 ERP operations (data queries, form fills, navigation)
2. GTM_WebObject.xaml — 20 actions: Navigate to GTM URL, login with CyberArk, handle 2FA (edr domain if MDS_OCFC_GTM_2FA_FLAG), CRN sentinel operations
3. PSDL_WebObject.xaml — 23 actions: Navigate to PSDL URL, login with CyberArk (alias), perform data queries
4. ECF_DataUpload.xaml — 10 actions: Login to ECF upload portal (UIA login URL), upload file, check upload status, verify upload message

Each must: use CSS selectors where possible, handle iframe navigation, detect 2FA modals, retry on timeout.
```

**Developer Actions:**
1. Import scaffolds
2. Open each web application in browser — capture selectors using UiPath Explorer
3. Implement 2FA flow for ERP and GTM (detect 2FA modal → GetCredential for 2FA domain → enter token)
4. Test complete login + data retrieval cycle for each
5. Test ECF file upload with sample file

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | ERP login without 2FA | Successful login | |
| 2 | ERP login with 2FA enabled | 2FA modal handled, login completes | |
| 3 | GTM 2FA login | Token entered, access granted | |
| 4 | PSDL data query | Data returned correctly | |
| 5 | ECF file upload | File uploaded, status = success | |

---

## Step 9: Business Logic Libraries (32 Workflows)

**Tag:** 🤖+👷 BOTH

**Purpose:** Build all business logic workflows (3019/3334, 3077, 3122, 3917, MVP2/4, CPN_TYP, etc.)

**Agent Prompt:**
```
Generate UiPath XAML scaffolds for business logic workflows. Each receives: in_ItemData(DataRow), in_ESPData(DataTable), in_MDPData(DataTable), in_RKSData(DataTable), in_Config(Dictionary), and returns out_Result(String), out_Action(String).

3019/3334 Check Group (16 WF):
1. Logic_3019_3334_Wrapper — Orchestrates all 3019/3334 scenarios based on security type
2. Logic_3019_3334_SpecificActions — Common actions for 3019/3334
3-16. [One per scenario: AdjCorpGovt, AdjMuni, CorpGovtAdjComparison, CpnTypAdjMtge, CpnTypFixedMtge, CpnTypMultiple, FixedToFloat, FloatingVariable, FloatingComparison, MtgeAdjComparison, MtgeScenario, MuniAdjComparison, NotFixedToFloat, StepScenario]

Each scenario workflow: evaluate security attributes → match against scenario criteria → return action (Update/Notify/Skip)

3077 Group: Logic_3077_RKSUpdate — Determine if RKS update needed based on data comparison
3122 Group (4 WF): Logic_3122_MDP, Logic_3122_TargetDate, Logic_3122_TransactionLevel, Logic_3122_TransactionLevelNEW
3917 Group: Logic_3917_MDP
Other (10 WF): Logic_CPN_TYP, Logic_MDP, Logic_MVP2_Generic, Logic_MVP4_3016_Wrapper, Logic_MVP4_3329_Wrapper, Logic_PrincipalType, Logic_FindUnderlyingSecurity, Logic_GetFactorTable, Logic_Rounding, Logic_DummySecurity, Logic_MasterFileValidation
```

**Developer Actions:**
1. Import scaffolds
2. Review BP business rules for each check code — map decision logic to VB.NET
3. Implement each scenario's conditional branching
4. Test with known test cases (security type → expected action)
5. Build comprehensive test matrix for 3019/3334 scenarios (CORP/GOVT, MUNI, MTGE, Float, Step, etc.)

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | 3019/3334 CORP/GOVT Adjustable scenario | Correct comparison result | |
| 2 | 3019/3334 MUNI Adjustable scenario | Correct comparison result | |
| 3 | 3019/3334 MTGE Fixed scenario | Correct coupon type action | |
| 4 | 3019/3334 Float-to-Variable scenario | Correct transition detection | |
| 5 | 3019/3334 STEP scenario | Step coupon logic applied | |
| 6 | 3077 RKS Update needed | Returns "Update" action | |
| 7 | 3077 RKS no update | Returns "Skip" action | |
| 8 | 3122 MDP logic with mismatch | Returns discrepancy details | |
| 9 | 3917 MDP logic | Correct factor comparison | |
| 10 | Rounding logic — 6 decimal places | Correct rounding per BGOV config | |

---

## Step 10: Sentinel & Orchestrator Process Wiring

**Tag:** 🤖+👷 BOTH

**Purpose:** Wire Sentinel (35 subsheets) and Orchestrator (4 subsheets) processes.

**Agent Prompt:**
```
Generate UiPath XAML scaffolds for:

SentinelMain.xaml (REFramework non-queue performer):
- InitAllSettings → Load config, validate prerequisites
- Process: 
  1. CheckESPAccess — invoke ESP_Main.CheckConnection → if fail, throw BusinessRuleException
  2. CheckClientConfig — load client config from DB, validate sentinel flags (MDS_OCFC_SENTINEL CHECK CLIENT, CHECK ESP ACCESS)
  3. For each enabled check: monitor process status via Orchestrator API or DB status table
  4. GenerateSentinelReport — aggregate results, export to MDS_OCFC_SENTINEL REPORT TEMP FOLDER
  5. Send collective notifications based on MDS_OCFC_NOTIFICATION TO SEND config
- SetTransactionStatus: log completion, handle exceptions

OrchestratorMain.xaml (Controller):
- Read MDS_OCFC_DYNAMIC SCHEDULER to determine which checks run today
- For each check: invoke MainProcessTriggering → start Orchestrator job or invoke workflow
- Monitor via MDS_OCFC_MAX TIME WORKER PROCESSING timeout
- On completion: trigger StatusReportProcess
```

**Developer Actions:**
1. Import Sentinel scaffold into Studio
2. Wire ESP access check to actual ESP connection
3. Wire client config loading to DB_Interactions
4. Wire notification sending to MailWrapper + CollectiveNotification
5. Test Sentinel with ESP accessible → verify clean run
6. Test Sentinel with ESP down → verify BusinessRuleException
7. Wire Orchestrator to trigger check processes via Orchestrator API

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | Sentinel with all systems up | Clean completion, report generated | |
| 2 | Sentinel with ESP down | BusinessRuleException, escalation email sent | |
| 3 | Orchestrator triggers Factor Generic process | Process starts in Orchestrator | |
| 4 | Orchestrator respects dynamic schedule | Only scheduled checks triggered | |
| 5 | Automation end time exceeded | Graceful shutdown, status report | |

---

## Step 11: Process Integration — Factor Generic (Dispatcher/Performer)

**Tag:** 🤖+👷 BOTH

**Purpose:** Wire the Factor Generic Process as Dispatcher + Performer handling 5 check types.

**Agent Prompt:**
```
Generate UiPath XAML scaffolds for Factor Generic:

FactorGenericDispatcher.xaml:
- Read check type from Config (NullFactor, StaleFactor, FactorVariance, DuplicateFactor, FactorRateConsistency)
- Query DB for items matching check type → DB_Interactions.ExecuteQuery
- For each row: AddToQueue with appropriate queue name
- Handle deferred items: check defer time config, set PostponeDate on queue item

FactorGenericPerformer.xaml (REFramework):
- GetTransactionData: Get next queue item from appropriate queue
- Process:
  1. Parse item data (SecurityID, CheckCode, ClientCode)
  2. Acquire DB lock if needed (DB_Locking.AcquireFactorLock)
  3. Retrieve ESP data via ESP_Extended
  4. Retrieve MDP data via MDP_HTTP_DataRetriever
  5. Retrieve RKS data if needed via RKS_Wrapper
  6. Route to check-specific logic based on CheckCode:
     - 3006 → Logic_MVP2_Generic
     - 3077 → Logic_3077_RKSUpdate
     - 3122 → Logic_3122_MDP / Logic_3122_TargetDate / Logic_3122_TransactionLevel
     - 3334/3019 → Logic_3019_3334_Wrapper
     - 3917 → Logic_3917_MDP
  7. Execute result action (RKS update, notification, skip)
  8. Release DB lock
- SetTransactionStatus: Complete / BusinessException / SystemException
```

**Developer Actions:**
1. Import scaffolds
2. Wire Dispatcher → verify items loaded from DB and added to correct queue
3. Wire Performer → verify queue item processing lifecycle
4. Test with Null Factor check type end-to-end
5. Test with Factor Rate Consistency (includes deferred processing)
6. Verify DB locking works across concurrent performers

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | Dispatcher loads Null Factor items | Items in MDS_OCFC_NullFactor queue | |
| 2 | Performer processes Null Factor item | Item marked Complete/BusinessException | |
| 3 | Factor Rate Consistency with deferred item | Item deferred, reprocessed after timeout | |
| 4 | Two concurrent performers — DB lock | Lock prevents conflict, both complete | |
| 5 | Business exception — data mismatch | Item marked BusinessException, notification sent | |

---

## Step 12: Remaining Process Wiring

**Tag:** 🤖+👷 BOTH

**Purpose:** Wire all remaining processes (Security Integrity, Coupon/Date, Specialized).

**Developer Actions:**
1. Wire 7 Security Integrity processes (each uses same Dispatcher/Performer pattern from Step 11)
2. Wire 7 Coupon/Date processes
3. Wire 10 Specialized processes (ECF with 3-retry queue, Inflation Index, ConvertibleBonds, etc.)
4. Wire Status Report process
5. Integration test: Orchestrator → Sentinel → Factor Generic → Security Integrity → Coupon/Date → Specialized → Status Report

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | DupeISIN process end-to-end | Duplicate ISINs detected correctly | |
| 2 | ECF Checks with retry | After ECF failure, retries 3 times | |
| 3 | Inflation Index processing | Inflation index mapped correctly | |
| 4 | Validate USER1 end-to-end | USER1 field validated across systems | |
| 5 | Status Report generated | Report file created with correct data | |
| 6 | Full orchestrator cycle | All 33 processes complete, status report generated | |

---

## Step 13: Mail & Notification Integration

**Tag:** 🤖+👷 BOTH

**Purpose:** Wire email notification workflows.

**Agent Prompt:**
```
Generate XAML scaffolds for:
1. MailWrapper.xaml — 11 actions: compose email, add attachments, send via Outlook, retry logic (max 10 from Mails queue config)
2. CollectiveNotification.xaml — 6 actions: group notifications by type, aggregate into single email, send via MailWrapper
3. Wire MDS_OCFC_BU GENERAL MAIL, MDS_OCFC_DATA_MANAGEMENT_MAIL, MDS_OCFC_RPA SUPPORT MAIL as recipient assets
```

**Developer Actions:**
1. Import scaffolds
2. Configure Outlook integration (MAPI connection)
3. Test email sending with DEV distribution lists
4. Wire notification triggers in Sentinel and check processes

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | Send single notification email | Email delivered with correct subject/body | |
| 2 | Collective notification (5 items grouped) | Single email with 5 items listed | |
| 3 | Email send failure + retry | Retries up to 10 times | |

---

## Step 14: End-to-End Testing & UAT

**Tag:** 👷 DEVELOPER + LEAD

**Purpose:** Comprehensive integration testing and business validation.

**Developer Actions:**
1. Run full Orchestrator cycle with sample production data
2. Compare UiPath output against Blue Prism output for same inputs
3. Test all 15 exception scenarios from Exception Handling Matrix
4. Test deferred processing for all 4 deferred queue types
5. Verify all 12 CyberArk credential integrations
6. Test RKS in all 3 launch modes
7. Generate test evidence artifacts (screenshots, logs, result comparisons)

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| 1 | Full cycle — factor validation results match BP | 100% match | |
| 2 | Full cycle — notification emails match BP | Same recipients, same content | |
| 3 | Full cycle — status report matches BP format | Report format and data match | |
| 4 | ESP failure during processing | Sentinel blocks processes, escalation sent | |
| 5 | MCH session recovery | Auto-reconnect, retry succeeds | |
| 6 | RKS fallback (HTTP fail → Edge → UIA) | Fallback works, no data loss | |
| 7 | All 26 queues process to completion | 0 items left in any queue | |
