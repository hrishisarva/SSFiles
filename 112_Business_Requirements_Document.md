# 112 MDS OCFC — Business Requirements Document

| Field | Value |
|---|---|
| Document ID | 112-BRD-001 |
| Project | MDS.112 – COB MDS |
| Source | BP7.3_112_MDS_OCFC_Full_Package_Release_Sep23_2025_001.bprelease |
| Date | 2026-04-13 |
| Status | Draft v1.0 |
| Blue Prism Version | 7.3 |
| Target UiPath Version | 2023.10.7 |
| Target Orchestrator Version | 2023.10.4 |

---

## 1. Executive Summary

This document captures the business requirements for migrating the **MDS.112 OCFC (Ongoing Corporate Finance Checks)** automation from Blue Prism 7.3 to UiPath 2023.10.7. The MDS OCFC automation performs Master Data Services validation and integrity checks across multiple financial systems — RKS, MCH, MDP, ESP, ECF, ERP, GTM, and others — processing securities data through 26 work queues with an orchestrator-sentinel-worker pattern.

**Business Purpose:** Automate the validation, reconciliation, and correction of securities master data (factors, coupon rates, maturity dates, ISINs, SEDOLs, currency codes, exchange codes, etc.) across internal and external data sources. The automation detects data integrity issues, applies business rules for resolution, and generates notifications/reports for operations teams.

**Scope:** Full conversion of 33 Blue Prism processes, 101 business objects, 26 work queues, 272 environment variables, and all associated credentials from Blue Prism to UiPath REFramework with zero business logic regression.

---

## 2. Project Context

### 2.1 Business Driver

The organization is migrating from Blue Prism to UiPath to:
- Consolidate all RPA automations onto a single platform (UiPath)
- Leverage REFramework for standardized exception handling and queue-based processing
- Improve monitoring and observability via UiPath Orchestrator
- Reduce licensing costs and operational overhead from maintaining two RPA platforms

### 2.2 Stakeholders

| Role | Responsibility |
|---|---|
| Business Owner | Approve business rules, sign-off UAT |
| MDS Operations Team | Provide test data, validate functional parity |
| RPA CoE Lead | Architecture review, deployment approval |
| IT Security | Credential vault migration approval |
| Infrastructure Team | Robot machine provisioning, network access |

### 2.3 Current State (Blue Prism 7.3)

The MDS OCFC automation runs on Blue Prism 7.3 with:
- **33 processes** covering securities data validation check types (factor, coupon, maturity, ISIN, SEDOL, exchange code, currency, etc.)
- **101 business objects** providing system interactions (RKS, MCH, MDP, ESP, ECF, ERP, GTM, HTTP APIs, DB, Mail, utilities)
- **26 work queues** with queue-based orchestration
- **272 environment variables** externalizing all configuration
- **1 Orchestrator process** controlling execution flow with a **Sentinel process** monitoring completion
- CyberArk (Cloakware) credential management across 9 domains

### 2.4 Target State (UiPath 2023.10.7)

- UiPath REFramework-based Dispatcher/Performer model
- UiPath Orchestrator queues replacing BP Work Queues
- UiPath Assets replacing BP Environment Variables
- CyberArk integration via UiPath Credential Store
- Reusable libraries imported from 058 – Market Profiling (MCH, MAP, Logging, LoginAgent, Environment, Excel Utilities)
- Orchestrator-level monitoring, alerts, and job tracking

---

## 3. Component Inventory (Parsed from .bprelease)

### 3.1 Blue Prism Processes (33)

