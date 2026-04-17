# 112 MDS OCFC — Technical Design Document

| Field | Value |
|---|---|
| Document ID | 112-TDD-001 |
| Project | MDS.112 – COB MDS |
| Source | 112_Business_Requirements_Document.md |
| Date | 2026-04-13 |
| Status | Draft v1.0 |
| UiPath Studio Version | 2023.10.7 (Windows) |
| Orchestrator Version | 2023.10.4 |
| Framework | REFramework (Dispatcher/Performer) |

---

## 1. Solution Architecture

### 1.1 UiPath Project Folder Structure

```
MDS.112.OCFC/
├── Framework/
│   ├── InitAllSettings.xaml
│   ├── InitAllApplications.xaml
│   ├── GetTransactionData.xaml
│   ├── Process.xaml
│   ├── SetTransactionStatus.xaml
│   ├── CloseAllApplications.xaml
│   ├── KillAllProcesses.xaml
│   └── RetryCurrentTransaction.xaml
├── Data/
│   └── Config.xlsx
├── Common/
│   ├── GetUserName.xaml
│   ├── GetCredential.xaml
│   ├── GetStartUpUTCDatetime.xaml
│   ├── WriteProcessLog.xaml
│   ├── WriteTransactionLog.xaml
│   └── KillExcelProcess.xaml
├── Processes/
│   ├── Orchestrator/
│   │   ├── OrchestratorMain.xaml
│   │   └── TriggerCheckProcess.xaml
│   ├── Sentinel/
│   │   ├── SentinelMain.xaml
│   │   ├── CheckESPAccess.xaml
│   │   ├── CheckClientConfig.xaml
│   │   ├── MonitorProcesses.xaml
│   │   ├── GenerateSentinelReport.xaml
│   │   └── SentinelSpecificActions.xaml
│   ├── FactorGeneric/
│   │   ├── FactorGenericDispatcher.xaml
│   │   ├── FactorGenericPerformer.xaml
│   │   ├── NullFactorProcess.xaml
│   │   ├── StaleFactorProcess.xaml
│   │   ├── FactorVarianceProcess.xaml
│   │   ├── DuplicateFactorProcess.xaml
│   │   └── FactorRateConsistencyProcess.xaml
│   ├── SecurityIntegrity/
│   │   ├── DupeISINProcess.xaml
│   │   ├── DupeSedolsProcess.xaml
│   │   ├── DupeClientSecurityProcess.xaml
│   │   ├── NullSecurityCodeProcess.xaml
│   │   ├── SecurityCurrencyCodeProcess.xaml
│   │   ├── SecurityDetailsNullProcess.xaml
│   │   └── ExchangeCodeProcess.xaml
│   ├── CouponDate/
│   │   ├── CouponDateIntegrityProcess.xaml
│   │   ├── CouponRateIntegrityProcess.xaml
│   │   ├── CouponTypeChangeProcess.xaml
│   │   ├── MissingAdjCouponDatedDateProcess.xaml
│   │   ├── EndDateValidationProcess.xaml
│   │   ├── MaturityDateProcess.xaml
│   │   └── MaturedExpiredPositionsProcess.xaml
│   ├── Specialized/
│   │   ├── ECFChecksProcess.xaml
│   │   ├── InflationIndexProcess.xaml
│   │   ├── ConvertibleBondsProcess.xaml
│   │   ├── UnderlierSecurityProcess.xaml
│   │   ├── ValidateUSER1Process.xaml
│   │   ├── MultilistedUSER1Process.xaml
│   │   ├── SearchUSER1Process.xaml
│   │   ├── PrincipalTypeProcess.xaml
│   │   ├── SMFCalculationProcess.xaml
│   │   └── RKSDataMartsValidationProcess.xaml
│   └── Support/
│       └── StatusReportProcess.xaml
├── Reusable/
│   ├── MCH/                          ← Partially from 058
│   │   ├── MCH_Launch.xaml           ♻️ from 058
│   │   ├── MCH_Login.xaml            ♻️ from 058
│   │   ├── MCH_CheckIfLoggedIn.xaml  ♻️ from 058
│   │   ├── MCH_NavigateToTransaction.xaml  ♻️ from 058
│   │   ├── MCH_CloseApplication.xaml ♻️ from 058
│   │   ├── MCH_CleanUp.xaml          ♻️ from 058
│   │   ├── MCH_RetrievePassword.xaml ♻️ from 058
│   │   ├── MCH_CheckTransactionAccess.xaml  NEW
│   │   ├── MCH_BFSE_DataRetrieval.xaml      NEW
│   │   ├── MCH_BSEC_DataRetrieval.xaml      NEW
│   │   ├── MCH_BSEC_MultiLine.xaml          NEW
│   │   ├── MCH_BSEC_IDF_GetData.xaml        NEW
│   │   ├── MCH_BSEC_IDF_MainScreen.xaml     NEW
│   │   ├── MCH_BFSE_IDF_GetData.xaml        NEW
│   │   ├── MCH_BFSE_IDF_Generic.xaml        NEW
│   │   └── MCH_BGOV_IDF_GetData.xaml        NEW
│   ├── RKS/
│   │   ├── RKS_LaunchHTTP.xaml
│   │   ├── RKS_LaunchEdge.xaml
│   │   ├── RKS_LaunchUIA.xaml
│   │   ├── RKS_MainObject.xaml
│   │   ├── RKS_Wrapper.xaml
│   │   ├── RKS_IssueSearch.xaml
│   │   ├── RKS_UpdateLogic.xaml
│   │   ├── RKS_CrossReference.xaml
│   │   ├── RKS_FactorHistory.xaml
│   │   ├── RKS_FixedIncome.xaml
│   │   ├── RKS_FixedIncomeMaturityDate.xaml
│   │   ├── RKS_FloatingInterest.xaml
│   │   ├── RKS_FuturesLastTradingDate.xaml
│   │   ├── RKS_OptionsExpirationDate.xaml
│   │   ├── RKS_RightsExpirationDate.xaml
│   │   ├── RKS_WarrantsExpirationDate.xaml
│   │   ├── RKS_CashFlowSchedule.xaml
│   │   ├── RKS_CashFlowScheduleData.xaml
│   │   ├── RKS_ScheduledPrincipalPayments.xaml
│   │   ├── RKS_ScheduledPrincipalPaymentsEntry.xaml
│   │   ├── RKS_StepTable.xaml
│   │   ├── RKS_AdjustableTable.xaml
│   │   ├── RKS_BaseRateName.xaml
│   │   ├── RKS_CreditAdjustments.xaml
│   │   ├── RKS_CreditInfoStatusWindow.xaml
│   │   ├── RKS_DetailAnalysis.xaml
│   │   ├── RKS_MarketData.xaml
│   │   ├── RKS_PortfolioBrowser.xaml
│   │   ├── RKS_Schedules.xaml
│   │   ├── RKS_SecurityPaydown.xaml
│   │   ├── RKS_TransactionHistory.xaml
│   │   ├── RKS_ImportExternalData.xaml
│   │   ├── RKS_AddNewFactorLine.xaml
│   │   ├── RKS_AdditionalFixedIncomeInfo.xaml
│   │   ├── RKS_AdditionalIssueTypeInfo.xaml
│   │   └── RKS_MDPValidation.xaml
│   ├── HTTP/
│   │   ├── ECF_HTTP.xaml
│   │   ├── ERP_HTTP.xaml
│   │   ├── MDP_HTTP_DataRetriever.xaml
│   │   ├── MDP_HTTP_Extended.xaml
│   │   ├── OCF_HTTP.xaml
│   │   ├── RDOC_HTTP.xaml
│   │   ├── RMG_HTTP.xaml
│   │   ├── SSCD_HTTP.xaml
│   │   └── RKS_LaunchHTTP.xaml
│   ├── WebObjects/
│   │   ├── ERP_WebObject.xaml
│   │   ├── GTM_WebObject.xaml
│   │   ├── PSDL_WebObject.xaml
│   │   ├── ECF_DataUpload.xaml
│   │   └── IMS_HUB.xaml
│   ├── ESP/
│   │   ├── ESP_Main.xaml
│   │   ├── ESP_Extended.xaml
│   │   └── ESP_MDPValidation.xaml
│   ├── BusinessLogic/
│   │   ├── Logic_3019_3334_Wrapper.xaml
│   │   ├── Logic_3019_3334_SpecificActions.xaml
│   │   ├── Logic_3019_3334_FloatingComparison.xaml
│   │   ├── Logic_3019_3334_AdjCorpGovt.xaml
│   │   ├── Logic_3019_3334_AdjMuni.xaml
│   │   ├── Logic_3019_3334_CorpGovtAdjComparison.xaml
│   │   ├── Logic_3019_3334_CpnTypAdjMtge.xaml
│   │   ├── Logic_3019_3334_CpnTypFixedMtge.xaml
│   │   ├── Logic_3019_3334_CpnTypMultiple.xaml
│   │   ├── Logic_3019_3334_FixedToFloat.xaml
│   │   ├── Logic_3019_3334_FloatingVariable.xaml
│   │   ├── Logic_3019_3334_MtgeAdjComparison.xaml
│   │   ├── Logic_3019_3334_MtgeScenario.xaml
│   │   ├── Logic_3019_3334_MuniAdjComparison.xaml
│   │   ├── Logic_3019_3334_NotFixedToFloat.xaml
│   │   ├── Logic_3019_3334_StepScenario.xaml
│   │   ├── Logic_3077_RKSUpdate.xaml
│   │   ├── Logic_3122_MDP.xaml
│   │   ├── Logic_3122_TargetDate.xaml
│   │   ├── Logic_3122_TransactionLevel.xaml
│   │   ├── Logic_3122_TransactionLevelNEW.xaml
│   │   ├── Logic_3917_MDP.xaml
│   │   ├── Logic_CPN_TYP.xaml
│   │   ├── Logic_MDP.xaml
│   │   ├── Logic_MVP2_Generic.xaml
│   │   ├── Logic_MVP4_3016_Wrapper.xaml
│   │   ├── Logic_MVP4_3329_Wrapper.xaml
│   │   ├── Logic_PrincipalType.xaml
│   │   ├── Logic_FindUnderlyingSecurity.xaml
│   │   ├── Logic_GetFactorTable.xaml
│   │   ├── Logic_Rounding.xaml
│   │   ├── Logic_DummySecurity.xaml
│   │   └── Logic_MasterFileValidation.xaml
│   ├── Infrastructure/
│   │   ├── DB_Interactions.xaml
│   │   ├── DB_Locking.xaml
│   │   ├── MailWrapper.xaml
│   │   ├── CollectiveNotification.xaml
│   │   ├── MainProcessTriggering.xaml
│   │   ├── IMT_ItemFinalization.xaml
│   │   ├── ECF_DataUpload.xaml
│   │   ├── WebserviceLog.xaml
│   │   ├── OCF_DataSourceWrapper.xaml
│   │   ├── TBA_RKSModification.xaml
│   │   └── TBA_RKSOrchestrator.xaml
│   ├── Utilities/
│   │   ├── CSharp_Utilities.xaml
│   │   ├── VB_Utilities.xaml
│   │   └── VLOOKUP_Object.xaml
│   ├── Logging/                      ♻️ from 058
│   │   ├── WriteProcessLogs.xaml
│   │   ├── WriteTransactionLogs.xaml
│   │   └── InitializeLogger.xaml
│   ├── LoginAgent/                   ♻️ from 058
│   │   ├── GetSubIDPassword.xaml
│   │   └── ValidateCredential.xaml
│   └── Environment/                  ♻️ from 058
│       ├── GetDownloadsDefaultPath.xaml
│       └── GetMachineName.xaml
├── WorkQueue/                        ♻️ from 058
│   ├── TagItem.xaml
│   ├── UntagItem.xaml
│   ├── DeferItem.xaml
│   ├── CompleteItem.xaml
│   ├── ExceptionItem.xaml
│   └── AddToQueue.xaml
├── Exceptions/
│   └── ScreenshotOnException.xaml
└── Main.xaml
```

