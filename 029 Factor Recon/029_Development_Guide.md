# 029 – DEVELOPMENT GUIDE

| Field | Value |
|---|---|
| Document ID | 029-DEV-001 |
| Project | BOAT.029 – COB FACTOR RECON |
| Source | 029_Plan.md + 029_Technical_Design_Document.md |
| Date | 2026-04-08 |
| Status | Draft v1.0 |

---

**Legend:**
- 🤖 **AGENT** — AI can fully complete (XAML scaffolding, VB.Net code, Config.xlsx rows, documentation)
- 👷 **DEVELOPER** — Requires UiPath Studio (UI selectors, drag-drop, debug, visual wiring, MCH screen capture)
- 🤖+👷 **BOTH** — Agent generates scaffold, Developer imports and wires in Studio

---

## Step 0: Project Setup 🤖+👷
♻️ **Import from 057 – Ben Profiling — 0 effort**

**Purpose:** Create the 029 UiPath project scaffold by cloning the REFramework template from 057.

**Agent Prompt (copy-paste to Agent):**
```
Create a new UiPath Studio project named "029_COB_Factor_Recon".
Copy the Framework/ folder from "057 – Ben Profiling" exactly as-is.
Copy Data/Config.xlsx from "057 – Ben Profiling".
Update Config.xlsx Settings sheet:
  - OrchestratorQueueName_FundList = 029_FR_FundListFile_Queue
  - OrchestratorQueueName_Accounting = 029_FR_AccountingDataFile_Queue
  - MaxRetryNumber = 3
  - MaxConsecutiveSystemExceptions = 5
Update Config.xlsx Assets sheet with all 28 asset rows from 029_Technical_Design_Document.md Section 9.
Generate project.json with UiPath version 2023.10.7.
```

**Developer Actions:**
1. Open UiPath Studio → New Project → Windows → name "029_COB_Factor_Recon"
2. Copy Framework/ folder from 057 project into new project root
3. Copy Data/Config.xlsx; update rows per Agent output
4. Verify Main.xaml references Framework/Process.xaml correctly

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| T0-01 | Run Main.xaml with empty queue | Process starts, InitAllSettings completes, no exception | |
| T0-02 | Config.xlsx loads all 28 asset keys | Config dictionary populated; log shows asset count | |
| T0-03 | Queue name reads from Config correctly | Log shows correct queue name | |

---

## Step 1: Import Reusable Libraries 🤖+👷
♻️ **Import from 058 – Market Profiling — 0 effort**

**Purpose:** Add the 4 NuGet libraries (Logging, LoginAgent, Environment, Excel.Utilities) and Common/WorkQueue workflows.

**Agent Prompt:**
```
Generate the NuGet dependency entries for the following libraries in project.json:
- SSC.Platform.Logging v1.x.x
- SSC.Platform.LoginAgent v1.x.x
- SSC.Custom.Environment v1.x.x
- BOAT.COB.Excel.Utilities v1.x.x
Also generate the Reusable/Common/ and Reusable/WorkQueue/ folder structures
with all workflow files listed in 029_Technical_Design_Document.md Section 1.1.
```

**Developer Actions:**
1. In UiPath Studio → Manage Packages → add 4 libraries from internal NuGet feed
2. Copy Reusable/Common/ from 057 project (8 files)
3. Copy Reusable/WorkQueue/ from 057 project (9 files)
4. Verify Invoke Workflow arguments resolve without errors

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| T1-01 | Invoke GetCredential.xaml with test asset name | Returns credential object; no exception | |
| T1-02 | Invoke WriteProcessLog.xaml with test message | Log entry appears in Orchestrator/ELK | |
| T1-03 | Invoke SendEmail.xaml with test recipient | Email received; no SMTP exception | |
| T1-04 | Invoke WQ_AddToQueue.xaml with test item | Item appears in Orchestrator queue | |
| T1-05 | Invoke ReadWithOLEDB on a small test Excel file | Returns DataTable with correct rows and columns | |

---

## Step 2: Build GIO.FR.MCH.Library — Login/Session 👷
**Sprint 1 Stories: S1-09**