| # | Process Name | Subsheets | Purpose | Complexity |
|---|---|---|---|---|
| 1 | MDS.112.OCFC.Orchestrator | 4 | Master orchestrator – triggers and sequences all check processes | L |
| 2 | MDS.112.OCFC.Sentinel | 35 | Monitors execution status, generates status reports, handles completion | XL |
| 3 | MDS.112.OCFC.Factor Generic Process | 35 | Generic factor validation across multiple check types | XL |
| 4 | MDS.112.OCFC.Null Factor | 33 | Detects and processes null factor records | XL |
| 5 | MDS.112.OCFC.Dupe Sedols Process | 31 | Identifies and resolves duplicate SEDOL codes | XL |
| 6 | MDS.112.OCFC.Validate USER1 Process | 29 | Validates USER1 field integrity across securities | XL |
| 7 | MDS.112.OCFC.Inflation Index Process | 29 | Processes inflation index-linked securities | XL |
| 8 | MDS.112.OCFC.Dupe Client Security | 26 | Detects duplicate client-security mappings | L |
| 9 | MDS.112.OCFC.Dupe ISIN Process | 23 | Identifies duplicate ISIN codes | L |
| 10 | MDS.112.OCFC.ECF Checks Processing | 21 | ECF (External Corporate Finance) data validation | L |
| 11 | MDS.112.OCFC.Coupon Date Integrity Process | 19 | Validates coupon payment date consistency | L |
| 12 | MDS.112.OCFC.Multilisted USER1 Process | 17 | Checks multilisted securities USER1 field | M |
| 13 | MDS.112.OCFC.RKS And DataMarts Validation Process | 16 | Validates RKS data against DataMarts | M |
| 14 | MDS.112.OCFC.Null Security Code Process | 15 | Detects null security codes | M |
| 15 | MDS.112.OCFC.Security Details Null Check | 13 | Validates non-null security detail fields | M |
| 16 | MDS.112.OCFC.Search USER1 Process | 12 | Searches for USER1 field values across systems | M |
| 17 | MDS.112.OCFC.Factor Rate Consistency | 11 | Validates factor rate consistency across sources | M |
| 18 | MDS.112.OCFC.Underlier Security | 11 | Validates underlier security relationships | M |
| 19 | MDS.112.OCFC.Principal Type Name Integrity | 9 | Validates principal type name fields | S |
| 20 | MDS.112.OCFC.SMF Calculation Process | 8 | SMF (Standard Market Factor) calculations | S |
| 21 | MDS.112.OCFC.Convertible Bonds | 0 | Convertible bond validation | S |
| 22 | MDS.112.OCFC.Coupon Rate Integrity | 0 | Validates coupon rate data | S |
| 23 | MDS.112.OCFC.Coupon Type Change | 0 | Detects coupon type changes | S |
| 24 | MDS.112.OCFC.Duplicate Factor | 0 | Detects duplicate factor records | S |
| 25 | MDS.112.OCFC.End Date Validation | 0 | Validates end dates across securities | S |
| 26 | MDS.112.OCFC.Exchange Code | 0 | Validates exchange code fields | S |
| 27 | MDS.112.OCFC.Factor Variance | 0 | Detects factor variance anomalies | S |
| 28 | MDS.112.OCFC.Matured or Expired Positions | 0 | Identifies matured/expired positions | S |
| 29 | MDS.112.OCFC.Maturity Date | 0 | Validates maturity date fields | S |
| 30 | MDS.112.OCFC.Missing Adjustable Coupon Dated Date | 0 | Detects missing adjustable coupon dated dates | S |
| 31 | MDS.112.OCFC.Security Currency Code | 0 | Validates security currency code mappings | S |
| 32 | MDS.112.OCFC.Stale Factor | 0 | Identifies stale factor data | S |
| 33 | MDS.112.OCFC.Status Report | 0 | Generates automation status reports | S |

> **Note:** Processes with 0 subsheets are lightweight processes that delegate to shared objects. Subsheet count includes Main Page.

### 3.2 Blue Prism Objects (101)

#### 3.2.1 Core System Objects

| # | Object Name | Actions | Type | Purpose |
|---|---|---|---|---|
| 1 | MDS.OCFC.RKS Main Object | 24 | Project-Specific | Core RKS system interaction — issue search, navigation, data retrieval |
| 2 | MDS.OCFC.RKS Wrapper | 46 | Project-Specific | High-level RKS operations wrapper — orchestrates RKS sub-objects |
| 3 | MDS.OCFC.RKS Launch HTTP | 6 | Project-Specific | HTTP-based RKS launch mechanism |
| 4 | MDS.OCFC.RKS Launch Edge Mode | 15 | Project-Specific | RKS launch via Edge browser |
| 5 | MDS.OCFC.RKS Launch UIA Mode | 18 | Project-Specific | RKS launch via UIA automation |
| 6 | MDS.OCFC.RKS Issue Search | 9 | Project-Specific | Security issue search in RKS |
| 7 | MDS.OCFC.RKS Update Logic | 15 | Project-Specific | RKS data update operations |
| 8 | MDS.OCFC.MCH | 16 | Shared/Partial | MCH mainframe interaction — login, navigation, screen data retrieval |
| 9 | MDS.OCFC.ESP | 20 | Project-Specific | ESP system integration |
| 10 | MDS.OCFC.ESP Extended Object | 41 | Project-Specific | Extended ESP operations — cross-reference, factor tables, transactions |
| 11 | MDS.OCFC.ERP Web Object | 23 | Project-Specific | ERP web application automation |
| 12 | MDS.OCFC.GTM Web Object | 20 | Project-Specific | GTM web application automation |
| 13 | MDS.OCFC.PSDL Web Object | 23 | Project-Specific | PSDL web application automation |

#### 3.2.2 HTTP/API Objects

| # | Object Name | Actions | Purpose |
|---|---|---|---|
| 14 | MDS.OCFC.ECF HTTP | 19 | ECF API interaction |
| 15 | MDS.OCFC.ERP HTTP | 6 | ERP API interaction |
| 16 | MDS.OCFC.MDP HTTP Data Retriever | 22 | MDP API data retrieval |
| 17 | MDS.OCFC.MDP HTTP Data Retriever Extended Object | 12 | Extended MDP API operations |
| 18 | MDS.OCFC.OCF HTTP | 17 | OCF API interaction |
| 19 | MDS.OCFC.RDOC HTTP | 6 | RDOC API interaction |
| 20 | MDS.OCFC.RMG HTTP | 14 | RMG API interaction |
| 21 | MDS.OCFC.SSCD HTTP | 15 | SSCD API interaction |
| 22 | MDS.OCFC.RKS Launch HTTP | 6 | HTTP-based RKS launch |
| 23 | MDS.OCFC.IMS HUB | 10 | IMS Hub web service integration |

#### 3.2.3 RKS Sub-Objects (Screen-Specific)

