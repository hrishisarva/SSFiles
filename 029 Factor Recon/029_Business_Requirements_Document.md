# 029 – BUSINESS REQUIREMENTS DOCUMENT

| Field | Value |
|---|---|
| Document ID | 029-BRD-001 |
| Project | BOAT.029 – COB FACTOR RECON |
| Source | BP7.3_029_Factor_Recon_Full_Package_Release_May28_2025_001.bprelease |
| Date | 2026-04-08 |
| Status | Draft v1.0 |

---

## 1. Executive Summary

BOAT.029 – COB Factor Recon is a Close-of-Business (COB) automation that reconciles investment factor data across State Street's Global Income Operations (GIO) business unit. The automation reads a daily Master Scheduler file to determine which fund lists require processing, validates the data against regional holiday calendars, populates work queues, extracts accounting data from the MCH mainframe and external data sources (Bloomberg HTTP Web Service, RDR), performs multi-step data transformations, and posts validated results back to MCH across multiple posting types (PYDN, PYUP, PYDA). Lot-level reports are generated and distributed to business stakeholders by email.

This project migrates the existing Blue Prism 7.3 implementation (47 components: 9 processes, 8 objects, 2 work queues, 23 environment variables, 3 credentials, 2 schedules) to UiPath REFramework using UiPath Studio 2023.10.7 and UiPath Orchestrator 2023.10.4. The migration targets functional parity, improved maintainability, and alignment with the SSC RPA Center of Excellence standards (REFramework, Orchestrator assets, CyberArk credential management, structured logging).

**Business Purpose:** Daily reconciliation of investment factor data ensures regulatory compliance, accurate NAV computation, and correct fund accounting across multiple regions (APAC, EMEA, USIS, IIS).

**Scope Summary:** 9 BP Processes → 7 UiPath Processes; 8 BP Objects → 6 UiPath Libraries; 2 BP Queues → 2 Orchestrator Queues; 22 BP Environment Variables → Orchestrator Assets; 3 credential types → CyberArk-managed Orchestrator Assets.

---

## 2. Project Context

### 2.1 Business Driver

The GIO Factor Recon process runs daily post-market close. It ensures fund-level accounting factors (prices, lots, dividends) are accurately extracted from MCH, validated against Bloomberg/RDR external sources, transformed to posting format, and posted back to MCH for downstream consumers. Failure or delay in this process impacts NAV calculation, fund reporting, and regulatory submissions.

### 2.2 Stakeholders

| Role | Name / Group | Involvement |
|---|---|---|
| Business Owner | GIO – Income COE | Process sponsor, UAT, go-live approval |
| RPA Support L1 | RPA Support Team | Monitoring, first-line triage, ad-hoc queue management |
| Platform Engineering | SSC CoE | UiPath architecture, Orchestrator, CyberArk |
| Development Team | DEV-1, DEV-2, LEAD | Build, test, deploy |

### 2.3 Current State (Blue Prism 7.3)

- 9 standalone BP Processes scheduled via BP Scheduler
- 8 BP Objects providing MCH, Bloomberg, RDR, Excel, DB, and reporting functionality
- 2 BP Work Queues: FundListFile (dispatcher input) and AccountingDataFile (performer output/input)
- 22 BP Environment Variables for all runtime configuration
- Credentials stored in BP Credential Manager (fed from CyberArk)
- Logging to ELK/Splunk via analytical log environment variable
- Holiday checks embedded inline in populate-queue processes

### 2.4 Target State (UiPath 2023.10.7)

- 7 UiPath REFramework-based processes in UiPath Orchestrator
- 6 UiPath Libraries with reusable workflows
- 2 Orchestrator Queues with defined schema
- All configuration in Config.xlsx + Orchestrator Assets (String, Credential types)
- Credentials in CyberArk accessed via SSC.Platform.LoginAgent library
- Structured logging via SSC.Platform.Logging to enterprise log platform
- REFramework retry, exception handling, and alert patterns throughout

---

## 3. Component Inventory

### 3.1 Blue Prism Processes

| # | Process Name | Subsheet Count | Purpose | Complexity | UiPath Equivalent |
|---|---|---|---|---|---|
| P-01 | GIO.FR.Adhoc_PopulateQueue.Process | 6 | Ad-hoc population of FundListFile queue from manually-provided scheduler path | Medium | 029_FR_Dispatcher_Adhoc |
| P-02 | GIO.FR.Data_Extraction.Process | 13 | Extract accounting data from MCH (GMM) and Bloomberg HTTP WS; generate Temp RAW Data report | High | 029_FR_Performer_DataExtraction |
| P-03 | GIO.FR.Database_Cleanup.Process | 3 | Clean up database tables in configurable batch sizes to reduce DB load | Low | 029_FR_Utility_DatabaseCleanup |
| P-04 | GIO.FR.MainScheduler_PopulateQueue.Process | 11 | Daily main scheduler population with regional holiday validation (APAC, EMEA, USIS, IIS) | High | 029_FR_Dispatcher_MainScheduler |
| P-05 | GIO.FR.NOVAL_PopulateQueue.Process | 11 | Ad-hoc queue population bypassing validation (backup for corrupt scheduler) | Medium | 029_FR_Dispatcher_NOVAL |
| P-06 | GIO.FR.Sentinel.Process | 12 | Pre-requisite checks, drive mapping, credential validation, MCH/Bloomberg health monitoring | High | 029_FR_Sentinel |
| P-07 | GIO.FR.SLA_Notification.Process | 7 | Monitor queue processing SLA; send email alerts when threshold is approaching | Medium | 029_FR_SLAMonitor |
| P-08 | GIO.FR.Transform_Posting.Process | 9 | Transform extracted data and post to MCH via PYDN/PYUP/PYDA posting screens | Very High | 029_FR_Performer_TransformPosting |
| P-09 | GIO.FR.WriteLotLevelData.Process | 2 | Export lot-level data from DB to Excel report and notify stakeholders | Low | 029_FR_Utility_WriteLotLevel |