### 1.2 Orchestrator Folder Structure

```
Orchestrator/
├── Folder: MDS-OCFC-112/
│   ├── Processes/
│   │   ├── MDS_OCFC_Orchestrator
│   │   ├── MDS_OCFC_Sentinel
│   │   ├── MDS_OCFC_FactorGeneric
│   │   ├── MDS_OCFC_DupeISIN
│   │   ├── MDS_OCFC_DupeSedols
│   │   ├── MDS_OCFC_DupeClientSecurity
│   │   ├── MDS_OCFC_ECFChecks
│   │   ├── MDS_OCFC_CouponDateIntegrity
│   │   ├── MDS_OCFC_InflationIndex
│   │   ├── MDS_OCFC_ValidateUSER1
│   │   ├── MDS_OCFC_StatusReport
│   │   └── ... (33 processes)
│   ├── Queues/ (26 queues)
│   ├── Assets/ (272 + credentials)
│   ├── Triggers/
│   └── Machines/
```

### 1.3 NuGet Dependencies

| Package | Version | Purpose |
|---|---|---|
| UiPath.System.Activities | 23.10.x | Core system activities |
| UiPath.Mail.Activities | 1.21.x | Outlook email integration |
| UiPath.Database.Activities | 1.8.x | SQL database interactions |
| UiPath.WebAPI.Activities | 1.18.x | HTTP REST/SOAP API calls |
| UiPath.Terminal.Activities | 2.4.x | MCH mainframe (TN3270) |
| UiPath.UIAutomation.Activities | 23.10.x | RKS, ERP, GTM browser automation |
| UiPath.Excel.Activities | 2.22.x | Excel operations |
| UiPath.Credentials.Activities | 2.3.x | CyberArk credential retrieval |

