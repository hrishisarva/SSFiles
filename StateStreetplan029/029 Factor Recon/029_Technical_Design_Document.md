# 029 – TECHNICAL DESIGN DOCUMENT

| Field | Value |
|---|---|
| Document ID | 029-TDD-001 |
| Project | BOAT.029 – COB FACTOR RECON |
| Source | 029_Business_Requirements_Document.md |
| Date | 2026-04-08 |
| Status | Draft v1.0 |

---

## 1. Solution Architecture

### 1.1 UiPath Project Folder Structure

```
029_COB_Factor_Recon/
├── project.json
├── Main.xaml                                  ← REFramework entry point
├── Data/
│   ├── Config.xlsx                            ← Settings, Constants, Assets sheets
│   └── Templates/
│       ├── ProcessLog_Template.xlsx
│       └── TransactionLog_Template.xlsx
├── Framework/                                 ← REFramework standard (0 effort - from 057)
│   ├── InitAllSettings.xaml
│   ├── InitAllApplications.xaml
│   ├── KillAllProcesses.xaml
│   ├── CloseAllApplications.xaml
│   ├── GetTransactionData.xaml
│   ├── Process.xaml
│   ├── SetTransactionStatus.xaml
│   └── RetryCurrentTransaction.xaml
├── Process/
│   ├── 01_Sentinel/
│   │   ├── CheckPrerequisites.xaml
│   │   ├── MapNetworkDrive.xaml
│   │   ├── CheckMCHConnectivity.xaml
│   │   ├── CheckRDRConnectivity.xaml
│   │   ├── CheckBloombergConnectivity.xaml
│   │   └── GenerateStatusReport.xaml
│   ├── 02_Dispatcher_MainScheduler/
│   │   ├── ReadMasterScheduler.xaml
│   │   ├── ValidateScheduler.xaml
│   │   ├── CheckHolidays.xaml
│   │   ├── SetQueuePriority.xaml
│   │   └── PopulateFundListQueue.xaml
│   ├── 03_Dispatcher_Adhoc/
│   │   └── PopulateQueueAdhoc.xaml
│   ├── 04_Dispatcher_NOVAL/
│   │   └── PopulateQueueNOVAL.xaml
│   ├── 05_Performer_DataExtraction/
│   │   ├── GetFundList.xaml
│   │   ├── ExtractMCHData.xaml
│   │   ├── ExtractBloombergData.xaml
│   │   ├── ExtractRDRData.xaml
│   │   ├── MergeAndAggregateData.xaml
│   │   ├── WriteRawDataReport.xaml
│   │   └── PopulateAccountingQueue.xaml
│   ├── 06_Performer_TransformPosting/
│   │   ├── LoadAccountingData.xaml
│   │   ├── TransformData.xaml
│   │   ├── ValidatePostingData.xaml
│   │   ├── ExecutePosting.xaml
│   │   └── WriteLotLevelReport.xaml
│   ├── 07_SLA_Monitor/
│   │   ├── QueryPendingItems.xaml
│   │   └── EvaluateSLAThreshold.xaml
│   └── 08_Database_Cleanup/
│       └── CleanupDatabaseRecords.xaml
└── Reusable/
    ├── Common/                                ← 0 effort - from 057
    │   ├── GetCredential.xaml
    │   ├── WriteProcessLog.xaml
    │   ├── WriteTransactionLog.xaml
    │   ├── SendEmail.xaml
    │   ├── KillExcelProcess.xaml
    │   ├── GetUserName.xaml
    │   ├── GetStartUpUTCDatetime.xaml
    │   └── OleDbGetCollection.xaml
    └── WorkQueue/                             ← 0 effort - from 057
        ├── WQ_AddToQueue.xaml
        ├── WQ_GetNextItem.xaml
        ├── WQ_MarkCompleted.xaml
        ├── WQ_MarkException.xaml
        ├── WQ_SetData.xaml
        ├── WQ_Defer.xaml
        ├── WQ_TagItem.xaml
        ├── WQ_UntagItem.xaml
        └── WQ_GetPendingItems.xaml
```

### 1.2 UiPath Library Dependencies (NuGet)

| Library | Version | Source | Build Effort in 029 |
|---|---|---|---|
| **SSC.Platform.Logging** | 1.x.x | Import from 058 | **0h** |
| **SSC.Platform.LoginAgent** | 1.x.x | Import from 058 | **0h** |
| **SSC.Custom.Environment** | 1.x.x | Import from 058 | **0h** |
| **BOAT.COB.Excel.Utilities** | 1.x.x | Import from 058 | **0h** |
| **GIO.FR.MCH.Library** | 1.0.0 | Build new in 029 | ~80h |
| **GIO.FR.Accounting_WS.Library** | 1.0.0 | Build new in 029 | ~70h |
| **GIO.FR.DataTransformation.Library** | 1.0.0 | Build new in 029 | ~40h |
| **GIO.FR.Database.Library** | 1.0.0 | Build new in 029 | ~12h |
| **GIO.FR.MasterScheduler.Library** | 1.0.0 | Build new in 029 | ~35h |
| **GIO.FR.Reporting.Library** | 1.0.0 | Build new in 029 | ~25h |
| **GIO.FR.Posting.Library** | 1.0.0 | Build new in 029 | ~120h |
| UiPath.System.Activities | 23.10.x | UiPath Official Feed | 0h |
| UiPath.UIAutomation.Activities | 23.10.x | UiPath Official Feed | 0h |
| UiPath.Excel.Activities | 2.x.x | UiPath Official Feed | 0h |
| UiPath.Mail.Activities | 1.x.x | UiPath Official Feed | 0h |
| UiPath.Database.Activities | 1.x.x | UiPath Official Feed | 0h |
| UiPath.Web.Activities | 1.x.x | UiPath Official Feed | 0h |