**Process Subsheet Detail:**

| Process | Subsheet Name | Type | Purpose |
|---|---|---|---|
| P-01 Adhoc_PopulateQueue | Master Scheduler Read & Validations | Normal | Read scheduler file and validate contents |
| P-01 | Set Priority | Normal | Calculate and assign queue item priority |
| P-01 | Populate Queue | Normal | Add validated fund items to FundListFile Queue |
| P-01 | Report Error Data | Normal | Collect validation errors for reporting |
| P-01 | SEND MAIL | Normal | Send error notification email |
| P-01 | Send Mail | Normal | Send summary notification email |
| P-02 Data_Extraction | DB Connection | Normal | Establish SQL database connection |
| P-02 | Initialize Collection | Normal | Initialize in-memory data collection for processing |
| P-02 | Get Tag Information | Normal | Retrieve queue item tags from Orchestrator |
| P-02 | Get Fundlist Collection | Normal | Load fund list from queue item data |
| P-02 | Get Fundlist Collection - OLEDB | Normal | Load fund list via OLEDB for large datasets |
| P-02 | Excel Operations | Normal | Open/manipulate Excel temp report files |
| P-02 | Check Result | Normal | Validate extraction result and decide next action |
| P-02 | Get SubID and Passwd | Normal | Retrieve SubID and password from CyberArk |
| P-02 | Write Process Logs to ELK | Normal | Write process-level logs to Splunk/ELK |
| P-02 | Write Transaction Logs to ELK | Normal | Write transaction-level logs to Splunk/ELK |
| P-02 | Send Email | Normal | Send extraction summary or error notifications |
| P-02 | Copy Log Template | Normal | Copy logging template to working directory |
| P-02 | Create Log File | Normal | Initialize daily log file |
| P-03 Database_Cleanup | DB Connection | Normal | Establish database connection |
| P-03 | Send Email | Normal | Send cleanup completion notification |
| P-03 | Get Count | Normal | Count records eligible for deletion |
| P-04 MainScheduler_PopulateQueue | Master Scheduler Read & Validations | Normal | Read and validate master scheduler Excel |
| P-04 | Set Priority | Normal | Assign queue item priorities |
| P-04 | Populate Queue | Normal | Populate FundListFile Queue with validated items |
| P-04 | Holiday Check | Normal | Orchestrate regional holiday validation |
| P-04 | APAC Holiday | Normal | Asian-Pacific regional holiday check |
| P-04 | EMEA Holiday | Normal | Europe/Middle East/Africa holiday check |
| P-04 | USIS Holiday | Normal | US Institutional Services holiday check |
| P-04 | IIS Holiday | Normal | International Investor Services holiday check |
| P-04 | Remove Holiday Region | Normal | Exclude holiday regions from queue population |
| P-04 | Report Error Data | Normal | Collect errors for reporting |
| P-04 | Send Mail | Normal | Send population completion email |
| P-05 NOVAL_PopulateQueue | Master Scheduler Read | Normal | Read scheduler file (no validation) |
| P-05 | Populate Queue | Normal | Directly populate queue bypassing validation |
| P-05 | Holiday Check | Normal | Regional holiday orchestration |
| P-05 | APAC Holiday | Normal | APAC holiday check |
| P-05 | EMEA Holiday | Normal | EMEA holiday check |
| P-05 | USIS Holiday | Normal | USIS holiday check |
| P-05 | IIS Holiday | Normal | IIS holiday check |
| P-05 | Remove Holiday Region | Normal | Remove holiday regions |
| P-05 | Report Error Data | Normal | Error collection |
| P-05 | SEND MAIL | Normal | Send email notification |
| P-05 | Send Mail | Normal | Alternate send mail step |
| P-06 Sentinel | FR Pre Requisite Checks | Normal | Validate all prerequisites (drives, files, services) |
| P-06 | Map Network Drive | Normal | Map required shared drives for the session |
| P-06 | Get Credentials | Normal | Retrieve MCH/RDR credentials before login |
| P-06 | Login MCH Application | Normal | Launch and authenticate to MCH terminal |
| P-06 | MCH Screen Check | Normal | Verify MCH is accessible and correct screen |
| P-06 | Generate_Status Report | Normal | Generate current status report for monitoring |
| P-06 | RDR Check | Normal | Validate RDR web service connectivity |
| P-06 | Bloomberg Webpage Check | Normal | Validate Bloomberg HTTP WS accessibility |
| P-06 | SEND MAIL | Normal | Send alert emails |
| P-06 | KILL PROCESS | Normal | Terminate hung processes |
| P-06 | Delete File | Normal | Clean up temporary files |
| P-06 | Check read and Write access | Normal | Test network drive permissions |
| P-07 SLA_Notification | Main SLA Process | Normal | Orchestrate SLA monitoring loop |
| P-07 | Get Pending Items SLA | Normal | Query queue for pending items exceeding SLA |
| P-07 | Mail Send for Stuck | Normal | Send email for stuck/overdue queue items |
| P-07 | Send Mail | Normal | Generic email sending step |
| P-07 | Calculate Time In Min | Normal | Calculate elapsed time in minutes |
| P-07 | Calculate Threashold Reached? | Normal | Compare elapsed time vs SLA threshold |
| P-07 | Get Browse or Posting | Normal | Determine if item is in browse or posting state |
| P-08 Transform_Posting | Get SubID and Passwd | Normal | Retrieve credentials from CyberArk |
| P-08 | Extract Data to Collection | Normal | Load transformed data to in-memory collection |
| P-08 | Initialize Collection | Normal | Initialize collections for posting |
| P-08 | Check Result | Normal | Validate transformation result |
| P-08 | Send Email | Normal | Send posting completion or error notification |
| P-08 | Write Process Logs to ELK | Normal | Write process logs |
| P-08 | Write Transaction Logs to ELK | Normal | Write transaction logs |
| P-08 | Copy Log Template | Normal | Copy log template |
| P-08 | Create Log File | Normal | Create daily log file |
| P-09 WriteLotLevelData | DB Connection | Normal | Connect to database |
| P-09 | Send Email | Normal | Send lot-level report completion email |