---

## 2. Process Design

### 2.1 MDS.112.OCFC.Orchestrator

| Field | Value |
|---|---|
| UiPath Process Name | MDS_OCFC_Orchestrator |
| Trigger | Scheduled (Daily) or Manual |
| REFramework Role | Controller (non-queue) |
| Config Sheet | Settings: OrchestratorProcessName, DynamicScheduler, AutomationEndTime |

**Workflow List:**

| Workflow | Arguments (Direction) | Purpose |
|---|---|---|
| OrchestratorMain.xaml | in_Config (Dictionary), in_TransactionItem | Master orchestration flow |
| TriggerCheckProcess.xaml | in_ProcessName (String), in_Config, out_TriggerResult (String) | Trigger individual check process |
| MainProcessTriggering.xaml | in_ProcessList (DataTable), in_Config | Batch process triggering |

**Flow:**
1. InitAllSettings → Load Config.xlsx + Orchestrator Assets
2. Determine check schedule from DynamicScheduler asset
3. For each scheduled check → TriggerCheckProcess (invokes Orchestrator job or direct workflow)
4. Monitor completion or apply automation end time constraint
5. On completion → trigger Status Report

### 2.2 MDS.112.OCFC.Sentinel

| Field | Value |
|---|---|
| UiPath Process Name | MDS_OCFC_Sentinel |
| Trigger | Invoked by Orchestrator |
| REFramework Role | Performer (non-queue, sequential) |

**Workflow List:**

| Workflow | Arguments | Purpose |
|---|---|---|
| SentinelMain.xaml | in_Config | Main sentinel flow |
| CheckESPAccess.xaml | in_Config, out_ESPStatus (Boolean) | Validate ESP system connectivity |
| CheckClientConfig.xaml | in_Config, out_ClientStatus (Boolean) | Validate client configurations |
| MonitorProcesses.xaml | in_Config, out_ProcessStatuses (DataTable) | Monitor running check processes |
| GenerateSentinelReport.xaml | in_ProcessStatuses, in_Config | Generate sentinel status report |
| SentinelSpecificActions.xaml | in_Config | Execute sentinel-specific operations |

**Flow:**
1. InitAllSettings → Load Config + Assets
2. CheckESPAccess → If fail → BusinessRuleException, halt all
3. CheckClientConfig → If fail → notify, skip affected clients
4. For each monitoring cycle → MonitorProcesses
5. When all checks complete → GenerateSentinelReport
6. Send collective notifications

### 2.3 Factor Generic Process (Dispatcher/Performer)