**Purpose:** MCH terminal automation for Factor Recon. Login, session management, and screen initialization.

**Agent Prompt:**
```
Generate XAML scaffold for GIO.FR.MCH.Library with the following workflows:
1. MCH_StartUp.xaml — Arguments: in_MCHTerminalPath (String), in_MCHSessionFilePath (String), out_MCHInstance (UiPath.Core.Window). Logic: Use Start Process to launch AVIVA terminal; wait for window to appear (timeout from Config); return window handle.
2. MCH_CheckIfLogged.xaml — Arguments: in_MCHWindow (UiPath.Core.Window), out_IsLoggedIn (Boolean). Logic: Check if main MCH menu screen text is visible.
3. MCH_Login.xaml — Arguments: in_MCHWindow (UiPath.Core.Window), in_SubID (String), in_Password (String), out_LoginSuccess (Boolean). Logic: type SubID in SubID field, Tab, type password, Enter, wait for menu screen.
4. MCH_Logout.xaml — Arguments: in_MCHWindow (UiPath.Core.Window). Logic: Send Ctrl+F1 or standard MCH logout key sequence, wait for login screen.
```

**Developer Actions:**
1. Import XAML scaffolds into GIO.FR.MCH.Library project in UiPath Studio
2. Open MCH_StartUp.xaml — wire Start Process activity with actual AVIVA exe path
3. Open MCH_Login.xaml — capture selectors for SubID field, Password field using UiPath Element Spy on AVIVA terminal
4. Set dynamic selector for screen title: `<wnd app='aviva.exe' title='MCH Screen' />`
5. Test login with real MCH test credentials

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| T2-01 | MCH_StartUp with valid terminal path | AVIVA window opens; out_MCHInstance populated | |
| T2-02 | MCH_Login with valid credentials | LoginSuccess = True; MCH main menu visible | |
| T2-03 | MCH_Login with invalid credentials | LoginSuccess = False; no unhandled exception | |
| T2-04 | MCH_CheckIfLogged after login | IsLoggedIn = True | |
| T2-05 | MCH_Logout after login | MCH returns to login screen | |

---

## Step 3: Build GIO.FR.MCH.Library — Screen Workflows 👷
**Sprint 1–2 Stories: S1-10, S2-01, S2-02, S2-03, S2-04**

**Purpose:** Factor Recon-specific MCH screens: BGMM (fund master), BGIR (income report), BSEC (securities), PYDN/PYUP/PYDA (posting), BPOP (batch population).

**Agent Prompt:**
```
Generate XAML scaffolds for the following MCH screen workflows in GIO.FR.MCH.Library.
Each workflow follows this pattern:
1. Navigate to screen (type screen code, Enter)
2. Verify screen loaded (check title text)
3. Read/write fields via Type Into / Get Text activities
4. Return data as output argument

Workflows:
- MCH_BGMM.xaml: in_MCHWindow, in_FundCode (String) → out_BGMMData (DataTable)
- MCH_BGIR.xaml: in_MCHWindow, in_FundCode (String) → out_BGIRData (DataTable)
- MCH_BSEC.xaml: in_MCHWindow, in_SecurityCode (String) → out_BSECData (DataTable)
- MCH_BPOP.xaml: in_MCHWindow, in_BatchData (DataTable) → out_BPOPResult (String)
- MCH_PYDN.xaml: in_MCHWindow, in_PostingData (DataTable), in_IsTestMode (Boolean) → out_PostingResult (String)
- MCH_PYUP.xaml: in_MCHWindow, in_PostingData (DataTable), in_IsTestMode (Boolean) → out_PostingResult (String)  
- MCH_PYDA.xaml: in_MCHWindow, in_PostingData (DataTable), in_IsTestMode (Boolean) → out_PostingResult (String)
- MCH_CleanUp.xaml: in_MCHWindow → close AVIVA terminal process
```