### 3.2 Blue Prism Objects

| # | Object Name | Action Count | Purpose | Type | UiPath Equivalent Library |
|---|---|---|---|---|---|
| O-01 | GIO.FR.Accounting_WS_Data_Extraction.Object | 11 | GMM and Bloomberg HTTP WS data extraction, RDR WS extraction, CUSIP grouping, aggregation | Project-Specific | GIO.FR.Accounting_WS.Library |
| O-02 | GIO.FR.CreateLotlevelReport.Object | 2 | Create and write daily lot-level Excel report | Project-Specific | GIO.FR.Reporting.Library |
| O-03 | GIO.FR.Data_Transformation.Object | 8 | Filter/calculate transformations, column manipulation, BGI WS calls, Excel write | Project-Specific | GIO.FR.DataTransformation.Library |
| O-04 | GIO.FR.Database_Cleanup.Object | 2 | SQL database record cleanup operations | Project-Specific | GIO.FR.Database.Library |
| O-05 | GIO.FR.MainScheduler_Validation.Object | 9 | Read and validate master scheduler Excel file including holiday date formats per region | Project-Specific | GIO.FR.MasterScheduler.Library |
| O-06 | GIO.FR.MCH.Object | 12 | MCH terminal automation for Factor Recon-specific screens (BGMM, BGIR, BSEC, PYDN, PYUP, PYDA, BPOP) | Project-Specific | GIO.FR.MCH.Library |
| O-07 | GIO.FR.Posting_Main.Object | 17 | Full posting lifecycle: Excel sheet management, MCH login/logoff, posting orchestration for PYDN/PYUP/PYDA including UMAS variants | Project-Specific | GIO.FR.Posting.Library |
| O-08 | GIO.FR.RDR.Object | 3 | RDR web service login and Bloomberg web page navigation | Project-Specific | GIO.FR.Accounting_WS.Library (merged) |
| O-09 | GIO.FR.Reporting.Object | 4 | Create Excel report worksheets, write collections, send distribution email | Project-Specific | GIO.FR.Reporting.Library |

**Object Action Detail:**