### 1.3 Orchestrator Folder Structure

```
Orchestrator (2023.10.4)
└── SSC/
    └── GIO/
        └── 029_Factor_Recon/
            ├── Processes/
            │   ├── 029_FR_Sentinel
            │   ├── 029_FR_Dispatcher_MainScheduler
            │   ├── 029_FR_Dispatcher_Adhoc
            │   ├── 029_FR_Dispatcher_NOVAL
            │   ├── 029_FR_Performer_DataExtraction
            │   ├── 029_FR_Performer_TransformPosting
            │   ├── 029_FR_SLAMonitor
            │   └── 029_FR_DatabaseCleanup
            ├── Queues/
            │   ├── 029_FR_FundListFile_Queue
            │   └── 029_FR_AccountingDataFile_Queue
            ├── Assets/
            │   └── (23 assets - see Section 5)
            └── Robots/
                └── (unattended robot assigned)
```

---

## 2. Process Design

### 2.1 Process: 029_FR_Sentinel

| Property | Value |
|---|---|
| Process Type | Standalone (no queue) |
| Trigger | Scheduled – daily, 30 min before COB window |
| REFramework Role | Standalone (Init → Main → End) |
| Config Queue Name | N/A |
| Retry Count | 1 (non-retryable health check) |

**Workflow List:**

| Workflow | Arguments In | Arguments Out | Complexity |
|---|---|---|---|
| Main.xaml | – | – | M |
| Framework/InitAllSettings.xaml | – | Config Dictionary | XS (0 effort) |
| Framework/InitAllApplications.xaml | Config | – | S |
| Process/01_Sentinel/CheckPrerequisites.xaml | Config | AllPassFlag, FailedChecks | M |
| Process/01_Sentinel/MapNetworkDrive.xaml | SharedDrivePath | Success | S |
| Process/01_Sentinel/CheckMCHConnectivity.xaml | MCHTerminalPath, MCHCredential | MCHStatus | M |
| Process/01_Sentinel/CheckRDRConnectivity.xaml | RDRCloudClientAddress, RDRCredential | RDRStatus | S |
| Process/01_Sentinel/CheckBloombergConnectivity.xaml | BloombergBaseURL | BloombergStatus | S |
| Process/01_Sentinel/GenerateStatusReport.xaml | StatusResults | ReportFilePath | S |
| Reusable/Common/SendEmail.xaml | To, Subject, Body, Attachments | – | XS (0 effort) |
| Framework/KillAllProcesses.xaml | – | – | XS (0 effort) |

**Numbered Flow:**
1. InitAllSettings loads Config.xlsx and Orchestrator Assets into Config dictionary
2. MapNetworkDrive maps required UNC paths for shared drive access
3. GetCredential (MCH) retrieves MCH SubID/password from CyberArk
4. GetCredential (RDR) retrieves RDR credentials from CyberArk
5. CheckMCHConnectivity: launch MCH terminal, attempt login, verify screen, logoff
6. CheckRDRConnectivity: instantiate RDR cloud client DLL, ping endpoint
7. CheckBloombergConnectivity: HTTP GET to Bloomberg base URL, expect 200 response
8. CheckPrerequisites: validate file paths exist, test read/write on shared drive
9. If any check fails: email Alert to RPASupport_L1_EmailID and Business_EmailID
10. GenerateStatusReport: write sentinel results to dated status file
11. KillAllProcesses: clean up terminal, browser, Excel processes

### 2.2 Process: 029_FR_Dispatcher_MainScheduler

| Property | Value |
|---|---|
| Process Type | Dispatcher |
| Trigger | Scheduled – daily COB (per region schedule) |
| REFramework Role | Dispatcher |
| Output Queue | 029_FR_FundListFile_Queue |

**Workflow List:**

| Workflow | Arguments In | Arguments Out | Complexity |
|---|---|---|---|
| Main.xaml | – | – | M |
| Process/02_Dispatcher/ReadMasterScheduler.xaml | SchedulerFilePath | SchedulerDataTable | M |
| Process/02_Dispatcher/ValidateScheduler.xaml | SchedulerDataTable | ValidatedItems, Errors | M |
| Process/02_Dispatcher/CheckHolidays.xaml | ValidatedItems, Date | HolidayFilteredItems | L |
| Process/02_Dispatcher/SetQueuePriority.xaml | FilteredItems | PrioritizedItems | S |
| Process/02_Dispatcher/PopulateFundListQueue.xaml | PrioritizedItems | ItemsAdded | S |
| Reusable/Common/SendEmail.xaml (error) | To, Subject, Body | – | XS (0 effort) |

**Numbered Flow:**
1. InitAllSettings → load Config, assets
2. ReadMasterScheduler: OLEDB-read master scheduler Excel from GIO_FR_MASTERSCHED_FILEPATH asset
3. ValidateScheduler: call GIO.FR.MasterScheduler.Library — validate fields, validate values per 4 regional formats
4. CheckHolidays: for each region (APAC, EMEA, USIS, IIS), check holiday dates; remove holiday regions from batch
5. SetQueuePriority: assign priority per region/batch type rules
6. PopulateFundListQueue: WQ_AddToQueue for each filtered item → 029_FR_FundListFile_Queue
7. If validation errors: SendEmail to RPASupport_L1 with error detail report
8. KillAllProcesses