| Field | Value |
|---|---|
| UiPath Process Name | MDS_OCFC_FactorGeneric |
| Trigger | Invoked by Orchestrator |
| REFramework Role | Dispatcher + Performer |
| Queue | Multiple (Null Factor, Stale Factor, Factor Variance, Duplicate Factor, Factor Rate Consistency) |

**Dispatcher Workflow:**

| Workflow | Arguments | Purpose |
|---|---|---|
| FactorGenericDispatcher.xaml | in_Config, in_CheckType (String) | Load items from DB and add to queue |
| DB_Interactions.xaml | in_Query, in_Config, out_DataTable | Execute DB queries with retry |
| AddToQueue.xaml | in_QueueName, in_ItemData, in_Config | Add item to Orchestrator queue |

**Performer Workflow:**

| Workflow | Arguments | Purpose |
|---|---|---|
| FactorGenericPerformer.xaml | in_Config, in_TransactionItem | Process individual queue item |
| ESP_Extended.xaml | in_SecurityID, in_Config, out_ESPData | Retrieve ESP data |
| MDP_HTTP_DataRetriever.xaml | in_SecurityID, in_Config, out_MDPData | Retrieve MDP data |
| RKS_Wrapper.xaml | in_SecurityID, in_Action, in_Config, out_RKSData | RKS interaction |
| Logic_xxxx.xaml | in_ItemData, in_ESPData, in_MDPData, in_RKSData, out_Result | Check-specific logic |
| DB_Locking.xaml | in_LockName, in_Timeout, in_Config | Acquire/release DB locks |

**Flow (per queue item):**
1. GetTransactionData → get next queue item
2. Retrieve security data from DB (item details)
3. Acquire DB lock (if required by check type)
4. Retrieve comparison data from ESP/MDP via HTTP
5. Retrieve RKS data (if needed)
6. Apply check-specific logic (3006/3077/3122/3334-3019/3917)
7. If discrepancy → determine resolution (RKS update / notify / exception)
8. Execute resolution via appropriate object
9. Release DB lock
10. SetTransactionStatus (Complete/BusinessException/SystemException)

### 2.4 Security Integrity Processes

Each follows the Dispatcher/Performer pattern:

| Process | Queue | Key Logic | Complexity |
|---|---|---|---|
| DupeISIN | MDS.OCFC.Dupe ISIN | ISIN dedup matching, cross-ref validation | L |
| DupeSedols | (uses parent queue) | SEDOL dedup matching, 31-subsheet logic | XL |
| DupeClientSecurity | MDS.OCFC.Dupe Client Security | Client-security mapping dedup | L |
| NullSecurityCode | MDS.OCFC.Null Security Code | Null code detection and resolution | M |
| SecurityCurrencyCode | MDS.OCFC.Security Currency Code | Currency code validation | S |
| SecurityDetailsNull | MDS.OCFC.Security Details Null | Required field null checks | M |
| ExchangeCode | MDS.OCFC.Exchange Code | Exchange code validation | S |

### 2.5 Coupon & Date Processes

| Process | Queue | Key Logic | Complexity |
|---|---|---|---|
| CouponDateIntegrity | MDS.OCFC.Coupon Date Integrity | Date consistency across systems | L |
| CouponRateIntegrity | MDS.OCFC.Coupon Rate Integrity | Rate accuracy validation | S |
| CouponTypeChange | MDS.OCFC.Coupon Type Change | Type change detection (3019/3334 logic) | S |
| MissingAdjCouponDatedDate | MDS.OCFC.Missing Adjustable Coupon Dated Date | Missing date detection | S |
| EndDateValidation | MDS.OCFC.End Date Validation | End date validation | S |
| MaturityDate | MDS.OCFC.Maturity Date | Maturity date validation | S |
| MaturedExpiredPositions | MDS.OCFC.Matured or Expired Positions | Position status checks | S |

### 2.6 Specialized Processes

| Process | Queue | Key Logic | Complexity |
|---|---|---|---|
| ECFChecks | MDS.OCFC.ECF Checks (3 retries) | ECF data validation with upload | L |
| InflationIndex | MDS.OCFC.Inflation Index | Inflation index mapping/calc | XL |
| ConvertibleBonds | MDS.OCFC.Convertible Bonds | Convertible bond attributes | S |
| UnderlierSecurity | MDS.OCFC.Underlier Security | Underlier-derivative relationship | M |
| ValidateUSER1 | MDS.OCFC.Validate USER1 | USER1 field validation | XL |
| MultilistedUSER1 | (uses parent process) | Multilisted USER1 validation | M |
| SearchUSER1 | (uses parent process) | USER1 search operations | M |
| PrincipalType | MDS.OCFC.Principal Type Name Integrity | Principal type classification | S |
| SMFCalculation | (no queue — batch) | SMF calculation | S |
| RKSDataMartsValidation | (batch) | RKS vs DataMarts cross-validation | M |

---

## 3. Object/Library Design

### 3.1 MCH Library (Partial Reuse from 058)