| Object | Action | Action Type | Purpose |
|---|---|---|---|
| O-01 | GMM_WS_Data_Extraction | Normal | Extract fund data from GMM web service |
| O-01 | Group_CUSIPFundData | Normal | Group collected data by CUSIP/Fund |
| O-01 | Bloomberg_HTTP_WS_Data_Extraction - ISIN | Normal | Call Bloomberg HTTP WS to get ISIN/price data |
| O-01 | RDR_WS_Data_Extraction | Normal | Extract data from RDR cloud client web service |
| O-01 | MergeBB_LotLevel | Normal | Merge Bloomberg data with lot-level data |
| O-01 | Aggregate_Filter | Normal | Aggregate and filter merged data collection |
| O-01 | Testing / testbb / TestBB2 | Normal | Internal testing actions (exclude from production) |
| O-01 | Clean Up / Clean Up Memory | CleanUp/Normal | Release resources and clear memory |
| O-02 | WriteDailyReport | Normal | Write lot-level extract to Excel worksheet |
| O-02 | Clean Up | CleanUp | Release Excel COM objects |
| O-03 | Filter_Calculate | Normal | Apply factor calculation filters |
| O-03 | Set_Column_Data | Normal | Set/format column values |
| O-03 | Change_Column_Position | Normal | Reorder columns to target schema |
| O-03 | BGIR_WS_Call | Normal | Call BGIR web service for additional data |
| O-03 | BGI_Loop | Normal | Loop through BGI result set |
| O-03 | Write_ToExcel | Normal | Write transformed collection to Excel |
| O-03 | Testing | Normal | Internal test action (exclude from production) |
| O-03 | Clean Up | CleanUp | Release objects |
| O-04 | DB_Clean_Up | Normal | Delete old records from database in batch |
| O-04 | Clean Up | CleanUp | Close DB connections |
| O-05 | Main | Normal | Orchestrate master scheduler validation |
| O-05 | Read_Master_Scheduler_File | Normal | Read scheduler Excel file via OLEDB |
| O-05 | Field_Exists_Validation | Normal | Verify required columns exist in scheduler |
| O-05 | Values_Validation | Normal | Validate data values in scheduler rows |
| O-05 | Holiday_Date_Format_USIS | Normal | Parse/validate USIS holiday date formats |
| O-05 | Holiday_Date_Format_EMEA | Normal | Parse/validate EMEA holiday date formats |
| O-05 | Holiday_Date_Format_APAC | Normal | Parse/validate APAC holiday date formats |
| O-05 | Holiday_Date_Format_IIS | Normal | Parse/validate IIS holiday date formats |
| O-05 | Clean Up | CleanUp | Release file handles |
| O-06 | Start_Up | Normal | Launch and initialize MCH terminal |
| O-06 | Check_If_Logged | Normal | Check if MCH session is active |
| O-06 | MCH_Login | Normal | Authenticate to MCH mainframe |
| O-06 | MCH_Logout | Normal | Gracefully log off from MCH |
| O-06 | BGMM | Normal | Navigate and operate BGMM screen (fund master) |
| O-06 | BGIR | Normal | Navigate and operate BGIR screen (income report) |
| O-06 | BSEC | Normal | Navigate and operate BSEC screen (securities) |
| O-06 | PYDN | Normal | Navigate and operate PYDN posting screen |
| O-06 | PYUP | Normal | Navigate and operate PYUP posting screen |
| O-06 | PYDA | Normal | Navigate and operate PYDA posting screen |
| O-06 | BPOP | Normal | Navigate and operate BPOP screen (batch population) |
| O-06 | Clean Up | CleanUp | Close MCH terminal session |
| O-07 | Create_Instance | Normal | Create new Excel workbook instance |
| O-07 | Delete_Sheet_Contents | Normal | Clear worksheet contents |
| O-07 | Create_Sheet_and_Write_Contents | Normal | Create worksheet and populate with data |
| O-07 | MCH_Login | Normal | Invoke MCH login for posting session |
| O-07 | MCH_Logoff | Normal | Invoke MCH logoff post-posting |
| O-07 | Main_Action | Normal | Orchestrate full posting workflow |
| O-07 | Posting | Normal | Execute generic posting transaction |
| O-07 | Send_Mail | Normal | Send posting completion notification |
| O-07 | Check_Posting_Type | Normal | Determine posting type (PYDN/PYUP/PYDA) |
| O-07 | FR.PYDN_Posting | Normal | Execute PYDN posting to MCH |
| O-07 | FR.PYUP_Posting | Normal | Execute PYUP posting to MCH |
| O-07 | FR.PYDA_Posting | Normal | Execute PYDA posting to MCH |
| O-07 | FR.PYDN_Posting_UMAS | Normal | Execute PYDN posting via UMAS |
| O-07 | FR.PYUP_Posting_UMAS | Normal | Execute PYUP posting via UMAS |
| O-07 | FR.PYDA_Posting_UMAS | Normal | Execute PYDA posting via UMAS |
| O-07 | Clean Up | CleanUp | Close Excel, release COM objects |
| O-08 | RDR_Login | Normal | Login to RDR cloud client |
| O-08 | Bloomberg_Webpage | Normal | Navigate to Bloomberg data web page |
| O-08 | Clean Up | CleanUp | Close RDR and browser sessions |
| O-09 | Create_Report | Normal | Create new Excel report file |
| O-09 | Create_Worksheet_and_Write_Collection | Normal | Create Excel worksheet and write DataTable |
| O-09 | Send_Mail | Normal | Send report distribution email |
| O-09 | Clean Up | CleanUp | Release Excel objects |

### 3.3 Work Queues

| # | Queue Name | BP Queue ID | Purpose | Dispatcher Process | Performer Process | Key Status Codes |
|---|---|---|---|---|---|---|
| Q-01 | GIO.FR.FundListFile Queue | 27 | Holds fund list items parsed from Master Scheduler file; one item per fund/region/date combination | P-01, P-04, P-05 | P-02 (Data Extraction) | New, In Progress, Completed, Exception |
| Q-02 | GIO.FR.AccountingDataFile Queue | 28 | Holds extracted accounting data file references; feeds Transform/Posting performer | P-02 (as dispatcher) | P-08 (Transform Posting) | New, In Progress, Completed, Exception |

**Queue Schema — GIO.FR.FundListFile Queue:**

| Field | Data Type | Description | Example |
|---|---|---|---|
| FundListFilePath | String | Full path to the fund list Excel file | `\\shared\factor-recon\fundlists\FL_20260408.xlsx` |
| Region | String | Processing region code | `APAC`, `EMEA`, `USIS`, `IIS` |
| ProcessingDate | String | Target processing date (dd-MMM-yyyy) | `08-Apr-2026` |
| Priority | Integer | Queue item priority (1-10; 1=highest) | `1` |
| BatchLabel | String | Batch identifier for traceability | `FR_20260408_APAC_001` |
| SchedulerFilePath | String | Source master scheduler file path | `\\shared\scheduler\MasterSched_20260408.xlsx` |
| IsHoliday | String | Holiday exclusion flag (Y/N) | `N` |
| PostingType | String | Required posting types (comma-separated) | `PYDN,PYUP,PYDA` |
| RDR_Flag | String | RDR data required flag (Y/N) | `Y` |

**Queue Schema — GIO.FR.AccountingDataFile Queue:**