**Developer Actions:**
1. For each screen workflow: open AVIVA on the relevant screen (BGMM, BGIR, etc.) in MCH test environment
2. Use UiPath Element Spy to capture selectors for each input field and output display area
3. Replace placeholder selectors in Agent-generated XAML with real captured selectors
4. Add screen title verification using Check App State + Send Keys for navigation
5. For PYDN/PYUP/PYDA: add IsTestMode branch — if True, do not click final confirm; if False, click confirm
6. For each screen: ensure MCH_BGMM → BGMM screen name navigation works (type ` bgmm` + Enter pattern)

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| T3-01 | MCH_BGMM with valid fund code | BGMMData DataTable populated with fund master fields | |
| T3-02 | MCH_BGMM with invalid fund code | Empty DataTable returned; no exception | |
| T3-03 | MCH_BGIR with valid fund code | BGIRData DataTable populated | |
| T3-04 | MCH_BSEC with valid security code | BSECData populated with security info | |
| T3-05 | MCH_PYDN with IsTestMode=True | No confirmation click; screen shows posting preview | |
| T3-06 | MCH_PYDN with IsTestMode=False on test MCH | Posting submitted; result is "Success" or MCH confirmation code | |
| T3-07 | MCH_PYUP same as T3-06 | Posting submitted successfully | |
| T3-08 | MCH_PYDA same as T3-06 | Posting submitted successfully | |
| T3-09 | MCH_CleanUp | AVIVA process terminated; no orphan processes | |

---

## Step 4: Build GIO.FR.Accounting_WS.Library 🤖+👷
**Sprint 2 Stories: S2-06, S2-07, S2-08, S3-01**

**Purpose:** Extract accounting data from GMM web service, Bloomberg HTTP REST API, and RDR cloud client.

**Agent Prompt:**
```
Generate XAML scaffolds for GIO.FR.Accounting_WS.Library:

1. GMM_WS_DataExtraction.xaml
   Arguments: in_FundCode (String), in_GMMEndpoint (String) → out_AccountingData (DataTable)
   Logic: HTTP GET to in_GMMEndpoint/fund/{in_FundCode}; deserialize JSON response to DataTable. Handle 404 as BusinessException ("Fund not found in GMM"). Handle timeout as SystemException.

2. Group_CUSIPFundData.xaml
   Arguments: in_AccountingData (DataTable) → out_GroupedData (DataTable)
   Logic: LINQ grouping: group by CUSIP field; aggregate numeric columns with Sum.

3. Bloomberg_HTTP_WS_Extract.xaml
   Arguments: in_ISINList (List<String>), in_BaseURL (String), in_Credential (NetworkCredential) → out_BBData (DataTable)
   Logic: Loop through ISINList in batches of 50; HTTP POST to in_BaseURL/api/data with JSON body [{isin: "..."}]; deserialize response; append rows to DataTable. Handle HTTP 429 (rate limit) with 30s wait and retry.

4. RDR_Login.xaml
   Arguments: in_RDRAddress (String), in_Credential (NetworkCredential) → out_RDRSession (Object)
   Logic: Load DLL from CloudClient_FilePath; invoke CreateSession method with credentials; return session object.

5. RDR_WS_DataExtraction.xaml
   Arguments: in_RDRSession (Object), in_FundList (DataTable) → out_RDRData (DataTable)
   Logic: Loop through fund list; invoke GetFactorData method on RDRSession for each fund; aggregate results.

6. MergeBB_LotLevel.xaml
   Arguments: in_BBData (DataTable), in_LotLevelData (DataTable) → out_MergedData (DataTable)
   Logic: Join on ISIN/CUSIP key; include all columns from both tables.

7. Aggregate_Filter.xaml
   Arguments: in_MergedData (DataTable), in_FilterRules (Dictionary<String,String>) → out_FinalData (DataTable)
   Logic: Apply each filter rule as a LINQ where clause; return filtered DataTable.

8. Bloomberg_Webpage.xaml
   Arguments: in_BaseURL (String) → out_PageContent (String)
   Logic: HTTP GET to in_BaseURL; return response body as string; used for connectivity check by Sentinel.

9. CleanUp.xaml
   Arguments: in_RDRSession (Object).
   Logic: Invoke Dispose/Close on RDRSession if not null.
```