| # | Object Name | Actions | Purpose |
|---|---|---|---|
| 24 | MDS.OCFC.RKS Cross Reference | 21 | Cross-reference screen operations |
| 25 | MDS.OCFC.RKS Factor History | 17 | Factor history screen operations |
| 26 | MDS.OCFC.RKS Fixed Income | 15 | Fixed income screen operations |
| 27 | MDS.OCFC.RKS Fixed Income Maturity Date | 11 | Fixed income maturity date screen |
| 28 | MDS.OCFC.RKS Floating Interest | 17 | Floating interest screen operations |
| 29 | MDS.OCFC.RKS Futures Last Trading Date | 11 | Futures last trading date screen |
| 30 | MDS.OCFC.RKS Options Expiration Date | 11 | Options expiration date screen |
| 31 | MDS.OCFC.RKS Rights Expiration Date | 10 | Rights expiration date screen |
| 32 | MDS.OCFC.RKS Warrants Expiration Date | 11 | Warrants expiration date screen |
| 33 | MDS.OCFC.RKS Cash Flow Schedule | 9 | Cash flow schedule screen |
| 34 | MDS.OCFC.RKS Cash Flow Schedule Data | 8 | Cash flow schedule data extraction |
| 35 | MDS.OCFC.RKS Scheduled Principal Payments | 24 | Scheduled principal payments screen |
| 36 | MDS.OCFC.RKS Scheduled Principal Payments Entry | 11 | Principal payment entry screen |
| 37 | MDS.OCFC.RKS Step Table | 7 | Step table screen operations |
| 38 | MDS.OCFC.RKS Adjustable Table | 7 | Adjustable table screen |
| 39 | MDS.OCFC.RKS Base Rate Name | 11 | Base rate name screen |
| 40 | MDS.OCFC.RKS Credit Adjustments | 6 | Credit adjustments screen |
| 41 | MDS.OCFC.RKS Credit Information Status Window | 5 | Credit information status window |
| 42 | MDS.OCFC.RKS Detail Analysis | 7 | Detail analysis screen |
| 43 | MDS.OCFC.RKS Market Data | 5 | Market data screen |
| 44 | MDS.OCFC.RKS Portfolio Browser | 9 | Portfolio browser screen |
| 45 | MDS.OCFC.RKS Schedules | 6 | Schedules screen |
| 46 | MDS.OCFC.RKS Security Paydown | 5 | Security paydown screen |
| 47 | MDS.OCFC.RKS Transaction History | 12 | Transaction history screen |
| 48 | MDS.OCFC.RKS Import External Data | 10 | Import external data screen |
| 49 | MDS.OCFC.RKS Add New Factor Line | 13 | Add new factor line screen |
| 50 | MDS.OCFC.RKS Additional Fixed Income Information | 5 | Additional fixed income info screen |
| 51 | MDS.OCFC.RKS Additional Issue Type Information | 10 | Additional issue type info screen |
| 52 | MDS.OCFC.RKS MDP Validation | 4 | MDP validation operations in RKS |

#### 3.2.4 Business Logic Objects