| Field | Data Type | Description | Example |
|---|---|---|---|
| AccountingDataFilePath | String | Path to extracted accounting data file | `\\shared\factor-recon\data\ACC_20260408_APAC.xlsx` |
| FundListReference | String | Reference to parent FundListFile queue item | `FR_20260408_APAC_001` |
| Region | String | Processing region | `APAC` |
| ProcessingDate | String | Target processing date | `08-Apr-2026` |
| ExtractionStatus | String | Extraction completion status | `Complete`, `Partial` |
| MCHScreensExtracted | String | Comma-separated list of MCH screens used | `BGMM,BGIR,BSEC` |
| BloombergExtracted | String | Bloomberg data extracted flag (Y/N) | `Y` |
| RDRExtracted | String | RDR data extracted flag (Y/N) | `Y` |
| PostingType | String | Required posting types | `PYDN,PYUP,PYDA` |

### 3.4 Credentials

| # | BP Credential Name | System | Vault Type | UiPath Asset Name | Access Scope | Used By |
|---|---|---|---|---|---|---|
| CR-01 | GIO FR MCH SubID | MCH Mainframe Terminal | CyberArk | `029_FR_MCH_Credentials` | Robot | P-02, P-06, P-08, O-06, O-07 |
| CR-02 | GIO FR RDR Service | RDR Cloud Client | CyberArk | `029_FR_RDR_Credentials` | Robot | P-06, O-01, O-08 |
| CR-03 | GIO FR Bloomberg WS | Bloomberg HTTP Web Service | CyberArk | `029_FR_Bloomberg_Credentials` | Robot (if auth required) | O-01 |

### 3.5 Environment Variables → UiPath Assets

| # | BP Environment Variable | Description | UiPath Asset Name | Asset Type | Notes |
|---|---|---|---|---|---|
| EV-01 | GIO_FR_ANALYTICAL_LOG | Analytical log write to Splunk flag | `029_FR_AnalyticalLog_Flag` | String | Splunk/ELK write toggle |
| EV-02 | GIO_FR_AUTOMATECONFIG_FILE_PATH | Automate config filepath | `029_FR_AutomateConfig_FilePath` | String | Legacy BP config path |
| EV-03 | GIO_FR_BATCH_FLAG | Batch file processing flag | `029_FR_Batch_Flag` | String | Enable/disable batch mode |
| EV-04 | GIO_FR_BATCH_SIZE | Batch size for MCH data extraction and posting | `029_FR_Batch_Size` | String | Integer string (e.g., "100") |
| EV-05 | GIO_FR_Bloomberg_BaseURL | Bloomberg HTTP WS base URL | `029_FR_Bloomberg_BaseURL` | String | REST endpoint base URL |
| EV-06 | GIO_FR_BULK_COPY_BATCH_SIZE | SQL bulk copy batch size | `029_FR_BulkCopy_BatchSize` | String | SQL batch insert size |
| EV-07 | GIO_FR_BUSINESS_EMAILID | Income COE business email address | `029_FR_Business_EmailID` | String | Distribution email address |
| EV-08 | GIO_FR_CCMAILID | CC email IDs for notifications | `029_FR_CC_EmailIDs` | String | Semicolon-separated email list |
| EV-09 | GIO_FR_CLOUDCLIENT_FILE_PATH | RDR Cloud Client DLL file path | `029_FR_CloudClient_FilePath` | String | UNC path to cloud client |
| EV-10 | GIO_FR_DELETE_COUNT | Number of DB rows to delete per run | `029_FR_Delete_Count` | String | Integer string |
| EV-11 | GIO_FR_FundList_Check_FLAG | Flag to manage fund list count check | `029_FR_FundListCheck_Flag` | String | Y/N flag |
| EV-12 | GIO_FR_FUNDLIST_REPORT_FOLDER | Shared drive path for fund list reports | `029_FR_FundListReport_Folder` | String | UNC path |
| EV-13 | GIO_FR_LOOP_COUNT | Number of database loop iterations | `029_FR_Loop_Count` | String | Integer string |
| EV-14 | GIO_FR_MASTERSCHED_FILEPATH | Master Scheduler Excel file path | `029_FR_MasterSched_FilePath` | String | UNC path to scheduler file |
| EV-15 | GIO_FR_MCH SESSION FILE PATH | Path for AVIVA MCH session files | `029_FR_MCH_SessionFile_Path` | String | AVIVA terminal session path |
| EV-16 | GIO_FR_MCH_REGION_SENT | MCH region identifier | `029_FR_MCH_Region` | String | Region code for MCH routing |
| EV-17 | GIO_FR_POSTING_TEST | Posting test flag | `029_FR_Posting_TestFlag` | String | TEST/PROD mode toggle |
| EV-18 | GIO_FR_RDR_CloudClientAddress | RDR web service URL | `029_FR_RDR_CloudClientAddress` | String | RDR WCF/REST endpoint |
| EV-19 | GIO_FR_RDR_FLAG | Bloomberg/RDR data mode flag | `029_FR_RDR_Flag` | String | Y=use RDR, N=use Bloomberg only |
| EV-20 | GIO_FR_RPA-SUPPORT-L1_EMAILID | RPA Support L1 email address | `029_FR_RPASupport_L1_EmailID` | String | RPA team notification email |
| EV-21 | GIO_FR_SLA_Threshold | SLA processing threshold in minutes | `029_FR_SLA_Threshold` | String | Integer string (minutes) |
| EV-22 | GIO_FR_SQL_BATCH_SIZE | SQL query batch size | `029_FR_SQL_BatchSize` | String | Integer string |
| EV-23 | GIO_FR_UMAS_FILE_PATH | H drive path for UMAS DLL | `029_FR_UMAS_FilePath` | String | UMAS posting DLL path |

---

## 4. Business Process Descriptions