**Developer Actions:**
1. Import XAML scaffolds into GIO.FR.Accounting_WS.Library project
2. Add reference to RDR Cloud Client DLL (path from 029_FR_CloudClient_FilePath asset)
3. Test Bloomberg HTTP WS endpoint with a known ISIN in dev environment
4. Confirm RDR DLL method signatures with integration team (method names may differ from BP Object actions)
5. Wire HTTP Request activity for Bloomberg with correct auth headers

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| T4-01 | GMM_WS_DataExtraction with valid fund code | AccountingData DataTable has > 0 rows | |
| T4-02 | GMM_WS_DataExtraction with invalid fund code | BusinessException thrown with "Fund not found" | |
| T4-03 | Bloomberg_HTTP_WS_Extract with 5 known ISINs | BBData has 5 rows; no HTTP exception | |
| T4-04 | RDR_Login with valid credentials | Session object returned; no exception | |
| T4-05 | RDR_WS_DataExtraction with 3 fund codes | RDRData has 3+ rows | |
| T4-06 | MergeBB_LotLevel with matching ISIN/CUSIP | MergedData has correct combined columns | |
| T4-07 | Aggregate_Filter with exclusion filter | Rows matching filter removed | |
| T4-08 | Bloomberg_Webpage connectivity check | Returns HTTP 200; string content non-empty | |

---

## Step 5: Build GIO.FR.MasterScheduler.Library 🤖+👷
**Sprint 1–2 Stories: S1-12, S2-05**

**Purpose:** Read and validate the Master Scheduler Excel file; parse regional holiday dates.

**Agent Prompt:**
```
Generate XAML scaffolds for GIO.FR.MasterScheduler.Library:

1. ReadMasterSchedulerFile.xaml
   Arguments: in_FilePath (String) → out_RawDataTable (DataTable)
   Logic: Use BOAT.COB.Excel.Utilities.ReadWithOLEDB to read first sheet; return DataTable.

2. FieldExistsValidation.xaml
   Arguments: in_DataTable (DataTable), in_RequiredFields (List<String>) → out_MissingFields (List<String>), out_IsValid (Boolean)
   Logic: Check each required field name against DataTable.Columns collection; collect missing fields.

3. ValuesValidation.xaml
   Arguments: in_DataTable (DataTable) → out_ValidRows (DataTable), out_InvalidRows (DataTable), out_ValidationErrors (List<String>)
   Logic: For each row: check non-empty FundCode, valid Region (APAC/EMEA/USIS/IIS), PostingType not empty; segregate valid/invalid.

4. HolidayDateFormat_USIS.xaml
   Arguments: in_DataTable (DataTable) → out_HolidayDates (List<DateTime>)
   Logic: Extract HolidayDate column for USIS rows; parse with format "M/d/yyyy"; return list.

5. HolidayDateFormat_EMEA.xaml / HolidayDateFormat_APAC.xaml / HolidayDateFormat_IIS.xaml
   Same pattern as USIS but with region-specific date formats from BP analysis ("dd-MMM-yyyy" for EMEA, "yyyy/MM/dd" for APAC, "dd/MM/yyyy" for IIS — verify with actual scheduler samples).

6. CleanUp.xaml — dispose any open file handles.
```

**Developer Actions:**
1. Confirm exact column names in real Master Scheduler file with RPA Support L1
2. Confirm date format per region (may differ from scaffold assumptions)
3. Wire ReadWithOLEDB from Excel.Utilities library
4. Test with 3 real scheduler file samples (normal, holiday region, validation error)

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| T5-01 | ReadMasterSchedulerFile with valid file | DataTable returned with rows and columns | |
| T5-02 | FieldExistsValidation with all required fields | IsValid=True; MissingFields empty | |
| T5-03 | FieldExistsValidation with missing FundCode column | IsValid=False; MissingFields contains "FundCode" | |
| T5-04 | ValuesValidation with clean data | All rows in ValidRows; InvalidRows empty | |
| T5-05 | ValuesValidation with blank FundCode row | Row segregated to InvalidRows | |
| T5-06 | HolidayDateFormat_USIS with USIS holiday row | HolidayDates list contains correct DateTime | |
| T5-07 | HolidayCheck blocks correct region on holiday | Filtered out region's items removed from queue batch | |