| # | Object Name | Actions | Purpose |
|---|---|---|---|
| 53 | MDS.OCFC.3019 3334 Logic Wrapper | 15 | 3019/3334 check logic orchestration |
| 54 | MDS.OCFC.3019 3334 Specific Actions | 15 | 3019/3334 specific business actions |
| 55 | MDS.OCFC.3019 3334 Floating Comparison | 20 | Floating rate comparison logic |
| 56 | MDS.OCFC.3019 3334 Adjustable (CORP OR GOVT) Scenario | 2 | Adjustable scenario — CORP/GOVT |
| 57 | MDS.OCFC.3019 3334 Adjustable (MUNI) Scenario | 2 | Adjustable scenario — MUNI |
| 58 | MDS.OCFC.3019 3334 CORP OR GOVT (Adjustable) Comparison | 4 | CORP/GOVT adjustable comparison |
| 59 | MDS.OCFC.3019 3334 CPN TYP Adjustable (MTGE) | 2 | Coupon type adjustable — MTGE |
| 60 | MDS.OCFC.3019 3334 CPN TYP Fixed (MTGE) | 2 | Coupon type fixed — MTGE |
| 61 | MDS.OCFC.3019 3334 CPN TYP Multiple Scenarios | 2 | Coupon type multiple scenarios |
| 62 | MDS.OCFC.3019 3334 Fixed to Float Or Fixed to Variable | 4 | Fixed-to-float/variable logic |
| 63 | MDS.OCFC.3019 3334 Floating or Variable Scenario | 2 | Floating/variable scenario |
| 64 | MDS.OCFC.3019 3334 MTGE (Adjustable) Comparison | 4 | MTGE adjustable comparison |
| 65 | MDS.OCFC.3019 3334 MTGE Scenario | 4 | MTGE scenario logic |
| 66 | MDS.OCFC.3019 3334 MUNI (Adjustable) Comparison | 4 | MUNI adjustable comparison |
| 67 | MDS.OCFC.3019 3334 Not Fixed to Float And Fixed to Variable | 3 | Not-fixed-to-float logic |
| 68 | MDS.OCFC.3019 3334 STEP Scenario | 5 | STEP scenario logic |
| 69 | MDS.OCFC.3077 RKS Update Logic | 12 | 3077 check — RKS update logic |
| 70 | MDS.OCFC.3122 MDP Logic | 10 | 3122 check — MDP logic |
| 71 | MDS.OCFC.3122 Target Date Logic | 9 | 3122 check — target date logic |
| 72 | MDS.OCFC.3122 Transaction Level Logic | 17 | 3122 check — transaction level logic |
| 73 | MDS.OCFC.3122 Transaction Level Logic NEW | 8 | 3122 check — new transaction level logic |
| 74 | MDS.OCFC.3917 MDP Logic | 8 | 3917 check — MDP logic |
| 75 | MDS.OCFC.CPN_TYP Logic | 5 | Coupon type classification logic |
| 76 | MDS.OCFC.MDP Logic | 5 | General MDP logic |
| 77 | MDS.OCFC.MVP2 Generic Logic | 15 | MVP2 generic validation logic |
| 78 | MDS.OCFC.MVP4 3016 Logic Wrapper | 31 | MVP4 3016 check orchestration |
| 79 | MDS.OCFC.MVP4 3329 Logic Wrapper | 25 | MVP4 3329 check orchestration |
| 80 | MDS.OCFC.Principal Type Logic | 13 | Principal type validation logic |
| 81 | MDS.OCFC.Find Underlying Security ID | 9 | Underlying security ID resolution |
| 82 | MDS.OCFC.Get Factor Table | 8 | Factor table retrieval logic |
| 83 | MDS.OCFC.Rounding Object | 4 | Rounding logic for factor/rate values |
| 84 | MDS.OCFC.Dummy Security Logic | 2 | Dummy security handling |
| 85 | MDS.OCFC.ESP MDP Validation | 5 | ESP-MDP cross-validation |
| 86 | MDS.OCFC.Master File Validation | 22 | Master file validation rules |
| 87 | MDS.OCFC.MDL INTRA POS SM Factor Rate Consistency | — | Intraday position factor rate consistency (uses queue) |
| 88 | MDS.OCFC.Sentinel Specific Actions | 6 | Sentinel-specific automation actions |
| 89 | MDS.OCFC.Project Specific Actions | 17 | Cross-cutting project-specific logic |
| 90 | MDS.OCFC.TBA RKS Modification | 4 | TBA (To Be Announced) RKS modification |
| 91 | MDS.OCFC.TBA RKS Orchestrator | 6 | TBA RKS orchestration |
| 92 | MDS.OCFC.OCF Data Source Wrapper | 5 | OCF data source abstraction |

#### 3.2.5 Infrastructure & Utility Objects

| # | Object Name | Actions | Purpose |
|---|---|---|---|
| 93 | MDS.OCFC.DB Interactions | 12 | Database CRUD operations with retry |
| 94 | MDS.OCFC.DB Locking | 6 | Database locking/unlocking mechanism |
| 95 | MDS.OCFC.Mail Wrapper | 11 | Email sending orchestration |
| 96 | MDS.OCFC.Mails | — | Email template and sending |
| 97 | MDS.OCFC.Collective Notification | 6 | Batch notification grouping and sending |
| 98 | MDS.OCFC.Main Process Triggering | 11 | Process triggering and scheduling |
| 99 | MDS.OCFC.IMT Item Finalization | 3 | IMT item finalization |
| 100 | MDS.OCFC.ECF Data Upload | 10 | ECF data upload operations |
| 101 | MDS.OCFC.Webservice Log | 3 | Web service logging |
| 102 | MDS.OCFC.C# Utilities | 16 | C# utility functions |
| 103 | MDS.OCFC.VB utilities | 2 | VB.NET utility functions |
| 104 | MOA.095.DVPC.VLOOKUP Object - EDG2024 | 2 | External VLOOKUP utility (shared) |

#### 3.2.6 Validation & Integrity Objects

| # | Object Name | Actions | Purpose |
|---|---|---|---|
| 105 | MDS.OCFC.Convertible Bonds | — | Convertible bond business rules |
| 106 | MDS.OCFC.Coupon Date Integrity | — | Coupon date integrity validation |
| 107 | MDS.OCFC.Coupon Rate Integrity | — | Coupon rate integrity checks |
| 108 | MDS.OCFC.Coupon Type Change | — | Coupon type change detection |
| 109 | MDS.OCFC.Dupe Client Security | — | Duplicate client security detection |
| 110 | MDS.OCFC.Dupe ISIN | — | Duplicate ISIN detection |
| 111 | MDS.OCFC.Duplicate Factor | — | Duplicate factor detection |
| 112 | MDS.OCFC.ECF Checks | — | ECF check validation |
| 113 | MDS.OCFC.End Date Validation | — | End date validation |
| 114 | MDS.OCFC.Exchange Code | — | Exchange code validation |
| 115 | MDS.OCFC.Factor Rate Consistency | — | Factor rate consistency checks |
| 116 | MDS.OCFC.Factor Variance | — | Factor variance detection |
| 117 | MDS.OCFC.Inflation Index | — | Inflation index processing |
| 118 | MDS.OCFC.Matured or Expired Positions | — | Matured/expired position handling |
| 119 | MDS.OCFC.Maturity Date | — | Maturity date validation |
| 120 | MDS.OCFC.Missing Adjustable Coupon Dated Date | — | Missing adjustable coupon date detection |
| 121 | MDS.OCFC.Null Factor | — | Null factor handling |
| 122 | MDS.OCFC.Null Security Code | — | Null security code detection |
| 123 | MDS.OCFC.Principal Type Name Integrity | — | Principal type name integrity |
| 124 | MDS.OCFC.Security Currency Code | — | Security currency code validation |
| 125 | MDS.OCFC.Security Details Null | — | Security details null checking |
| 126 | MDS.OCFC.Stale Factor | — | Stale factor detection |
| 127 | MDS.OCFC.Underlier Security | — | Underlier security validation |
| 128 | MDS.OCFC.Validate USER1 | — | USER1 field validation |