### 2.3 Process: 029_FR_Dispatcher_Adhoc

| Property | Value |
|---|---|
| Process Type | Dispatcher (manual trigger) |
| Trigger | Manual execution by RPA Support L1 |
| Input | Startup parameter: `SchedulerFilePath` (overrides asset) |

**Flow:** Same as MainScheduler but reads SchedulerFilePath from in_SchedulerFilePath startup argument. Skips holiday check (all items queued regardless of holidays).

### 2.4 Process: 029_FR_Dispatcher_NOVAL

| Property | Value |
|---|---|
| Process Type | Dispatcher (manual trigger) |
| Trigger | Manual execution |
| Behavior | Holiday check retained; field/value validation skipped |

**Flow:** ReadMasterScheduler → CheckHolidays only → SetQueuePriority → PopulateFundListQueue.

### 2.5 Process: 029_FR_Performer_DataExtraction

| Property | Value |
|---|---|
| Process Type | Performer (REFramework) |
| Trigger | Queue item available in 029_FR_FundListFile_Queue |
| REFramework Role | Performer |
| Input Queue | 029_FR_FundListFile_Queue |
| Output Queue | 029_FR_AccountingDataFile_Queue |
| Retry Count | 3 (system exceptions) |
| Max Consecutive Exceptions | 5 |

**Workflow List:**

| Workflow | Arguments In | Arguments Out | Complexity |
|---|---|---|---|
| Main.xaml | – | – | S (0 effort) |
| Framework/GetTransactionData.xaml | QueueName | TransactionItem | XS (0 effort) |
| Framework/Process.xaml | TransactionItem, Config | – | S (0 effort) |
| Process/05_Performer_DataExtraction/GetFundList.xaml | FundListFilePath | FundListCollection | M |
| Process/05_Performer_DataExtraction/ExtractMCHData.xaml | FundListCollection, MCHCredential, MCHScreens | AccountingDataCollection | L |
| Process/05_Performer_DataExtraction/ExtractBloombergData.xaml | AccountingDataCollection, BloombergBaseURL | BBDataCollection | L |
| Process/05_Performer_DataExtraction/ExtractRDRData.xaml | FundListCollection, RDRCredential, RDRAddress | RDRDataCollection | M |
| Process/05_Performer_DataExtraction/MergeAndAggregateData.xaml | AccountingDataCollection, BBDataCollection, RDRDataCollection | MergedCollection | M |
| Process/05_Performer_DataExtraction/WriteRawDataReport.xaml | MergedCollection, OutputPath | OutputFilePath | S |
| Process/05_Performer_DataExtraction/PopulateAccountingQueue.xaml | OutputFilePath, TransactionItem | – | S |
| Framework/SetTransactionStatus.xaml | TransactionItem, Status, Error | – | XS (0 effort) |

**Numbered Flow:**
1. GetTransactionData: dequeue next item from 029_FR_FundListFile_Queue
2. GetFundList: OLEDB-read fund list Excel file from queue item FundListFilePath
3. If FundList has >threshold records, use GetFundList OLEDB method; else standard Excel read
4. GetCredential (MCH): retrieve MCH SubID/password
5. ExtractMCHData: loop through fund list → call GIO.FR.MCH.Library BGMM for each fund
6. If RDR_Flag=Y from queue item: GetCredential (RDR) → ExtractRDRData via cloud client
7. ExtractBloombergData: HTTP GET Bloomberg WS for ISIN codes in collection
8. MergeAndAggregateData: MB lot-level merge → GIO.FR.Accounting_WS.Library MergeBB_LotLevel + Aggregate_Filter
9. WriteRawDataReport: write merged collection to Excel (GIO.FR.Reporting.Library)
10. PopulateAccountingQueue: WQ_AddToQueue → 029_FR_AccountingDataFile_Queue with output file path
11. WriteTransactionLog → ELK/Splunk
12. SetTransactionStatus Complete
13. On BusinessException: SetTransactionStatus BusinessException; no retry
14. On SystemException: retry up to 3x; if exhausted → SetTransactionStatus SystemException; email alert

### 2.6 Process: 029_FR_Performer_TransformPosting

| Property | Value |
|---|---|
| Process Type | Performer (REFramework) |
| Trigger | Queue item in 029_FR_AccountingDataFile_Queue |
| Input Queue | 029_FR_AccountingDataFile_Queue |
| Retry Count | 3 |
| Max Consecutive Exceptions | 5 |

**Workflow List:**

| Workflow | Arguments In | Arguments Out | Complexity |
|---|---|---|---|
| Process/06_Performer_TransformPosting/LoadAccountingData.xaml | AccountingDataFilePath | RawDataCollection | M |
| Process/06_Performer_TransformPosting/TransformData.xaml | RawDataCollection | TransformedCollection | M |
| Process/06_Performer_TransformPosting/ValidatePostingData.xaml | TransformedCollection | ValidatedCollection | S |
| Process/06_Performer_TransformPosting/ExecutePosting.xaml | ValidatedCollection, PostingType, MCHCredential | PostingResults | XL |
| Process/06_Performer_TransformPosting/WriteLotLevelReport.xaml | PostingResults, ReportPath | ReportFilePath | S |