---

## Step 6: Build 029_FR_Dispatcher_MainScheduler Process 🤖+👷
**Sprint 2 Stories: S2-10**

**Purpose:** Daily scheduled dispatcher – reads scheduler, validates, checks holidays, populates FundListFile queue.

**Agent Prompt:**
```
Generate the following workflows for 029_FR_Dispatcher_MainScheduler:

1. Process/02_Dispatcher_MainScheduler/ReadMasterScheduler.xaml
   Logic: Invoke GIO.FR.MasterScheduler.Library.ReadMasterSchedulerFile with path from Config[MasterSched_FilePath].

2. Process/02_Dispatcher_MainScheduler/ValidateScheduler.xaml
   Logic: Invoke FieldExistsValidation → ValuesValidation; collect all errors.

3. Process/02_Dispatcher_MainScheduler/CheckHolidays.xaml
   Logic: For each region in (APAC, EMEA, USIS, IIS): Get today's date; invoke HolidayDateFormat_{Region}; if today in holiday list, remove that region's rows from DataTable.

4. Process/02_Dispatcher_MainScheduler/SetQueuePriority.xaml
   Logic: Assign Priority field: APAC=1, EMEA=2, USIS=3, IIS=4 (order from BP analysis).

5. Process/02_Dispatcher_MainScheduler/PopulateFundListQueue.xaml
   Logic: For each row in prioritized DataTable: invoke WQ_AddToQueue with queue name 029_FR_FundListFile_Queue; specific content = {FundListFilePath, Region, ProcessingDate, BatchLabel, SchedulerFilePath, PostingType, RDR_Flag}.
```

**Developer Actions:**
1. Wire all invoke workflows in Main.xaml InitState/Process.xaml sequence
2. Wire error path: if validation errors → SendEmail to RPASupport_L1
3. Test with 3 scheduler files (valid, invalid field, holiday day)

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| T6-01 | Valid scheduler, no holidays | Queue populated with all items; email sent with count | |
| T6-02 | Scheduler with invalid column | Queue not populated; error email sent | |
| T6-03 | Today is a holiday for EMEA | EMEA items excluded; all other regions queued | |
| T6-04 | Empty scheduler file | Zero items queued; notification email sent | |

---

## Step 7: Build 029_FR_Performer_DataExtraction Process 🤖+👷
**Sprint 3 Stories: S3-09**

**Purpose:** REFramework performer – dequeues FundListFile items; extracts MCH + Bloomberg + RDR data; populates AccountingDataFile queue.