| # | Workflow | Source | Arguments | Complexity | Notes |
|---|---|---|---|---|---|
| 1 | MCH_Launch.xaml | ♻️ 058 | in_Config | XS | Import from 058 |
| 2 | MCH_Login.xaml | ♻️ 058 | in_Config, in_Credential | S | Import from 058 |
| 3 | MCH_CheckIfLoggedIn.xaml | ♻️ 058 | out_IsLoggedIn (Boolean) | XS | Import from 058 |
| 4 | MCH_NavigateToTransaction.xaml | ♻️ 058 | in_TransactionCode (String) | XS | Import from 058 |
| 5 | MCH_CloseApplication.xaml | ♻️ 058 | — | XS | Import from 058 |
| 6 | MCH_CleanUp.xaml | ♻️ 058 | — | XS | Import from 058 |
| 7 | MCH_RetrievePassword.xaml | ♻️ 058 | in_Domain, out_Password | XS | Import from 058 |
| 8 | MCH_CheckTransactionAccess.xaml | **NEW** | in_TransactionCode, out_HasAccess | S | New — verify user can access transaction |
| 9 | MCH_BFSE_DataRetrieval.xaml | **NEW** | in_SecurityID, in_Config, out_BFSEData (DataTable) | M | New — BFSE screen automation |
| 10 | MCH_BSEC_DataRetrieval.xaml | **NEW** | in_SecurityID, in_Config, out_BSECData (DataTable) | M | New — BSEC screen automation |
| 11 | MCH_BSEC_MultiLine.xaml | **NEW** | in_SecurityID, in_Config, out_MultiLineData (DataTable) | M | New — Multi-line BSEC retrieval |
| 12 | MCH_BSEC_IDF_GetData.xaml | **NEW** | in_SecurityID, in_Config, out_IDFData (DataTable) | M | New — BSEC IDF data retrieval |
| 13 | MCH_BSEC_IDF_MainScreen.xaml | **NEW** | in_SecurityID, in_Config, out_MainScreenData | M | New — BSEC IDF main screen |
| 14 | MCH_BFSE_IDF_GetData.xaml | **NEW** | in_SecurityID, in_Config, out_IDFData (DataTable) | M | New — BFSE IDF data retrieval |
| 15 | MCH_BFSE_IDF_Generic.xaml | **NEW** | in_SecurityID, in_Fields, out_GenericData | M | New — Generic BFSE IDF |
| 16 | MCH_BGOV_IDF_GetData.xaml | **NEW** | in_SecurityID, in_Config, out_BGOVData (DataTable) | M | New — BGOV IDF data retrieval |

> **MCH Reuse Summary:** 7 workflows reused from 058 (0 effort), 9 workflows NEW for 112 (require build).

### 3.2 RKS Library (All NEW — 35 workflows)

The RKS library is project-specific with 35 workflows covering:

| Group | Workflows | Total Actions | Complexity |
|---|---|---|---|
| Launch (3) | LaunchHTTP, LaunchEdge, LaunchUIA | 39 | L per workflow |
| Core (4) | MainObject, Wrapper, IssueSearch, UpdateLogic | 100 | L–XL |
| Fixed Income (4) | FixedIncome, FixedIncomeMaturityDate, FloatingInterest, AdditionalFixedIncomeInfo | 48 | M |
| End Date (4) | FuturesLastTradingDate, OptionsExpDate, RightsExpDate, WarrantsExpDate | 43 | M |
| Schedule (5) | CashFlowSchedule, CashFlowScheduleData, ScheduledPrincipalPayments, ScheduledPrincipalPaymentsEntry, StepTable | 59 | M |
| Data (7) | CrossReference, FactorHistory, AdjustableTable, BaseRateName, CreditAdjustments, CreditInfoStatusWindow, DetailAnalysis | 77 | M |
| Other (8) | MarketData, PortfolioBrowser, Schedules, SecurityPaydown, TransactionHistory, ImportExternalData, AddNewFactorLine, AdditionalIssueTypeInfo, MDPValidation | 69 | S–M |

### 3.3 HTTP/API Libraries (All NEW — 9 workflows)

| # | Workflow | Actions | Systems | Complexity |
|---|---|---|---|---|
| 1 | ECF_HTTP.xaml | 19 | ECF API — login, queries, status updates | L |
| 2 | ERP_HTTP.xaml | 6 | ERP API — login, queries | S |
| 3 | MDP_HTTP_DataRetriever.xaml | 22 | MDP API — login, security/factor queries, ForgeRock auth | L |
| 4 | MDP_HTTP_Extended.xaml | 12 | Extended MDP queries | M |
| 5 | OCF_HTTP.xaml | 17 | OCF API — data source queries | M |
| 6 | RDOC_HTTP.xaml | 6 | RDOC API — document retrieval | S |
| 7 | RMG_HTTP.xaml | 14 | RMG API — template upload, report approval | M |
| 8 | SSCD_HTTP.xaml | 15 | SSCD API — classification data queries | M |
| 9 | IMS_HUB.xaml | 10 | IMS Hub web service (SOAP) | M |

### 3.4 Web Object Libraries (All NEW — 4 workflows)

| # | Workflow | Actions | Complexity |
|---|---|---|---|
| 1 | ERP_WebObject.xaml | 23 | L — Browser automation with 2FA |
| 2 | GTM_WebObject.xaml | 20 | L — Browser automation with 2FA |
| 3 | PSDL_WebObject.xaml | 23 | L — Browser automation |
| 4 | ECF_DataUpload.xaml | 10 | M — File upload automation |

### 3.5 ESP Library (All NEW — 3 workflows)

| # | Workflow | Actions | Complexity |
|---|---|---|---|
| 1 | ESP_Main.xaml | 20 | L — ODBC/HTTP data retrieval |
| 2 | ESP_Extended.xaml | 41 | XL — Cross-ref, factor tables, transactions |
| 3 | ESP_MDPValidation.xaml | 5 | S — ESP-MDP cross-validation |