**Numbered Flow:**
1. GetTransactionData: dequeue item from 029_FR_AccountingDataFile_Queue
2. GetCredential (MCH): retrieve MCH credentials
3. LoadAccountingData: read accounting data Excel file into DataTable
4. TransformData: invoke GIO.FR.DataTransformation.Library — Filter_Calculate → Set_Column_Data → Change_Column_Position → BGIR_WS_Call (if applicable)
5. ValidatePostingData: check all required columns populated, data types correct
6. ExecutePosting: based on PostingType from queue item:
   - PYDN: GIO.FR.Posting.Library FR.PYDN_Posting (or FR.PYDN_Posting_UMAS if UMAS mode)
   - PYUP: GIO.FR.Posting.Library FR.PYUP_Posting (or FR.PYUP_Posting_UMAS)
   - PYDA: GIO.FR.Posting.Library FR.PYDA_Posting (or FR.PYDA_Posting_UMAS)
7. WriteLotLevelReport: GIO.FR.Reporting.Library WriteDailyReport
8. SendEmail: distribute report to Business_EmailID
9. WriteTransactionLog → ELK/Splunk
10. SetTransactionStatus Complete

### 2.7 Process: 029_FR_SLAMonitor

| Property | Value |
|---|---|
| Process Type | Standalone (no queue) |
| Trigger | Scheduled — every 15 minutes during COB window |

**Workflow List:**

| Workflow | Arguments In | Arguments Out | Complexity |
|---|---|---|---|
| Process/07_SLA_Monitor/QueryPendingItems.xaml | QueueNames, Orchestrator Config | PendingItems | M |
| Process/07_SLA_Monitor/EvaluateSLAThreshold.xaml | PendingItems, SLAThreshold | BreachedItems | M |

**Flow:** Query both queues for New/InProgress items → for each, calculate elapsed time → if > SLA_Threshold → send email alert.

### 2.8 Process: 029_FR_DatabaseCleanup

| Property | Value |
|---|---|
| Process Type | Standalone (no queue) |
| Trigger | Scheduled (weekly/on-demand) |

**Flow:** GetCount eligible records → loop in Delete_Count batches via GIO.FR.Database.Library → SendEmail completion summary.

---

## 3. Library Design

### 3.1 GIO.FR.MCH.Library (New — Build in 029)

| Workflow | In Arguments | Out Arguments | Complexity | Hours |
|---|---|---|---|---|
| MCH_StartUp | MCHTerminalPath, MCHSessionFilePath | MCHInstance | S | 6h |
| MCH_CheckIfLogged | MCHInstance | IsLoggedIn | S | 4h |
| MCH_Login | MCHInstance, SubID, Password | LoginSuccess | S | 6h |
| MCH_Logout | MCHInstance | – | XS | 3h |
| MCH_BGMM | MCHInstance, FundCode | BGMMData | M | 12h |
| MCH_BGIR | MCHInstance, FundCode | BGIRData | M | 12h |
| MCH_BSEC | MCHInstance, SecurityCode | BSECData | M | 12h |
| MCH_PYDN | MCHInstance, PostingData | PostingResult | L | 20h |
| MCH_PYUP | MCHInstance, PostingData | PostingResult | L | 20h |
| MCH_PYDA | MCHInstance, PostingData | PostingResult | L | 20h |
| MCH_BPOP | MCHInstance, BatchData | BPOPResult | M | 12h |
| MCH_CleanUp | MCHInstance | – | XS | 2h |
| **Total** | | | | **~129h → target 80h with reuse of MCH Login pattern from 058** |

**Note:** MCH_Login and MCH_Logout patterns can be adapted from BOAT.COB.MCH.Library (058). Screen-specific workflows (BGMM, BGIR, BSEC, PYDN, PYUP, PYDA, BPOP) are Factor Recon-specific and require new selectors and business logic. With login/logoff pattern reuse, estimated at **80h net**.

### 3.2 GIO.FR.Accounting_WS.Library (New — Build in 029)

Consolidates GIO.FR.Accounting_WS_Data_Extraction.Object + GIO.FR.RDR.Object:

| Workflow | In Arguments | Out Arguments | Complexity | Hours |
|---|---|---|---|---|
| GMM_WS_DataExtraction | FundCode, GMMEndpoint | AccountingData | L | 20h |
| Group_CUSIPFundData | AccountingData | GroupedData | M | 12h |
| Bloomberg_HTTP_WS_Extract | ISINList, BaseURL | BBData | L | 20h |
| RDR_Login | RDRAddress, Credential | RDRSession | M | 8h |
| RDR_WS_DataExtraction | RDRSession, FundList | RDRData | L | 16h |
| MergeBB_LotLevel | BBData, LotLevelData | MergedData | M | 12h |
| Aggregate_Filter | MergedData, FilterRules | FinalData | M | 12h |
| Bloomberg_Webpage | BaseURL | PageContent | S | 6h |
| CleanUp | Sessions | – | XS | 2h |
| **Total** | | | | **~108h → target 70h with parallel dev pattern** |

### 3.3 GIO.FR.DataTransformation.Library (New — Build in 029)

| Workflow | In Arguments | Out Arguments | Complexity | Hours |
|---|---|---|---|---|
| Filter_Calculate | RawData, FilterRules | FilteredData | M | 14h |
| Set_Column_Data | DataTable, ColName, Value | UpdatedDataTable | S | 6h |
| Change_Column_Position | DataTable, ColOrder | ReorderedDataTable | S | 4h |
| BGIR_WS_Call | FundData, BGIREndpoint | BGIRResult | M | 12h |
| BGI_Loop | BGIRResult, DataTable | EnrichedDataTable | M | 12h |
| Write_ToExcel | DataTable, FilePath, SheetName | – | S | 6h |
| CleanUp | – | – | XS | 2h |
| **Total** | | | | **~56h → target 40h** |