### 3.3 Work Queues (26)

| # | Queue Name | Key Field | Max Attempts | Purpose |
|---|---|---|---|---|
| 1 | MDS.OCFC.Convertible Bonds | Item Key | Default | Convertible bond check items |
| 2 | MDS.OCFC.Coupon Date Integrity | Item Key | Default | Coupon date integrity items |
| 3 | MDS.OCFC.Coupon Rate Integrity | Item Key | Default | Coupon rate integrity items |
| 4 | MDS.OCFC.Coupon Type Change | Item Key | Default | Coupon type change items |
| 5 | MDS.OCFC.Dupe Client Security | Item Key | Default | Duplicate client security items |
| 6 | MDS.OCFC.Dupe ISIN | Item Key | Default | Duplicate ISIN items |
| 7 | MDS.OCFC.Duplicate Factor | Item Key | Default | Duplicate factor items |
| 8 | MDS.OCFC.ECF Checks | MF ITEM KEY | 3 | ECF check items (with retry) |
| 9 | MDS.OCFC.End Date Validation | Item Key | Default | End date validation items |
| 10 | MDS.OCFC.Exchange Code | Item Key | Default | Exchange code items |
| 11 | MDS.OCFC.Factor Rate Consistency | Item Key | Default | Factor rate consistency items |
| 12 | MDS.OCFC.Factor Variance | Item Key | Default | Factor variance items |
| 13 | MDS.OCFC.Inflation Index | Item Key | Default | Inflation index items |
| 14 | MDS.OCFC.Mails | Subject | 10 | Email notification items (high retry) |
| 15 | MDS.OCFC.Matured or Expired Positions | Item Key | Default | Matured/expired position items |
| 16 | MDS.OCFC.Maturity Date | Item Key | Default | Maturity date items |
| 17 | MDS.OCFC.MDL INTRA POS SM Factor Rate Consistency | Item Key | Default | Intraday factor rate items |
| 18 | MDS.OCFC.Missing Adjustable Coupon Dated Date | Item Key | Default | Missing adjustable coupon items |
| 19 | MDS.OCFC.Null Factor | Item Key | Default | Null factor items |
| 20 | MDS.OCFC.Null Security Code | Item Key | Default | Null security code items |
| 21 | MDS.OCFC.Principal Type Name Integrity | Item Key | Default | Principal type items |
| 22 | MDS.OCFC.Security Currency Code | Item Key | Default | Security currency code items |
| 23 | MDS.OCFC.Security Details Null | Item Key | Default | Security details null items |
| 24 | MDS.OCFC.Stale Factor | Item Key | Default | Stale factor items |
| 25 | MDS.OCFC.Underlier Security | Iten Key | Default | Underlier security items |
| 26 | MDS.OCFC.Validate USER1 | Item Key | Default | USER1 validation items |

> **Note:** Queue 25 (Underlier Security) has key field "Iten Key" — likely a typo in BP. UiPath mapping should use "Item Key".

### 3.4 Credentials (CyberArk / Cloakware)

| # | Credential Domain | System | CyberArk Domain | UiPath Asset Name | Usage |
|---|---|---|---|---|---|
| 1 | ECF | External Corporate Finance | robmfa | CyberArk_ECF_Credential | ECF HTTP login, ECF data upload |
| 2 | ERP | Enterprise Resource Planning | robmfa | CyberArk_ERP_Credential | ERP HTTP login |
| 3 | ERP 2FA | ERP Two-Factor Auth | edr | CyberArk_ERP_2FA_Credential | ERP 2FA token |
| 4 | GTM 2FA | GTM Two-Factor Auth | edr | CyberArk_GTM_2FA_Credential | GTM 2FA token |
| 5 | IMS HUB | IMS Hub Services | edr | CyberArk_IMSHUB_Credential | IMS Hub web service auth |
| 6 | MCH | MCH Mainframe (RACF) | racf | CyberArk_MCH_Credential | MCH mainframe login |
| 7 | MDP | Market Data Platform | adcorp | CyberArk_MDP_Credential | MDP HTTP login |
| 8 | MDP SecurID | MDP Two-Factor Auth | robmfa | CyberArk_MDP_SecurID_Credential | MDP SecurID token |
| 9 | PSDL | PSDL Application | mdpapp08797robocfc02 | CyberArk_PSDL_Credential | PSDL web object auth |
| 10 | RDOC | RDOC System | edr | CyberArk_RDOC_Credential | RDOC HTTP login |
| 11 | RKS | RKS Application | adcorp | CyberArk_RKS_Credential | RKS launch and login |
| 12 | RKS SecurID | RKS Two-Factor Auth | robmfa | CyberArk_RKS_SecurID_Credential | RKS SecurID token |