### 4.1 Process: Main Scheduler Queue Population (P-04)

**Trigger:** Scheduled daily (time based on COB schedule for each region)
**Input:** Master Scheduler Excel file at `GIO_FR_MASTERSCHED_FILEPATH`
**Business Rules:**
1. Read the master scheduler file and validate all required fields exist
2. Validate values against permitted ranges and formats
3. For each region in the scheduler, check the regional holiday calendar (APAC, EMEA, USIS, IIS)
4. If today is a holiday for a region, remove that region's items from the processing batch
5. Set priority on each queue item based on region or batch type
6. Add validated, non-holiday items to GIO.FR.FundListFile Queue
7. If any errors occur during validation, email the RPA Support L1 team

**Outputs:** Queue items in GIO.FR.FundListFile Queue; error/success notification email
**Exceptions:** Invalid scheduler format → Business Exception (email L1 team, halt); DB connection failure → System Exception (retry 3x, alert)

### 4.2 Process: Ad-Hoc Queue Population (P-01)

**Trigger:** Manual execution by RPA Support L1 team (business emails scheduler path)
**Input:** Ad-hoc Master Scheduler file path (provided as startup parameter)
**Business Rules:**
1. Same validation as P-04 except the file path comes from startup parameter, not default asset
2. Priority assignment same as P-04
3. No holiday check (ad-hoc items always processed regardless of holiday)
**Outputs:** Queue items added to GIO.FR.FundListFile Queue; notification email
**Exceptions:** Invalid file path or format → Business Exception

### 4.3 Process: NOVAL Queue Population (P-05)

**Trigger:** Manual execution (used when master scheduler is corrupt or business bypasses validation)
**Input:** Master Scheduler file path (from asset)
**Business Rules:**
1. Read scheduler file but skip field and value validation (NOVAL = No Validation)
2. Apply regional holiday checks (same as P-04)
3. Populate queue directly with items
**Outputs:** Queue items in GIO.FR.FundListFile Queue; notification email
**Exceptions:** File read failure → System Exception

### 4.4 Process: Data Extraction (P-02)

**Trigger:** Queue item available in GIO.FR.FundListFile Queue
**Input:** FundListFile queue item (file path, region, date, posting type, RDR flag)
**Business Rules:**
1. Retrieve MCH credentials from CyberArk
2. Extract fund list from the referenced Excel file (OLEDB for large files, standard for small)
3. For each fund in the list, call GMM web service to extract accounting data (BGMM screen)
4. If RDR_Flag = Y, call RDR cloud client web service for additional factor data
5. If Bloomberg data required, call Bloomberg HTTP WS API with ISIN codes
6. Merge Bloomberg lot-level data with MCH accounting data
7. Aggregate and filter the merged collection per business rules
8. Generate RAW Data Temp Excel report to shared drive
9. Add the output file path to GIO.FR.AccountingDataFile Queue
10. Write process and transaction logs to ELK/Splunk
**Outputs:** RAW Data Excel file on shared drive; AccountingDataFile Queue item
**Exceptions:** MCH login failure → System Exception retry; Bloomberg/RDR failure → Business Exception (continue without that data source if configured); DB connection failure → System Exception

### 4.5 Process: Transform and Posting (P-08)

**Trigger:** Queue item available in GIO.FR.AccountingDataFile Queue
**Input:** AccountingDataFile queue item (file path, region, date, posting type)
**Business Rules:**
1. Retrieve MCH credentials from CyberArk
2. Load extracted data from the accounting data file into memory
3. Apply factor calculation filters (Filter_Calculate action)
4. Validate and format column data per MCH posting requirements
5. Reorder columns to match MCH posting schema
6. If posting type includes BGIR, call BGIR web service and loop through results (BGI_Loop)
7. Write transformed data to posting Excel template
8. Based on PostingType from queue item:
   - PYDN: Post to MCH PYDN screen (standard posting)
   - PYUP: Post to MCH PYUP screen (update posting)
   - PYDA: Post to MCH PYDA screen (delete/adjust posting)
   - UMAS variants: same postings via UMAS DLL interface
9. Log each posting transaction to ELK/Splunk
10. Write lot-level data to daily report
**Outputs:** Posting completion in MCH; lot-level Excel report; completion email
**Exceptions:** MCH posting failure → retry up to 3x; persistent failure → Business Exception with full audit trail

### 4.6 Process: Sentinel (P-06)

**Trigger:** Scheduled before main COB window (e.g., 30 minutes before Data Extraction)
**Business Rules:**
1. Map required network drives (shared drive for files)
2. Retrieve MCH and RDR credentials
3. Test MCH terminal launch and login to verify session
4. Test RDR web service connectivity
5. Test Bloomberg HTTP endpoint accessibility
6. Check read/write access on shared drive paths
7. Generate status report file
8. If any prerequisite fails, send alert email to RPA Support L1 and business team; halt downstream processes if critical
**Outputs:** Sentinel status report; alert emails on failure
**Exceptions:** Any connectivity failure → System Exception with email alert

### 4.7 Process: SLA Notification (P-07)

**Trigger:** Scheduled every N minutes during COB processing window
**Business Rules:**
1. Query queue for items still pending beyond SLA threshold (from GIO_FR_SLA_Threshold asset)
2. For each overdue item, identify if it is in Browse or Posting state
3. Calculate elapsed time in minutes since item entered queue
4. If threshold reached/exceeded, send alert email to business and L1 teams
**Outputs:** SLA breach notification emails
**Exceptions:** Queue query failure → System Exception