### 3.4 GIO.FR.Database.Library (New — Build in 029)

| Workflow | In Arguments | Out Arguments | Complexity | Hours |
|---|---|---|---|---|
| DB_GetConnection | ConnectionString | DBConnection | S | 4h |
| DB_ExecuteQuery | DBConnection, SQLQuery, BatchSize | ResultDataTable | S | 6h |
| DB_BulkInsert | DBConnection, DataTable, TableName, BulkBatchSize | RowsInserted | S | 6h |
| DB_CleanUp | DBConnection, DeleteSQL, BatchSize, LoopCount | DeletedCount | M | 10h |
| DB_CloseConnection | DBConnection | – | XS | 2h |
| **Total** | | | | **~28h → target 12h with Database Activities standard patterns** |

### 3.5 GIO.FR.MasterScheduler.Library (New — Build in 029)

| Workflow | In Arguments | Out Arguments | Complexity | Hours |
|---|---|---|---|---|
| ReadMasterSchedulerFile | FilePath | RawDataTable | M | 10h |
| FieldExistsValidation | DataTable, RequiredFields | ValidationResult | S | 6h |
| ValuesValidation | DataTable | ValidatedRows, InvalidRows | S | 6h |
| HolidayDateFormat_USIS | DataTable | UISHolidayDates | S | 5h |
| HolidayDateFormat_EMEA | DataTable | EMEAHolidayDates | S | 5h |
| HolidayDateFormat_APAC | DataTable | APACHolidayDates | S | 5h |
| HolidayDateFormat_IIS | DataTable | IISHolidayDates | S | 5h |
| CleanUp | FileHandle | – | XS | 2h |
| **Total** | | | | **~44h → target 35h** |

### 3.6 GIO.FR.Reporting.Library (New — Build in 029)

| Workflow | In Arguments | Out Arguments | Complexity | Hours |
|---|---|---|---|---|
| CreateReport | ReportName, TemplatePath | WorkbookPath | M | 10h |
| CreateWorksheetAndWriteCollection | WorkbookPath, SheetName, DataTable | – | M | 10h |
| WriteDailyReport | LotLevelData, ReportPath | ReportFilePath | M | 12h |
| SendEmail | To, CC, Subject, Body, Attachments | – | XS (reuse Common/SendEmail) | 2h |
| CleanUp | WorkbookHandle | – | XS | 2h |
| **Total** | | | | **~36h → target 25h** |

### 3.7 GIO.FR.Posting.Library (New — Build in 029)

| Workflow | In Arguments | Out Arguments | Complexity | Hours |
|---|---|---|---|---|
| CreateInstance | TemplatePath | WorkbookInstance | S | 6h |
| DeleteSheetContents | WorkbookInstance, SheetName | – | S | 4h |
| CreateSheetAndWriteContents | WorkbookInstance, SheetName, DataTable | – | M | 12h |
| Check_PostingType | QueueItemData | PostingType, IsUMAS | S | 6h |
| MCH_Login | MCHInstance, Credential | LoginSuccess | XS (delegate to GIO.FR.MCH.Library) | 0h |
| MCH_Logoff | MCHInstance | – | XS (delegate to GIO.FR.MCH.Library) | 0h |
| Main_Action | WorkbookInstance, PostingType, MCHInstance | PostingResult | L | 20h |
| FR_PYDN_Posting | MCHInstance, PostingData | PostingResult | L | 18h |
| FR_PYUP_Posting | MCHInstance, PostingData | PostingResult | L | 18h |
| FR_PYDA_Posting | MCHInstance, PostingData | PostingResult | L | 18h |
| FR_PYDN_Posting_UMAS | UMASInstance, PostingData | PostingResult | L | 14h |
| FR_PYUP_Posting_UMAS | UMASInstance, PostingData | PostingResult | L | 14h |
| FR_PYDA_Posting_UMAS | UMASInstance, PostingData | PostingResult | L | 14h |
| CleanUp | WorkbookInstance, UMASInstance | – | XS | 2h |
| **Total** | | | | **~146h → target 120h (PYDN/PYUP/PYDA pattern reuse reduces effort)** |

### 3.8 Imported Libraries (0 Effort)

| Library | Source | Workflows | 029 Effort |
|---|---|---|---|
| SSC.Platform.Logging | 058 – Market Profiling | WriteProcessLogs, WriteTransactionLogs, InitializeLogger | **0h** |
| SSC.Platform.LoginAgent | 058 – Market Profiling | GetSubIDPassword, ValidateCredential | **0h** |
| SSC.Custom.Environment | 058 – Market Profiling | GetDownloadsDefaultPath, GetMachineName | **0h** |
| BOAT.COB.Excel.Utilities | 058 – Market Profiling | MergeExcelFiles, ReadWithOLEDB, SetTitusClassification | **0h** |

---

## 4. Queue Design

### 4.1 Queue: 029_FR_FundListFile_Queue

| Property | Value |
|---|---|
| Orchestrator Queue Name | 029_FR_FundListFile_Queue |
| Folder | SSC/GIO/029_Factor_Recon |
| Retry Count | 2 |
| SLA (minutes) | From 029_FR_SLA_Threshold asset |
| Unique Reference | BatchLabel + Region + ProcessingDate |

**Content Schema:**