### 3.5 Environment Variables → UiPath Assets (272)

The 272 environment variables are categorized by functional area:

| Category | Count | Examples | UiPath Asset Type |
|---|---|---|---|
| System URLs & API Endpoints | ~45 | ECF HTTP API URL, ERP APPLICATION URL, MDP HTTP API URL, RKS URL | Text |
| HTTP Login Payloads | ~15 | ECF HTTP LOGIN PAYLOAD, ERP HTTP LOGIN PAYLOAD, MDP HTTP LOGIN PAYLOAD | Text |
| CyberArk Domains | 12 | MCH CLOAKWARE DOMAIN, RKS CLOAKWARE DOMAIN | Text |
| Process Names | ~15 | ORCHESTRATOR PROCESS NAME, FACTOR GENERIC PROCESS NAME | Text |
| DB Tables | ~12 | ITEM DETAILS DB TABLE, LOCKING DB TABLE, MAIL GROUPING DB TABLE | Text |
| File Paths | ~20 | ROOT DIRECTORY, MASTER FILE PATH, SCREENSHOT FOLDER PATH | Text |
| Timeouts & Timespans | ~15 | WQ TIMEOUT, FACTOR LOCK TIMEOUT, 3122 DEFER TIME | Text |
| Flags & Toggles | ~20 | TESTING FLAG, API URL LOG FLAG, MDP NEW LAYOUT, ERP_2FA_FLAG | Flag (Boolean) |
| Client Mappings | ~10 | CLIENT MAPPING, INFLATION MAPPING, MVP1 BGOV CHECK | Text |
| RKS Configuration | ~30 | RKS LAUNCH VIA HTTP, RKS EDGE INPRIVATE, RKS CLOSE WAITING TIME | Text/Number |
| ESP Configuration | ~10 | ESP CONNECTION STRING, ESP CONNECTION DRIVER, ESP DOMAIN | Text |
| Email Configuration | ~10 | BU GENERAL MAIL, DATA_MANAGEMENT_MAIL, RPA SUPPORT MAIL | Text |
| Template & Upload Config | ~10 | TEMPLATE NAME, TEMPLATES_PATH, RMG TEMPLATE FOLDER | Text |
| MDP Configuration | ~20 | MDP PROFILE NAME, MDP LOGON MODE, MDP FORGEROCK | Text |
| Thresholds & Limits | ~15 | WQ MAX ATTEMPTS, CLEANUP THRESHOLD, MAX TIME WORKER PROCESSING | Number |
| Miscellaneous | ~13 | USE CASE NAME, WS USER DEV, WS USER DOMAIN | Text |

> **Full Environment Variable List:** All 272 variables are documented in the .bprelease file with name, type, value, and description. Each will be created as a UiPath Orchestrator Asset with the same name/value mapping.

---

## 4. Business Process Descriptions

### 4.1 MDS.112.OCFC.Orchestrator

**Trigger:** Scheduled (daily or on-demand)
**Steps:**
1. Initialize environment — load configuration, validate prerequisites
2. Determine which check processes to execute based on schedule/configuration
3. Trigger individual check processes in sequence or parallel based on dependency rules
4. Monitor execution progress via status tracking
5. Handle orchestration-level exceptions

**Business Rules:**
- Process execution order follows dependency chain (e.g., Sentinel must run before status reporting)
- Dynamic scheduler configuration controls which checks run on which days
- Automation end time constraint enforced to prevent overruns

**Exceptions:** Orchestration failure → email notification to RPA Support

### 4.2 MDS.112.OCFC.Sentinel

**Trigger:** Invoked by Orchestrator
**Steps:**
1. Check ESP access and connectivity
2. Check client configurations
3. Validate sentinel-specific prerequisites (cookie timeouts, system availability)
4. Monitor running check processes
5. Aggregate results and generate sentinel reports
6. Send status notifications

**Business Rules:**
- Sentinel checks must complete before data-modifying checks begin
- ESP access validation is mandatory — failure blocks all downstream processes
- Client-specific sentinel skip flags allow selective bypasses

**Exceptions:** Sentinel failure → immediate escalation, all worker processes paused

### 4.3 Factor Validation Processes

The following processes share a common pattern via the **Factor Generic Process** framework:

- **Null Factor:** Detects securities with missing factor values; validates against MDP/ESP data sources; uses deferred processing with lock mechanism
- **Stale Factor:** Identifies factor data that hasn't been updated within threshold periods
- **Factor Variance:** Detects statistically significant deviations in factor values between sources
- **Factor Rate Consistency:** Cross-validates factor rates between RKS, ESP, and MDP
- **Duplicate Factor:** Identifies duplicate factor records in the system