**Agent Prompt:**
```
Generate workflow scaffolds for 029_FR_Performer_DataExtraction:

1. Process/05_Performer_DataExtraction/GetFundList.xaml
   Arguments: in_FundListFilePath (String) → out_FundListDataTable (DataTable)
   Logic: Invoke BOAT.COB.Excel.Utilities.ReadWithOLEDB on in_FundListFilePath; return DataTable.

2. Process/05_Performer_DataExtraction/ExtractMCHData.xaml
   Arguments: in_FundListDataTable (DataTable), in_MCHWindow (Window), in_BatchSize (Int32) → out_AccountingDataTable (DataTable)
   Logic: Loop through FundList in batches of in_BatchSize; invoke GIO.FR.MCH.Library.MCH_BGMM for each fund; append results to output DataTable.

3. Process/05_Performer_DataExtraction/ExtractBloombergData.xaml
   Arguments: in_AccountingDataTable (DataTable), in_BloombergBaseURL (String), in_BloombergCredential → out_BBDataTable (DataTable)
   Logic: Extract ISIN list from AccountingDataTable; invoke GIO.FR.Accounting_WS.Library.Bloomberg_HTTP_WS_Extract.

4. Process/05_Performer_DataExtraction/ExtractRDRData.xaml
   Arguments: in_FundListDataTable (DataTable), in_RDRAddress (String), in_RDRCredential → out_RDRDataTable (DataTable)
   Logic: RDR_Login; RDR_WS_DataExtraction; CleanUp.

5. Process/05_Performer_DataExtraction/MergeAndAggregateData.xaml
   Arguments: in_AccountingData (DataTable), in_BBData (DataTable), in_RDRData (DataTable) → out_MergedDataTable (DataTable)
   Logic: MergeBB_LotLevel(AccountingData, BBData); then join RDRData on common key if RDRData not empty; Aggregate_Filter.

6. Process/05_Performer_DataExtraction/WriteRawDataReport.xaml
   Arguments: in_MergedDataTable (DataTable), in_OutputFolderPath (String), in_Region (String), in_Date (String) → out_FilePath (String)
   Logic: GIO.FR.Reporting.Library.CreateReport(fileName=ACC_{Region}_{Date}.xlsx); CreateWorksheetAndWriteCollection; SetTitusClassification; return file path.

7. Process/05_Performer_DataExtraction/PopulateAccountingQueue.xaml
   Arguments: in_OutputFilePath (String), in_TransactionItem (QueueItem) → 
   Logic: Invoke WQ_AddToQueue to 029_FR_AccountingDataFile_Queue with AccountingDataFilePath, FundListReference, Region, ProcessingDate, PostingType from parent queue item.
```

**Developer Actions:**
1. Wire all Process.xaml invocations from GetFundList → ExtractMCHData → (conditional) ExtractBloombergData → (conditional RDR_Flag=Y) ExtractRDRData → MergeAndAggregateData → WriteRawDataReport → PopulateAccountingQueue
2. Add MCH_Login call in InitAllApplications.xaml for this process
3. Add RDR_Login conditional on in_RDR_Flag = "Y" check
4. Wire WriteTransactionLog with queue item reference

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| T7-01 | Process one FundListFile queue item (RDR_Flag=N) | MCH data extracted; BB data extracted; merged file written; AccountingDataFile queue populated | |
| T7-02 | Process one item (RDR_Flag=Y) | RDR data also extracted and merged | |
| T7-03 | Fund not found in MCH (invalid fund code) | BusinessException; queue item marked exception; email | |
| T7-04 | MCH login timeout (simulated) | SystemException; retry 3x; after 3 failures alert email | |
| T7-05 | Output file created with Titus classification | File has Titus metadata applied | |
| T7-06 | 50-fund batch: log shows each fund processed | No timeout; all 50 funds extracted without exception | |

---

## Step 8: Build GIO.FR.Posting.Library 🤖+👷
**Sprint 3 Stories: S3-05, S3-06, S3-07, S3-08**

**Purpose:** Full posting lifecycle for PYDN, PYUP, PYDA (standard and UMAS variants).

**Agent Prompt:**
```
Generate XAML scaffolds for GIO.FR.Posting.Library:

1. CreateInstance.xaml — Arguments: in_TemplatePath (String) → out_WorkbookInstance (UiPath.Excel.WorkbookApplication). Open Excel workbook using Excel Application Scope.

2. DeleteSheetContents.xaml — Arguments: in_WorkbookInstance, in_SheetName (String). Logic: Clear worksheet contents; keep headers.

3. CreateSheetAndWriteContents.xaml — Arguments: in_WorkbookInstance, in_SheetName (String), in_DataTable (DataTable). Logic: If sheet not exists, add sheet; Write Range to sheet with headers.

4. Check_PostingType.xaml — Arguments: in_QueueItemData (Dictionary<String,Object>) → out_PostingType (String), out_IsUMAS (Boolean). Logic: Read PostingType from queue item; check if UMAS_FilePath asset is populated → set IsUMAS.

5. Main_Action.xaml — Arguments: in_WorkbookPath (String), in_PostingType (String), in_IsUMAS (Boolean), in_MCHCredential → out_PostingResult (String). Logic: Login to MCH; invoke FR_PYDN/PYUP/PYDA_Posting (or UMAS variant) based on PostingType; MCH_Logout.

6. FR_PYDN_Posting.xaml — Arguments: in_MCHWindow, in_PostingWorkbook, in_IsTestMode → out_PostingResult. Logic: Navigate MCH to PYDN screen (MCH_PYDN); read each row from posting workbook; enter on PYDN screen; verify confirmation.

7. FR_PYUP_Posting.xaml — same pattern as PYDN but for PYUP screen.

8. FR_PYDA_Posting.xaml — same pattern as PYDN but for PYDA screen.

9. FR_PYDN_Posting_UMAS.xaml — Arguments: in_UMASInstance (Object), in_PostingData (DataTable), in_IsTestMode → out_PostingResult. Logic: Invoke UMAS DLL PostPYDN method with posting data collection.

10. FR_PYUP_Posting_UMAS.xaml / FR_PYDA_Posting_UMAS.xaml — same pattern for PYUP/PYDA.

11. CleanUp.xaml — close WorkbookInstance; dispose UMASInstance.
```