### 3.6 Business Logic Libraries (All NEW — 32 workflows)

| Group | Workflow Count | Total Actions | Complexity | Notes |
|---|---|---|---|---|
| 3019/3334 Logic | 16 | 90 | S–L | Coupon type scenario logic (CORP/GOVT, MUNI, MTGE, Float, Step) |
| 3077 Logic | 1 | 12 | M | RKS update logic |
| 3122 Logic | 4 | 44 | M–L | MDP logic, target date, transaction level |
| 3917 Logic | 1 | 8 | S | MDP logic |
| Other Logic | 10 | 101 | S–XL | CPN_TYP, MVP2/MVP4, Principal Type, Master File Validation |

### 3.7 Infrastructure Libraries (All NEW — 11 workflows)

| # | Workflow | Actions | Complexity | Notes |
|---|---|---|---|---|
| 1 | DB_Interactions.xaml | 12 | M | DB CRUD with retry (configurable attempts/sleep) |
| 2 | DB_Locking.xaml | 6 | S | Acquire/release locks with timeout |
| 3 | MailWrapper.xaml | 11 | M | Email orchestration |
| 4 | CollectiveNotification.xaml | 6 | S | Batch notification grouping |
| 5 | MainProcessTriggering.xaml | 11 | M | Sub-process invocation |
| 6 | IMT_ItemFinalization.xaml | 3 | XS | Item finalization |
| 7 | ECF_DataUpload.xaml | 10 | M | ECF file upload |
| 8 | WebserviceLog.xaml | 3 | XS | API call logging |
| 9 | OCF_DataSourceWrapper.xaml | 5 | S | Data source abstraction |
| 10 | TBA_RKSModification.xaml | 4 | S | TBA RKS modification |
| 11 | TBA_RKSOrchestrator.xaml | 6 | S | TBA orchestration |

### 3.8 Utility Libraries

| # | Workflow | Actions | Complexity | Source |
|---|---|---|---|---|
| 1 | CSharp_Utilities.xaml | 16 | M | NEW — Convert C# functions to VB.NET/Invoke Code |
| 2 | VB_Utilities.xaml | 2 | XS | NEW |
| 3 | VLOOKUP_Object.xaml | 2 | XS | Shared (MOA.095.DVPC) |

### 3.9 Reused Libraries (from 058 — 0 effort)

| Library | Workflows | Source | 112 Effort |
|---|---|---|---|
| SSC.Platform.Logging | 3 (WriteProcessLogs, WriteTransactionLogs, InitializeLogger) | 058 | **0h** |
| SSC.Platform.LoginAgent | 2 (GetSubIDPassword, ValidateCredential) | 058 | **0h** |
| SSC.Custom.Environment | 2 (GetDownloadsDefaultPath, GetMachineName) | 058 | **0h** |
| Common Workflows | 6 (GetUserName, GetCredential, GetStartUpUTC, WriteProcessLog, WriteTransLog, KillExcel) | 058 | **0h** |
| WorkQueue Wrappers | 6 (Tag, Untag, Defer, Complete, Exception, AddToQueue) | 058 | **0h** |
| REFramework + Config.xlsx | Full template | 058 | **0h** |
| MCH Core (7 WF) | Launch, Login, CheckIfLoggedIn, Navigate, Close, CleanUp, RetrievePassword | 058 | **0h** |

---

## 4. Queue Design

### 4.1 Orchestrator Queue Configuration

| # | Queue Name | Key Field | Max Retry | SLA (min) | Deferred | Notes |
|---|---|---|---|---|---|---|
| 1 | MDS_OCFC_ConvertibleBonds | ItemKey | 1 | — | No | |
| 2 | MDS_OCFC_CouponDateIntegrity | ItemKey | 1 | — | No | |
| 3 | MDS_OCFC_CouponRateIntegrity | ItemKey | 1 | — | No | |
| 4 | MDS_OCFC_CouponTypeChange | ItemKey | 1 | — | No | |
| 5 | MDS_OCFC_DupeClientSecurity | ItemKey | 1 | — | No | |
| 6 | MDS_OCFC_DupeISIN | ItemKey | 1 | — | No | |
| 7 | MDS_OCFC_DuplicateFactor | ItemKey | 1 | — | No | |
| 8 | MDS_OCFC_ECFChecks | MF_ITEM_KEY | 3 | — | No | Higher retry for external system |
| 9 | MDS_OCFC_EndDateValidation | ItemKey | 1 | — | No | |
| 10 | MDS_OCFC_ExchangeCode | ItemKey | 1 | — | No | |
| 11 | MDS_OCFC_FactorRateConsistency | ItemKey | 1 | — | Yes | Defer time: 1 min |
| 12 | MDS_OCFC_FactorVariance | ItemKey | 1 | — | No | |
| 13 | MDS_OCFC_InflationIndex | ItemKey | 1 | — | No | |
| 14 | MDS_OCFC_Mails | Subject | 10 | — | No | High retry for email delivery |
| 15 | MDS_OCFC_MaturedExpiredPositions | ItemKey | 1 | — | No | |
| 16 | MDS_OCFC_MaturityDate | ItemKey | 1 | — | No | |
| 17 | MDS_OCFC_MDL_IntraPosSM_FactorRateConsistency | ItemKey | 1 | — | No | |
| 18 | MDS_OCFC_MissingAdjCouponDatedDate | ItemKey | 1 | — | No | |
| 19 | MDS_OCFC_NullFactor | ItemKey | 1 | — | Yes | Defer time configured |
| 20 | MDS_OCFC_NullSecurityCode | ItemKey | 1 | — | No | |
| 21 | MDS_OCFC_PrincipalTypeNameIntegrity | ItemKey | 1 | — | Yes | Defer time configured |
| 22 | MDS_OCFC_SecurityCurrencyCode | ItemKey | 1 | — | No | |
| 23 | MDS_OCFC_SecurityDetailsNull | ItemKey | 1 | — | Yes | Defer time configured |
| 24 | MDS_OCFC_StaleFactor | ItemKey | 1 | — | No | |
| 25 | MDS_OCFC_UnderlierSecurity | ItemKey | 1 | — | No | |
| 26 | MDS_OCFC_ValidateUSER1 | ItemKey | 1 | — | No | |