| Field | Type | Required | Example |
|---|---|---|---|
| FundListFilePath | String | Yes | `\\server\share\fundlists\FL_20260408.xlsx` |
| Region | String | Yes | `APAC` |
| ProcessingDate | String | Yes | `08-Apr-2026` |
| BatchLabel | String | Yes | `FR_20260408_APAC_001` |
| SchedulerFilePath | String | Yes | `\\server\share\scheduler\MasterSched_20260408.xlsx` |
| PostingType | String | Yes | `PYDN,PYUP,PYDA` |
| RDR_Flag | String | Yes | `Y` |

**Status Codes:**

| Status | Meaning |
|---|---|
| New | Item added by dispatcher, not yet processed |
| In Progress | Performer dequeued and is processing |
| Successful | Data extraction completed; AccountingDataFile queue populated |
| Business Exception | Invalid fund data; logged; no retry |
| System Exception | Connectivity/system failure; retry up to 2x |

### 4.2 Queue: 029_FR_AccountingDataFile_Queue

| Property | Value |
|---|---|
| Orchestrator Queue Name | 029_FR_AccountingDataFile_Queue |
| Retry Count | 2 |
| SLA (minutes) | From 029_FR_SLA_Threshold asset |
| Unique Reference | FundListReference + ProcessingDate |

**Content Schema:**

| Field | Type | Required | Example |
|---|---|---|---|
| AccountingDataFilePath | String | Yes | `\\server\share\data\ACC_20260408_APAC.xlsx` |
| FundListReference | String | Yes | `FR_20260408_APAC_001` |
| Region | String | Yes | `APAC` |
| ProcessingDate | String | Yes | `08-Apr-2026` |
| PostingType | String | Yes | `PYDN,PYUP,PYDA` |
| ExtractionStatus | String | Yes | `Complete` |
| BloombergExtracted | String | Yes | `Y` |
| RDRExtracted | String | Yes | `Y` |

---

## 5. Asset & Credential Design

| # | Asset Name | Type | Description | Access Scope | Used By |
|---|---|---|---|---|---|
| A-01 | 029_FR_MCH_Credentials | Credential | MCH SubID + Password (via CyberArk) | Robot | Sentinel, DataExtraction, TransformPosting, MCH.Library |
| A-02 | 029_FR_RDR_Credentials | Credential | RDR cloud client login | Robot | Sentinel, DataExtraction, Accounting_WS.Library |
| A-03 | 029_FR_Bloomberg_Credentials | Credential | Bloomberg HTTP WS (if auth required) | Robot | Accounting_WS.Library |
| A-04 | 029_FR_AnalyticalLog_Flag | String | Toggle Splunk write (Y/N) | Robot | Logging.Library |
| A-05 | 029_FR_Batch_Flag | String | Enable batch processing mode (Y/N) | Robot | DataExtraction, TransformPosting |
| A-06 | 029_FR_Batch_Size | String | MCH extraction batch size (integer) | Robot | DataExtraction, MCH.Library |
| A-07 | 029_FR_Bloomberg_BaseURL | String | Bloomberg HTTP WS base URL | Robot | Accounting_WS.Library |
| A-08 | 029_FR_BulkCopy_BatchSize | String | SQL bulk insert batch size | Robot | Database.Library |
| A-09 | 029_FR_Business_EmailID | String | Business stakeholder email(s) | Robot | TransformPosting, SLAMonitor |
| A-10 | 029_FR_CC_EmailIDs | String | CC email addresses (semicolon list) | Robot | All email-sending processes |
| A-11 | 029_FR_CloudClient_FilePath | String | UNC path to RDR cloud client DLL | Robot | Accounting_WS.Library |
| A-12 | 029_FR_Delete_Count | String | Max DB rows deleted per run | Robot | DatabaseCleanup |
| A-13 | 029_FR_FundListCheck_Flag | String | Fund list count check flag (Y/N) | Robot | Dispatchers |
| A-14 | 029_FR_FundListReport_Folder | String | UNC path for fund list reports folder | Robot | Reporting.Library |
| A-15 | 029_FR_Loop_Count | String | Database loop iteration count | Robot | Database.Library |
| A-16 | 029_FR_MasterSched_FilePath | String | UNC path to master scheduler Excel | Robot | Dispatchers |
| A-17 | 029_FR_MCH_SessionFile_Path | String | AVIVA MCH session file path | Robot | MCH.Library |
| A-18 | 029_FR_MCH_Region | String | MCH region identifier | Robot | MCH.Library, Dispatchers |
| A-19 | 029_FR_Posting_TestFlag | String | Posting mode: TEST or PROD | Robot | Posting.Library |
| A-20 | 029_FR_RDR_CloudClientAddress | String | RDR WCF/REST endpoint URL | Robot | Accounting_WS.Library |
| A-21 | 029_FR_RDR_Flag | String | Include RDR extraction (Y/N) | Robot | DataExtraction |
| A-22 | 029_FR_RPASupport_L1_EmailID | String | RPA L1 support team email | Robot | All alert-sending processes |
| A-23 | 029_FR_SLA_Threshold | String | SLA threshold in minutes | Robot | SLAMonitor |
| A-24 | 029_FR_SQL_BatchSize | String | SQL query batch size | Robot | Database.Library |
| A-25 | 029_FR_UMAS_FilePath | String | H drive path for UMAS DLL | Robot | Posting.Library |
| A-26 | 029_FR_CyberArk_Safe | String | CyberArk safe name for FR credentials | Robot | LoginAgent.Library |
| A-27 | 029_FR_CyberArk_Object_MCH | String | CyberArk account object for MCH | Robot | LoginAgent.Library |
| A-28 | 029_FR_CyberArk_Object_RDR | String | CyberArk account object for RDR | Robot | LoginAgent.Library |