**Common Pattern (35 subsheets in Factor Generic):**
1. Load queue item from work queue
2. Retrieve security data from DB (item details table)
3. Retrieve factor data from ESP/MDP via HTTP APIs
4. Compare values against RKS data
5. Apply check-specific business rules (3006, 3077, 3122, 3334/3019, 3917)
6. If discrepancy found → determine resolution action (RKS update, notification, exception)
7. Execute resolution action via appropriate system object
8. Log results and mark queue item complete/exception

### 4.4 Securities Integrity Processes

- **Dupe ISIN Process:** Detects duplicate ISIN codes across security masters; 23 subsheets with complex matching logic
- **Dupe Sedols Process:** Detects duplicate SEDOL codes; 31 subsheets — highest complexity among dupe checks
- **Dupe Client Security:** Detects duplicate client-security mappings; 26 subsheets
- **Null Security Code Process:** Identifies securities with null/missing security codes
- **Security Currency Code:** Validates currency code assignments against reference data
- **Security Details Null Check:** Checks for null values in required security detail fields
- **Exchange Code:** Validates exchange code assignments

### 4.5 Coupon & Date Processes

- **Coupon Date Integrity:** Validates coupon payment date consistency across systems (19 subsheets)
- **Coupon Rate Integrity:** Validates coupon rate data accuracy
- **Coupon Type Change:** Detects changes in coupon type classifications (Fixed/Float/Adjustable/Variable/STEP)
- **Missing Adjustable Coupon Dated Date:** Identifies adjustable securities missing dated dates
- **End Date Validation:** Validates security end dates (maturity, expiration)
- **Maturity Date:** Validates maturity date fields
- **Matured or Expired Positions:** Identifies positions in matured/expired securities

### 4.6 Specialized Processes

- **ECF Checks Processing:** External Corporate Finance data validation with 3-retry queue (21 subsheets)
- **Inflation Index Process:** Processes inflation index-linked securities (29 subsheets)
- **Convertible Bonds:** Validates convertible bond security attributes
- **Underlier Security:** Validates underlier-to-derivative security relationships (11 subsheets)
- **Validate USER1 Process:** Validates USER1 field values across all securities (29 subsheets)
- **Multilisted USER1 Process:** Handles multilisted securities USER1 validation (17 subsheets)
- **Search USER1 Process:** Searches for USER1 field values across systems (12 subsheets)
- **Principal Type Name Integrity:** Validates principal type classifications (9 subsheets)
- **SMF Calculation Process:** Standard Market Factor calculation (8 subsheets)
- **RKS And DataMarts Validation Process:** Cross-validates RKS data against DataMarts (16 subsheets)

### 4.7 Support Processes

- **Status Report:** Generates automation execution status reports
- **Orchestrator:** Master process triggering and sequencing

---

## 5. External System Dependencies

| # | System | Type | Authentication | Protocol | Purpose |
|---|---|---|---|---|---|
| 1 | **RKS** | Citrix/Web Application | CyberArk (adcorp) + SecurID (robmfa) | HTTP/UIA/Edge | Securities master — data retrieval and updates |
| 2 | **MCH** | Mainframe (3270 Terminal) | CyberArk (racf) | TN3270 | MCH mainframe — security data retrieval (BFSE, BSEC, BGOV screens) |
| 3 | **MDP** | Web API | CyberArk (adcorp) + ForgeRock/SecurID | HTTPS REST | Market Data Platform — factor/security data retrieval |
| 4 | **ESP** | Database/Web | CyberArk + Connection String | ODBC/HTTP | Enterprise Securities Platform — cross-reference, transactions, factor tables |
| 5 | **ECF** | Web API + Upload | CyberArk (robmfa) | HTTPS REST | External Corporate Finance — data validation and upload |
| 6 | **ERP** | Web Application + API | CyberArk (robmfa) + 2FA (edr) | HTTPS | Enterprise Resource Planning — workflow integration |
| 7 | **GTM** | Web Application | CyberArk + 2FA (edr) | HTTPS | GTM — securities reference data |
| 8 | **PSDL** | Web Application | CyberArk (mdpapp alias) | HTTPS | PSDL — securities data |
| 9 | **RDOC** | Web API | CyberArk (edr) | HTTPS REST | RDOC — document retrieval |
| 10 | **RMG** | Web API | CyberArk + Upload User | HTTPS REST | RMG — report management, template upload |
| 11 | **SSCD** | Web API | CyberArk | HTTPS REST | SSCD — securities classification data |
| 12 | **IMS HUB** | Web Service (SOAP) | CyberArk (edr) | HTTPS SOAP | IMS Hub — integration messaging |
| 13 | **OCF** | Web API | CyberArk | HTTPS REST | OCF — data source wrapper |
| 14 | **SQL Database** | Database | Integrated/Connection String | SQL | Item details, locking, notifications, logging |
| 15 | **Outlook** | Desktop Application | Windows Auth | MAPI | Email notifications and reports |

---

## 6. Non-Functional Requirements

### 6.1 Performance
- All 26 queues must process within the configured automation end time window
- Individual queue item processing time must not regress from Blue Prism baseline
- RKS launch and navigation must handle Citrix latency with configurable wait times

### 6.2 Reliability
- Retry logic for DB interactions (configurable attempts and sleep intervals)
- ECF queue: 3 max attempts; Mails queue: 10 max attempts
- DB locking mechanism with configurable lock timeouts to prevent deadlocks
- Deferred processing for Null Factor, Factor Consistency, Principal Type, and Security Details with configurable defer times