**Developer Actions:**
1. Wire MCH selector for PYDN screen navigation in FR_PYDN_Posting (capture real selectors on MCH test)
2. Add UMAS DLL load logic: `New Assembly.LoadFrom(in_UMASFilePath).GetType("UMAS.PostingClient")`
3. Confirm UMAS method signatures with UMAS team
4. Test all 6 posting variants (3 standard × 2 modes: test/real) + IsTestMode branching

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| T8-01 | CreateInstance with valid template | WorkbookInstance open; no exception | |
| T8-02 | CreateSheetAndWriteContents with 10-row DataTable | Sheet created; rows written with headers | |
| T8-03 | Check_PostingType = "PYDN" with UMAS asset populated | PostingType="PYDN"; IsUMAS=True | |
| T8-04 | FR_PYDN_Posting with IsTestMode=True (MCH dev) | MCH PYDN screen shows data; no final confirm | |
| T8-05 | FR_PYDN_Posting with IsTestMode=False (MCH dev) | Posting confirmed; MCH shows success message | |
| T8-06 | FR_PYDN_Posting_UMAS with test data | UMAS DLL returns success code; PostingResult="Success" | |
| T8-07 | FR_PYUP_Posting and FR_PYDA_Posting (same as T8-05) | All three posting types confirmed on MCH | |
| T8-08 | All UMAS variants on test data | All three UMAS postings return success | |

---

## Step 9: Build 029_FR_Performer_TransformPosting 🤖+👷
**Sprint 4 Stories: S4-01**

**Purpose:** REFramework performer – dequeues AccountingDataFile items; transforms data; executes postings; writes lot-level reports.

**Agent Prompt:**
```
Generate workflow scaffolds for 029_FR_Performer_TransformPosting:

1. Process/06_Performer_TransformPosting/LoadAccountingData.xaml
   Logic: ReadWithOLEDB on AccountingDataFilePath from queue item.

2. Process/06_Performer_TransformPosting/TransformData.xaml
   Logic: Invoke GIO.FR.DataTransformation.Library.Filter_Calculate → Set_Column_Data → Change_Column_Position → (if BGIRRequired) BGIR_WS_Call → BGI_Loop.

3. Process/06_Performer_TransformPosting/ValidatePostingData.xaml
   Logic: Check all required columns for posting exist (PostingDate, FundCode, PostingType, Amount, etc.); raise BusinessException if any missing.

4. Process/06_Performer_TransformPosting/ExecutePosting.xaml
   Logic: GIO.FR.Posting.Library.CreateInstance → CreateSheetAndWriteContents → Check_PostingType → Main_Action → CleanUp.

5. Process/06_Performer_TransformPosting/WriteLotLevelReport.xaml
   Logic: GIO.FR.Reporting.Library.WriteDailyReport → SetTitusClassification → SendEmail with report to Business_EmailID.
```