---

## 6. Exception Handling Matrix

| # | Exception Type | Source Workflow | Cause | Handling Action | Retry | Notification |
|---|---|---|---|---|---|---|
| EX-01 | BusinessException | ValidateScheduler | Missing/invalid scheduler fields | Log + email RPA L1 + skip batch | N | Yes – L1 Email |
| EX-02 | BusinessException | GetFundList | Fund list file not found or corrupt | Log + mark queue item Business Exception | N | Yes – L1 Email |
| EX-03 | BusinessException | ExtractMCHData | Fund code not found in MCH | Log + continue with remaining funds | N | Log only |
| EX-04 | BusinessException | ExecutePosting | MCH rejects posting (validation error) | Log + mark queue item Business Exception | N | Yes – Business + L1 Email |
| EX-05 | SystemException | MCH_Login | MCH terminal unreachable / hung | Retry 3x with 60s delay; kill MCH instance between retries | Y (3x) | Alert on 3rd failure |
| EX-06 | SystemException | Bloomberg_HTTP_WS_Extract | Bloomberg endpoint timeout | Retry 2x with 30s delay | Y (2x) | Alert on 2nd failure |
| EX-07 | SystemException | RDR_WS_DataExtraction | RDR cloud client DLL exception | Retry 2x; if persistent, continue without RDR data and flag | Y (2x) | Alert on 2nd failure |
| EX-08 | SystemException | DB_ExecuteQuery | SQL connection lost | Retry 3x with 30s delay | Y (3x) | Alert on 3rd failure |
| EX-09 | SystemException | MapNetworkDrive | Drive mapping failed | Retry 2x then use UNC path fallback | Y (2x) | Alert on 2nd failure |
| EX-10 | SystemException | Any workflow | Unhandled .NET exception | Global Try/Catch → log full stack trace → retry | Y (framework) | Alert Level 1 |
| EX-11 | ApplicationException | MCH_PYDN/PYUP/PYDA | MCH screen timeout mid-posting | Screenshot; kill MCH; raise SystemException for retry | Y (3x) | Alert Level 1 |
| EX-12 | BusinessException | ValidatePostingData | Posting data column missing | Log details + mark queue item Business Exception | N | Yes – L1 email |

---

## 7. Logging Design

**Log Fields (all logs):**

| Field | Source | Example |
|---|---|---|
| ProcessName | Config | `029_FR_Performer_DataExtraction` |
| RobotName | SSC.Custom.Environment.GetMachineName | `BOT-GIO-029-01` |
| UserName | Reusable/Common/GetUserName | `SVC_GIO_029` |
| JobID | Orchestrator context | UUID |
| Timestamp | UTC | `2026-04-08T18:00:00Z` |
| QueueItemReference | Queue item | `FR_20260408_APAC_001` |
| TransactionStatus | Process result | `Completed`, `BusinessException`, `SystemException` |
| ErrorMessage | Exception | `MCH login timeout on retry 2 of 3` |
| ErrorType | Exception class | `TimeoutException` |
| Region | Queue item | `APAC` |

**Log Levels:**

| Level | When Used | Library |
|---|---|---|
| Information | Normal flow milestones | SSC.Platform.Logging.WriteProcessLogs |
| Warning | Retries, partial data, near-threshold | SSC.Platform.Logging.WriteProcessLogs |
| Error | Exceptions, failures, alerts | SSC.Platform.Logging.WriteProcessLogs |
| Trace | Debug mode only (disabled in prod) | UiPath standard Log Message activity |

**PII Exclusion Rules:**
- Fund codes, account numbers: included as reference identifiers (business-approved)
- Credentials, passwords, API keys: **never logged**
- Personal names: not applicable to this automation
- File paths: included for audit (do not contain PII)

---

## 8. Selector Strategy

| Scenario | Strategy | Notes |
|---|---|---|
| MCH Terminal (AVIVA) | Dynamic selectors with screen name + field position | Use `aaname` or `idx` for stable field identification; validate screen name before action |
| MCH Screen Navigation | Text-based screen code matching (BGMM, BGIR, etc.) | Verify screen title text after every navigation |
| Excel files | Excel Application Scope + column name binding | Never use column index; always reference by header name |
| HTTP WS (Bloomberg) | UiPath Web Activities HTTP Request activity | No selectors needed — pure REST call |
| RDR Cloud Client | DLL invocation via Invoke Method / VB.Net code | No UI selectors — COM/DLL interface |
| UMAS DLL | Invoke Method on UMAS COM object | Load DLL from GIO_FR_UMAS_FILE_PATH asset |
| Email (Outlook/SMTP) | UiPath Mail Activities | No selectors — SMTP activity |

**Dynamic Selector Pattern (MCH):**

```xml
<html app='aviva.exe' />
<ctrl name='{{MCHScreenCode}}' role='client' />
<ctrl idx='{{FieldIndex}}' focus='true' />
```

**Fallback Strategy:** If primary selector fails — wait 3 seconds + retry selector once; if still fails → screenshot + raise SystemException.

---

## 9. Config.xlsx Design

### Sheet: Settings