### 4.2 Queue Item Schema (Common Fields)

| Field | Type | Description |
|---|---|---|
| ItemKey | String | Unique security identifier |
| SecurityID | String | Internal security ID |
| CheckCode | String | Check type identifier (3006, 3077, 3122, etc.) |
| ClientCode | String | Client identifier |
| ProcessingStatus | String | Current processing status |
| SourceSystem | String | Data source (ESP/MDP/RKS) |
| ExceptionDetails | String | Exception description if any |
| RetryCount | Integer | Current retry count |

---

## 5. Asset & Credential Design

### 5.1 UiPath Orchestrator Assets

| # | Asset Name | Type | Description | Scope | Used By |
|---|---|---|---|---|---|
| 1 | MDS_OCFC_OrchestratorProcessName | Text | Orchestrator process name | Folder | Orchestrator |
| 2 | MDS_OCFC_DynamicScheduler | Text | Check schedule configuration | Folder | Orchestrator |
| 3 | MDS_OCFC_AutomationEndTime | Text | Daily cutoff time | Folder | All processes |
| 4 | MDS_OCFC_RootDirectory | Text | Root working directory | Folder | All processes |
| 5 | MDS_OCFC_TestingFlag | Boolean | Testing mode toggle | Folder | All processes |
| 6 | MDS_OCFC_CreateSplunkProcessLogs | Boolean | Splunk process log toggle | Folder | Logging |
| 7 | MDS_OCFC_CreateSplunkTransactionLogs | Boolean | Splunk transaction log toggle | Folder | Logging |
| 8 | MDS_OCFC_DBInteractionsRetryAttemptAmount | Integer | DB retry count | Folder | DB_Interactions |
| 9 | MDS_OCFC_DBInteractionsRetrySleep | Integer | DB retry sleep (ms) | Folder | DB_Interactions |
| 10 | MDS_OCFC_WQMaxAttempts | Integer | Max WQ attempts | Folder | WorkQueue |
| 11 | MDS_OCFC_WQTimeout | Integer | WQ timeout (ms) | Folder | WorkQueue |
| ... | *(272 total — full list in Config.xlsx Assets sheet)* | | | | |

### 5.2 Credential Assets (CyberArk)

| # | Asset Name | Type | CyberArk Safe | Used By |
|---|---|---|---|---|
| 1 | CyberArk_ECF_Credential | Credential | robmfa | ECF_HTTP, ECF_DataUpload |
| 2 | CyberArk_ERP_Credential | Credential | robmfa | ERP_HTTP, ERP_WebObject |
| 3 | CyberArk_ERP_2FA_Credential | Credential | edr | ERP_WebObject (2FA) |
| 4 | CyberArk_GTM_2FA_Credential | Credential | edr | GTM_WebObject (2FA) |
| 5 | CyberArk_IMSHUB_Credential | Credential | edr | IMS_HUB |
| 6 | CyberArk_MCH_Credential | Credential | racf | MCH_Login |
| 7 | CyberArk_MDP_Credential | Credential | adcorp | MDP_HTTP_DataRetriever |
| 8 | CyberArk_MDP_SecurID_Credential | Credential | robmfa | MDP_HTTP (SecurID) |
| 9 | CyberArk_PSDL_Credential | Credential | mdpapp alias | PSDL_WebObject |
| 10 | CyberArk_RDOC_Credential | Credential | edr | RDOC_HTTP |
| 11 | CyberArk_RKS_Credential | Credential | adcorp | RKS_Launch* |
| 12 | CyberArk_RKS_SecurID_Credential | Credential | robmfa | RKS_Launch* (SecurID) |

---

## 6. Exception Handling Matrix

| # | Exception Type | Source Workflow | Handling Action | Retry | Notification |
|---|---|---|---|---|---|
| 1 | ESP Access Failure | CheckESPAccess | BusinessRuleException — halt all processes | No | Yes — immediate escalation |
| 2 | MCH Login Failure | MCH_Login | SystemException — retry 2x then escalate | Yes (2x) | Yes |
| 3 | RKS Launch Failure | RKS_Launch* | SystemException — retry with alternate mode (HTTP→Edge→UIA) | Yes (3 modes) | Yes on final failure |
| 4 | RKS Session Timeout | RKS_Wrapper | SystemException — relaunch and retry | Yes (2x) | No |
| 5 | MDP Auth Failure | MDP_HTTP_DataRetriever | SystemException — retry with ForgeRock/SecurID fallback | Yes (2x) | Yes on final failure |
| 6 | DB Lock Timeout | DB_Locking | SystemException — wait and retry per lock timeout | Yes (configurable) | No |
| 7 | DB Query Failure | DB_Interactions | SystemException — retry per configured attempts/sleep | Yes (configurable) | Yes on final failure |
| 8 | ECF HTTP Error | ECF_HTTP | SystemException — retry (max 3 from queue config) | Yes (3x) | Yes on final failure |
| 9 | Email Send Failure | MailWrapper | SystemException — retry (max 10 from queue config) | Yes (10x) | No |
| 10 | Data Integrity Mismatch | Business Logic workflows | BusinessRuleException — log and skip item | No | Yes — notification to BU |
| 11 | Missing Security Data | FactorGenericPerformer | BusinessRuleException — mark item as exception | No | Yes — added to exception securities list |
| 12 | 2FA Token Failure | ERP/GTM WebObject | SystemException — retry once, then halt | Yes (1x) | Yes |
| 13 | File Upload Failure | ECF_DataUpload, RMG_HTTP | SystemException — retry 2x | Yes (2x) | Yes |
| 14 | Queue Item Deferred | DeferItem.xaml | Deferred — reprocess after configured timeout | Auto | No |
| 15 | Automation End Time Exceeded | Orchestrator | BusinessRuleException — graceful shutdown | No | Yes — status report |