### 6.3 Security
- All credentials stored in CyberArk — no hardcoded passwords in workflows
- 2FA support for ERP, GTM, MDP, and RKS via SecurID tokens
- PII must not appear in UiPath logs
- Robot machine access restricted to approved network zones

### 6.4 Audit
- Process-level and transaction-level logging (WriteProcessLog, WriteTransactionLog)
- Splunk integration for process and transaction logs (configurable via flags)
- Screenshot capture on exception (configurable folder path)
- API URL request logging (configurable via flag)
- Web service call logging via Webservice Log object

---

## 7. Acceptance Criteria

| # | Criterion | Validation Method |
|---|---|---|
| 1 | All 33 processes converted and functional in UiPath | End-to-end test with sample data |
| 2 | All 26 work queues operational in Orchestrator | Queue item lifecycle test (add/process/complete/exception) |
| 3 | All 12 credential integrations working via CyberArk | Login test for each external system |
| 4 | Factor validation results match Blue Prism output | Parallel run comparison |
| 5 | Exception handling: Business and System exceptions processed correctly | Forced exception test scenarios |
| 6 | Email notifications generated correctly | Notification comparison test |
| 7 | Status reports generated and match format | Report output comparison |
| 8 | Deferred processing works within configured timeouts | Deferred item lifecycle test |
| 9 | DB locking mechanism prevents concurrent conflicts | Concurrent execution test |
| 10 | All 272 environment variables mapped to UiPath Assets | Asset inventory verification |
| 11 | Logging and audit trail complete (process + transaction level) | Log output review |
| 12 | RKS automation works in Edge, HTTP, and UIA modes | Multi-mode launch test |

---

## 8. Assumptions

1. Blue Prism 7.3 source code in the .bprelease accurately reflects production behavior
2. CyberArk vault configuration available for UiPath robot accounts
3. All external systems (RKS, MCH, MDP, ESP, ECF, ERP, GTM, etc.) accessible from UiPath robot machines
4. Database schema and stored procedures remain unchanged during migration
5. Email distribution lists and templates remain the same
6. 058 – Market Profiling reusable libraries (MCH core, MAP, Logging, LoginAgent, Environment, Excel Utilities) are available for import
7. RKS Citrix/browser automation can be replicated in UiPath with equivalent reliability
8. MDP ForgeRock authentication flow is compatible with UiPath HTTP activities
9. No business process re-engineering — functional parity only

---

## 9. Risks

| # | Risk | Probability | Impact | Mitigation |
|---|---|---|---|---|
| 1 | RKS Citrix automation instability | High | High | Implement robust retry logic; test all 3 launch modes (HTTP, Edge, UIA) |
| 2 | MCH mainframe session management | Medium | High | Reuse MCH core (Login/Logoff/Navigate) from 058; build 112-specific screens (BFSE, BSEC, BGOV) with session recovery |
| 3 | MDP ForgeRock 2FA integration complexity | Medium | High | Early spike in Sprint 1 to validate auth flow in UiPath |
| 4 | 26 queue schema migration correctness | Medium | Medium | Automated queue schema validation test suite |
| 5 | 272 environment variable mapping errors | Medium | Medium | Automated asset inventory comparison script |
| 6 | DB locking mechanism behavior differences | Low | High | Unit test locking with concurrent sessions early |
| 7 | Deferred processing timing differences | Medium | Medium | Validate defer times match BP behavior |
| 8 | ECF/RMG file upload compatibility | Medium | Medium | Test upload with production file formats in DEV |
| 9 | ESP ODBC connection string migration | Low | Medium | Verify connection string format in UiPath |
| 10 | Business rule regression in 3019/3334 logic (20+ scenario objects) | Medium | High | Comprehensive test matrix covering all coupon type scenarios |

---

## 10. Glossary

| Term | Definition |
|---|---|
| BFSE | MCH screen — Fund Search Extended |
| BSEC | MCH screen — Security Information |
| BGOV | MCH screen — Government Securities |
| CPN_TYP | Coupon Type (Fixed, Float, Adjustable, Variable, STEP) |
| ECF | External Corporate Finance |
| ERP | Enterprise Resource Planning |
| ESP | Enterprise Securities Platform |
| GTM | Global Transaction Management |
| IDF | Internal Data Feed |
| IMS HUB | Integration Messaging Services Hub |
| ISIN | International Securities Identification Number |
| MCH | Mainframe Clearing House |
| MDP | Market Data Platform |
| MDS | Master Data Services |
| MVP | Minimum Viable Product (development phase label) |
| OCFC | Ongoing Corporate Finance Checks |
| OCF | Ongoing Corporate Finance |
| PSDL | Platform Securities Data Layer |
| RDOC | Reference Document system |
| RKS | Reference Knowledge System (securities master) |
| RMG | Report Management Gateway |
| SEDOL | Stock Exchange Daily Official List (identifier) |
| SMF | Standard Market Factor |
| SSCD | Securities Standard Classification Data |
| TBA | To Be Announced (securities) |
| USER1 | Custom user field in securities master |