| Name | Value | Description |
|---|---|---|
| OrchestratorQueueName_FundList | 029_FR_FundListFile_Queue | Fund list queue name |
| OrchestratorQueueName_Accounting | 029_FR_AccountingDataFile_Queue | Accounting data queue name |
| MaxRetryNumber | 3 | REFramework retry count |
| MaxConsecutiveSystemExceptions | 5 | Stop robot after N consecutive system exceptions |
| OrchestratorFolderPath | SSC/GIO/029_Factor_Recon | Orchestrator folder for queue/asset access |
| LogLevel | Information | Logging verbosity (Debug for test, Information for prod) |
| PostingTestMode | False | Override asset 029_FR_Posting_TestFlag for local testing |

### Sheet: Constants

| Name | Value | Description |
|---|---|---|
| ProcessName | 029_COB_Factor_Recon | Process identifier in logs |
| RegionList | APAC,EMEA,USIS,IIS | All supported region codes |
| LogFileNamePattern | FR_{yyyyMMdd}_log.xlsx | Daily log file naming |
| ExcelReadTimeout | 120 | Seconds to wait for Excel file open |
| MCHLoginTimeout | 60 | Seconds to wait for MCH login confirmation |
| BloombergHTTPTimeout | 30 | Bloomberg REST call timeout (seconds) |
| RDRCallTimeout | 60 | RDR cloud client timeout (seconds) |
| SQLCommandTimeout | 120 | SQL query timeout (seconds) |
| MaxFundListBatchSize | 100 | Max funds per MCH extraction batch |
| PostingRetryDelay | 30 | Seconds between posting retry attempts |
| SentinelAlertSubject | [BOAT.029] Factor Recon Sentinel Alert | Email subject prefix for sentinel alerts |
| SLAAlertSubject | [BOAT.029] SLA Threshold Alert | SLA email subject prefix |

### Sheet: Assets (all mapped to Orchestrator Asset names)

| Name | Asset Name | Type |
|---|---|---|
| MCH_Credentials | 029_FR_MCH_Credentials | Credential |
| RDR_Credentials | 029_FR_RDR_Credentials | Credential |
| Bloomberg_Credentials | 029_FR_Bloomberg_Credentials | Credential |
| MasterSched_FilePath | 029_FR_MasterSched_FilePath | String |
| Bloomberg_BaseURL | 029_FR_Bloomberg_BaseURL | String |
| RDR_CloudClientAddress | 029_FR_RDR_CloudClientAddress | String |
| UMAS_FilePath | 029_FR_UMAS_FilePath | String |
| CloudClient_FilePath | 029_FR_CloudClient_FilePath | String |
| FundListReport_Folder | 029_FR_FundListReport_Folder | String |
| MCH_SessionFile_Path | 029_FR_MCH_SessionFile_Path | String |
| MCH_Region | 029_FR_MCH_Region | String |
| Business_EmailID | 029_FR_Business_EmailID | String |
| CC_EmailIDs | 029_FR_CC_EmailIDs | String |
| RPASupport_L1_EmailID | 029_FR_RPASupport_L1_EmailID | String |
| SLA_Threshold | 029_FR_SLA_Threshold | String |
| Posting_TestFlag | 029_FR_Posting_TestFlag | String |
| RDR_Flag | 029_FR_RDR_Flag | String |
| Batch_Flag | 029_FR_Batch_Flag | String |
| Batch_Size | 029_FR_Batch_Size | String |
| BulkCopy_BatchSize | 029_FR_BulkCopy_BatchSize | String |
| Delete_Count | 029_FR_Delete_Count | String |
| Loop_Count | 029_FR_Loop_Count | String |
| SQL_BatchSize | 029_FR_SQL_BatchSize | String |
| FundListCheck_Flag | 029_FR_FundListCheck_Flag | String |
| AnalyticalLog_Flag | 029_FR_AnalyticalLog_Flag | String |
| CyberArk_Safe | 029_FR_CyberArk_Safe | String |
| CyberArk_Object_MCH | 029_FR_CyberArk_Object_MCH | String |
| CyberArk_Object_RDR | 029_FR_CyberArk_Object_RDR | String |

---

## 10. Naming Conventions

| Artifact | Convention | Example |
|---|---|---|
| Process Name (Orchestrator) | `029_FR_<Type>_<Name>` | `029_FR_Performer_DataExtraction` |
| Library Name | `GIO.FR.<Domain>.Library` | `GIO.FR.MCH.Library` |
| Workflow File | `<Action><Entity>.xaml` (PascalCase) | `ExtractMCHData.xaml` |
| Variable (in/out arg) | `in_<Name>` / `out_<Name>` | `in_FundListFilePath`, `out_AccountingData` |
| Variable (internal) | `<camelCase>` | `fundListDataTable`, `isLoggedIn` |
| Asset Name | `029_FR_<Category>_<Name>` | `029_FR_MCH_Credentials` |
| Queue Name | `029_FR_<Entity>_Queue` | `029_FR_FundListFile_Queue` |
| Config.xlsx Key | `<PascalCase>` | `MaxRetryNumber`, `BusinessEmailID` |
| Log Message | `[<ProcessName>] <Action>: <Result>` | `[DataExtraction] MCH BGMM: Fund FL1234 extracted successfully` |
| Exception Message | `<ExceptionType> in <WorkflowName>: <Details>` | `SystemException in MCH_Login: Timeout after 60s on retry 2/3` |
| Screenshot File | `{ProcessName}_{QueueRef}_{Timestamp}.png` | `DataExtraction_FR_20260408_APAC_001_180001.png` |