### 4.8 Process: Database Cleanup (P-03)

**Trigger:** Scheduled (weekly or on-demand by RPA Support L1)
**Business Rules:**
1. Connect to SQL database
2. Count records eligible for deletion (based on age/status criteria)
3. Delete in batches (batch size from GIO_FR_DELETE_COUNT asset) to minimize DB load
**Outputs:** Confirmation email with deleted record count
**Exceptions:** DB connection failure → System Exception

### 4.9 Process: Write Lot Level Data (P-09)

**Trigger:** Triggered after Transform_Posting completes for all items in a batch
**Business Rules:**
1. Connect to SQL database
2. Read lot-level data records for the completed processing date
3. Write data to Excel report file
4. Email the report to business stakeholders
**Outputs:** Lot-level Excel report; distribution email
**Exceptions:** DB read failure → System Exception; file write failure → System Exception

---

## 5. External System Dependencies

| # | System | Type | Authentication | Protocol | Notes |
|---|---|---|---|---|---|
| SYS-01 | MCH Mainframe Terminal | Terminal Emulator (AVIVA) | SubID/Password via CyberArk | VB.Net COM / UI Automation | Factor Recon-specific screens: BGMM, BGIR, BSEC, PYDN, PYUP, PYDA, BPOP |
| SYS-02 | Bloomberg HTTP Web Service | REST HTTP API | API Key or passthrough | HTTP/HTTPS | Base URL from GIO_FR_Bloomberg_BaseURL asset; extracts ISIN/price data |
| SYS-03 | RDR Cloud Client | WCF/REST Web Service | Username/Password via CyberArk | WCF or REST | Cloud client DLL at GIO_FR_CLOUDCLIENT_FILE_PATH; address from GIO_FR_RDR_CloudClientAddress |
| SYS-04 | SQL Database (Factor Recon DB) | Relational Database | Windows Authentication or SQL Auth | ADO.NET/OLEDB | Stores extracted accounting data, lot-level records |
| SYS-05 | Master Scheduler Excel | File System | Windows File Share | OLEDB / Excel Interop | UNC path from GIO_FR_MASTERSCHED_FILEPATH |
| SYS-06 | Fund List Excel Files | File System | Windows File Share | OLEDB / Excel Interop | Dynamic paths from queue items |
| SYS-07 | Shared Network Drive | File System | Windows Authentication | UNC / Mapped Drive | Mapped in Sentinel; paths from env var assets |
| SYS-08 | Outlook / SMTP | Email | Windows Authentication | SMTP | Notifications; server from Config.xlsx |
| SYS-09 | UMAS DLL | COM/DLL Interface | Windows credentials | COM Interop | DLL path from GIO_FR_UMAS_FILE_PATH; posting alternative |
| SYS-10 | ELK / Splunk | Log Platform | API Key / network write | HTTP | Analytical logging; toggle via GIO_FR_ANALYTICAL_LOG |
| SYS-11 | UiPath Orchestrator | Orchestration Platform | OAuth2 / Machine Key | HTTPS | Queue management, asset storage, job scheduling |
| SYS-12 | CyberArk | Credential Vault | CyberArk API | HTTPS | Credential retrieval via SSC.Platform.LoginAgent |

---

## 6. Non-Functional Requirements

| Category | Requirement | Target |
|---|---|---|
| **Performance** | End-to-end COB window completion time | Must complete within the COB SLA (configurable via GIO_FR_SLA_Threshold) |
| **Throughput** | Queue item processing rate | Support concurrent processing of multiple regions simultaneously |
| **Reliability** | System exception retry count | Minimum 3 retry attempts per REFramework configuration |
| **Availability** | Unattended robot uptime during COB window | 99.5% during scheduled COB window |
| **Security** | Credential storage | Zero hardcoded credentials; all via CyberArk |
| **Security** | Log sanitization | No PII, passwords, or API keys in any logs |
| **Audit** | Transaction logging | Every queue item logged with start time, end time, status, error reason |
| **Audit** | Process logging | Every process execution logged with robot, machine, job ID |
| **Recoverability** | Exception recovery | Business exceptions handled without abort; system exceptions retry + alert |
| **Maintainability** | Zero hardcoded values | All environment-specific values in Config.xlsx or Orchestrator Assets |
| **Observability** | Orchestrator monitoring | Job status, queue statistics, exception alerts in Orchestrator dashboard |
| **Compliance** | Titus classification | All Excel reports produced by automation classified via SetTitusClassification |

---

## 7. Acceptance Criteria

| # | Criterion | Verification Method |
|---|---|---|
| AC-01 | Main Scheduler queue population produces identical queue items to BP baseline | Side-by-side comparison on 3 consecutive COB runs |
| AC-02 | Ad-hoc and NOVAL population work with manually provided file paths | Manual test with 2 test scenarios each |
| AC-03 | Data Extraction produces same RAW Data Excel files as BP for identical input | File diff of BP vs. UiPath output on 5 fund list samples |
| AC-04 | Bloomberg HTTP WS extraction returns same data volume as BP | Record count comparison on 3 samples |
| AC-05 | RDR extraction (when RDR_Flag=Y) matches BP results | Data comparison on 2 RDR-enabled samples |
| AC-06 | PYDN, PYUP, PYDA postings to MCH accepted without error | MCH screen confirmation captured in logs |
| AC-07 | UMAS posting variants produce same results as standard postings | Comparison on 1 sample per type |
| AC-08 | Sentinel detects prerequisite failures and sends correct alerts | Simulated failure test for each dependency |
| AC-09 | SLA Monitor fires alerts at correct threshold | Test with mock queue items exceeding threshold |
| AC-10 | DB Cleanup deletes correct count of records in batches | Record count validation before/after on test DB |
| AC-11 | All credentials retrieved from CyberArk (zero hardcoded) | Code review + penetration test |
| AC-12 | All logs sanitized (no PII, no secrets) | Log review on 3 test runs |
| AC-13 | REFramework retry logic fires on system exceptions | Simulated system exception test |
| AC-14 | Config.xlsx drives all environment-specific settings | Integration test with modified config values |
| AC-15 | Orchestrator deployment packages deploy and execute without errors | Full deployment test in DEV Orchestrator |