**Developer Actions:**
1. Wire full REFramework flow in 029_FR_Performer_TransformPosting Main.xaml
2. Add MCH Login to InitAllApplications
3. Wire SetTransactionStatus to queue item status after each posting

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| T9-01 | Process one AccountingDataFile queue item (PYDN only) | Data transformed; PYDN posted; MCH confirmed; lot-level report created | |
| T9-02 | Process item with all 3 posting types | PYDN + PYUP + PYDA all posted; one lot-level report | |
| T9-03 | Invalid column in accounting data | BusinessException; queue item marked; email sent | |
| T9-04 | UMAS posting when asset populated | UMAS variant used; DLL success | |
| T9-05 | Lot-level report emailed to correct address | Business email receives attachment | |
| T9-06 | MCH posting timeout (simulated) | SystemException retry 3x; alert on failure | |

---

## Step 10: Build Sentinel, SLA Monitor, Adhoc/NOVAL Dispatchers, DB Cleanup 🤖+👷
**Sprint 2–4 Stories: S2-10, S4-02, S4-03, S4-04**

**Purpose:** Supporting processes for monitoring, ad-hoc operations, and database maintenance.

**Agent Prompt (Sentinel):**
```
Generate 029_FR_Sentinel workflows:
Process/01_Sentinel/MapNetworkDrive.xaml — Arguments: in_SharedDrivePath (String). Logic: Invoke cmd /c "net use Z: \\server\share"; wait; verify mapped. Retry once if failed.

Process/01_Sentinel/CheckMCHConnectivity.xaml — Logic: MCH_StartUp → MCH_Login with stored credential → MCH_CheckIfLogged → if IsLoggedIn: log success, MCH_Logout, return True; else return False.

Process/01_Sentinel/CheckRDRConnectivity.xaml — Logic: RDR_Login with RDR credential → if no exception: CleanUp, return True; else return False.

Process/01_Sentinel/CheckBloombergConnectivity.xaml — Logic: HTTP GET to Bloomberg_BaseURL/health; if HTTP 200 return True; else return False.

Process/01_Sentinel/GenerateStatusReport.xaml — Logic: Create simple text file with timestamp, check results, machine name; write to FundListReport_Folder.
```

**Developer Actions:**
1. Wire all Sentinel checks in sequential flow; aggregate results
2. Send consolidated alert email if any check fails
3. Test each check with simulated failure (wrong URL, wrong credential, DLL not found)

**Test Cases:**

| # | Test | Expected | Pass/Fail |
|---|---|---|---|
| T10-01 | All systems healthy | Sentinel completes with all-pass status report; no alert email | |
| T10-02 | MCH terminal unreachable | MCH check fails; alert email sent; other checks continue | |
| T10-03 | Bloomberg URL wrong | Bloomberg check fails; alert sent | |
| T10-04 | SLA Monitor — queue item pending 120 min, threshold 90 min | Alert email sent with item details | |
| T10-05 | Adhoc Dispatcher with startup parameter path | Queue populated from provided path file | |
| T10-06 | NOVAL Dispatcher | Queue populated without validation errors; holiday check applied | |
| T10-07 | DB Cleanup with Delete_Count = 100 | 100 records deleted per batch; counts logged; email sent | |

---

## Step 11: Integration, UAT, and Deployment 🤖+👷
**Sprint 4 Stories: S4-05 through S4-09**

**Developer Actions:**
1. Run full end-to-end flow: MainScheduler Dispatcher → DataExtraction → TransformPosting → WriteLotLevel
2. Validate queue items transition correctly through both queues
3. Confirm Orchestrator job histories show green
4. Engage business team to validate 3 samples against BP output
5. Publish NuGet packages for all 7 new libraries with version 1.0.0
6. Publish 8 processes to Orchestrator
7. Configure production schedules with LEAD approval

**Deployment Checklist:**
- [ ] 7 library NuPkg files on internal feed
- [ ] 8 process packages deployed to Orchestrator prod
- [ ] 28 assets configured in prod Orchestrator
- [ ] 2 queues configured in prod Orchestrator
- [ ] CyberArk safe registered for robot
- [ ] Sentinel smoke test passes in prod
- [ ] Business parallel run sign-off obtained
- [ ] RPA Support L1 walkthrough complete