---

## 7. Logging Design

### 7.1 Log Fields

| Field | Type | Description |
|---|---|---|
| ProcessName | String | UiPath process name |
| TransactionID | String | Queue item reference |
| SecurityID | String | Security identifier being processed |
| CheckCode | String | Check type (3006, 3077, etc.) |
| ClientCode | String | Client identifier |
| Action | String | Operation performed |
| Status | String | Success/Failure/Exception |
| Duration | TimeSpan | Operation duration |
| MachineName | String | Robot machine name |
| Timestamp | DateTime | UTC timestamp |

### 7.2 Log Levels

| Level | When |
|---|---|
| Trace | Detailed step entry/exit (debug only) |
| Info | Transaction start, complete, key milestones |
| Warn | Retry triggered, deferred item, non-critical issue |
| Error | Exception caught, item marked as exception |
| Fatal | Process-level failure, unrecoverable error |

### 7.3 PII Exclusion Rules
- Never log credential values, passwords, or tokens
- Never log full connection strings (mask password portions)
- Log security IDs and check codes (not PII)
- Never log email content body — only subject and recipient count

---

## 8. Selector Strategy

### 8.1 MCH (Terminal Emulation)
- UiPath Terminal Activities (TN3270)
- Field-based selectors using row/column positions
- Screen recognition via screen title text
- Recovery: check screen state before each operation

### 8.2 RKS (Citrix/Browser)
- Three launch modes: HTTP, Edge, UIA
- Dynamic selectors using `aaname` and `idx` attributes
- Anchor-based selectors for table cells
- Fallback: retry with alternate launch mode
- Window title matching with wildcards: `RKS*`

### 8.3 Web Applications (ERP, GTM, PSDL)
- CSS selectors preferred for web elements
- `id` attribute primary; `class` + `tag` fallback
- iFrame handling for embedded content
- 2FA modal detection via visible element check

### 8.4 HTTP/API Objects
- No UI selectors needed — UiPath HTTP Request activities
- JSON/XML response parsing via Deserialize activities
- Error detection via HTTP status codes and response body patterns

---

## 9. Config.xlsx Design

### 9.1 Settings Sheet

| Name | Value | Description |
|---|---|---|
| OrchestratorURL | (Orchestrator URL) | Orchestrator connection |
| ProcessName | MDS_OCFC | Process identifier |
| LogLevel | Info | Default log level |
| MaxRetryNumber | 3 | Default retry count |
| RetryNumberGetTransactionItem | 3 | Queue get retry |
| RetryNumberSetTransactionStatus | 3 | Queue set retry |

### 9.2 Constants Sheet

| Name | Value | Description |
|---|---|---|
| MaxConsecutiveSystemExceptions | 3 | Halt after N consecutive system exceptions |
| QueueFolder | MDS-OCFC-112 | Orchestrator folder name |
| ScreenshotOnException | True | Capture screenshot on error |
| ScreenshotFolderPath | (from asset) | Screenshot save location |

### 9.3 Assets Sheet (Orchestrator Asset references)

| Name | Asset | Description |
|---|---|---|
| MDS_OCFC_RootDirectory | MDS_OCFC_RootDirectory | Root working directory |
| MDS_OCFC_TestingFlag | MDS_OCFC_TestingFlag | Testing toggle |
| MDS_OCFC_AutomationEndTime | MDS_OCFC_AutomationEndTime | Cutoff time |
| ... | *(All 272 env vars mapped here)* | |

---

## 10. Naming Conventions

| Element | Convention | Example |
|---|---|---|
| Process | MDS_OCFC_{ProcessName} | MDS_OCFC_FactorGeneric |
| Workflow File | {Module}_{Action}.xaml | RKS_CrossReference.xaml |
| Workflow Argument (In) | in_{Name} | in_SecurityID |
| Workflow Argument (Out) | out_{Name} | out_BSECData |
| Workflow Argument (In/Out) | io_{Name} | io_ProcessStatus |
| Variable | camelCase | transactionItem, securityData |
| Queue | MDS_OCFC_{QueueName} | MDS_OCFC_NullFactor |
| Asset | MDS_OCFC_{AssetName} | MDS_OCFC_RootDirectory |
| Credential | CyberArk_{System}_Credential | CyberArk_MCH_Credential |
| Library Folder | {Module}/ | RKS/, HTTP/, BusinessLogic/ |
| Config Sheet Key | {Section}_{Name} | Settings_ProcessName |