---

## 8. Assumptions

| # | Assumption |
|---|---|
| A-01 | MCH AVIVA terminal version and screen layouts are unchanged from BP 7.3 implementation |
| A-02 | Bloomberg HTTP WS API endpoint, authentication, and schema are unchanged |
| A-03 | RDR Cloud Client DLL version is compatible with UiPath .NET environment |
| A-04 | UMAS DLL is compatible with UiPath .NET environment (same as UMAS_FILE_PATH) |
| A-05 | SQL database schema and connection string remain unchanged |
| A-06 | Master Scheduler Excel file format (column structure) remains unchanged |
| A-07 | UiPath Orchestrator and CyberArk integration is pre-configured by Platform Engineering (SSC CoE) |
| A-08 | All network drives, UNC paths, and shared folder permissions are granted to the robot service account |
| A-09 | Testing environments (MCH dev, DB dev, Bloomberg sandbox) are available during test sprints |
| A-10 | Testing actions in BP Objects (O-01: Testing, testbb, TestBB2; O-03: Testing) are excluded from UiPath build |
| A-11 | The "Create Sheet and Write Contents_Old" action in O-07 is legacy and excluded from UiPath build |
| A-12 | BOAT.COB.MCH.Library from project 058 covers generic MCH login/logoff; Factor Recon-specific screens require a new GIO.FR.MCH.Library |
| A-13 | BOAT.COB.MAP.Library is NOT required for project 029 (no MAP system integration) |
| A-14 | Holiday calendars are embedded in the master scheduler file (not external feeds) |

---

## 9. Risks

| # | Risk | Probability | Impact | Mitigation |
|---|---|---|---|---|
| R-01 | MCH screen layout changes between BP 7.3 and UiPath implementation | Low | High | Capture screenshots of all MCH screens in BP before migration; validate in Sentinel dry run |
| R-02 | Bloomberg HTTP WS endpoint returns different data format or new authentication requirement | Medium | High | Confirm API spec with vendor/team before build; implement flexible JSON parsing |
| R-03 | RDR Cloud Client DLL incompatibility with UiPath .NET runtime | Medium | High | Test DLL in target .NET version during Sprint 1; evaluate REST alternative if incompatible |
| R-04 | UMAS DLL incompatibility with UiPath .NET runtime | Medium | Medium | Test UMAS DLL early in Sprint 2; plan COM interop wrapper if needed |
| R-05 | CyberArk integration not configured for new UiPath robot machine | Low | High | Engage Platform Engineering in Sprint 1 to register robot in CyberArk safe |
| R-06 | Data Extraction volume causes queue processing to exceed COB SLA | Low | High | Benchmark extraction time per fund in Sprint 3 UAT; adjust batch size if needed |
| R-07 | Holiday calendar data format varies across regions causing validation failures | Medium | Medium | Validate all 4 regional formats with real test data in Sprint 2 |
| R-08 | Network drive mapping fails intermittently in Orchestrator unattended robot context | Medium | Medium | Implement retry logic in Sentinel drive-mapping step; use UNC paths as fallback |

---

## 10. Glossary

| Term | Definition |
|---|---|
| BGMM | MCH mainframe screen – Bond/fund General Master Management |
| BGIR | MCH mainframe screen – Bond Global Income Report |
| BSEC | MCH mainframe screen – Security information lookup |
| BPOP | MCH mainframe screen – Batch population |
| PYDN | MCH posting screen – Post-down transaction type |
| PYUP | MCH posting screen – Post-up transaction type |
| PYDA | MCH posting screen – Post-day adjustment transaction type |
| UMAS | Unified Management Access System – alternative posting DLL interface |
| COB | Close of Business – end-of-day processing window |
| RDR | Regulatory Data Repository – cloud client for factor data |
| Bloomberg HTTP WS | Bloomberg data HTTP REST Web Service for price/ISIN data |
| GMM | General Market Management – web service providing fund accounting data |
| GIO | Global Income Operations – State Street business unit |
| FR | Factor Recon – abbreviation for Factor Reconciliation |
| ELK | Elasticsearch-Logstash-Kibana – logging platform (also referred to as Splunk in BP) |
| ISIN | International Securities Identification Number |
| NAV | Net Asset Value – fund valuation metric |
| REFramework | UiPath Robotic Enterprise Framework – standard dispatcher/performer architecture |
| SubID | MCH system user identifier (service account login) |
| OLEDB | Object Linking and Embedding Database – fast Excel read driver |
| SLA | Service Level Agreement – time limit for queue item processing |
| APAC | Asia-Pacific regional processing unit |
| EMEA | Europe, Middle East and Africa regional processing unit |
| USIS | US Institutional Services regional processing unit |
| IIS | International Investor Services regional processing unit |
