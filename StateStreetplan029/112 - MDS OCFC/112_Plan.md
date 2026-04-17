# 112 MDS OCFC — Plan

| Field | Value |
|---|---|
| Document ID | 112-PLAN-001 |
| Project | MDS.112 – COB MDS |
| Source | 112_Technical_Design_Document.md |
| Date | 2026-04-13 |
| Status | Draft v1.0 |

---

## Phase 1 — Assessment

| # | Task | Deliverable | Status |
|---|---|---|---|
| 1 | Parse BP release file (33 processes, 101 objects, 26 queues, 272 env vars) | 112_Business_Requirements_Document.md | ✅ Complete (Agent) |
| 2 | Inventory all components with subsheet/action counts and complexity | BRD Section 3 (Component Inventory) | ✅ Complete (Agent) |
| 3 | Map external system dependencies (14 systems, 12 credentials) | BRD Section 5 | ✅ Complete (Agent) |
| 4 | Compare MCH actions against 058 library — identify reuse vs. new builds | BRD + Reusable Components analysis | ✅ Complete (Agent) |
| 5 | Scan 058, 057, 029 for reusable components | 112_Reusable_Components_Document.md | ✅ Complete (Agent) |

---

## Phase 2 — Design

| # | Task | Deliverable | Status |
|---|---|---|---|
| 1 | Define UiPath project folder structure | TDD Section 1.1 | ✅ Complete (Agent) |
| 2 | Design Orchestrator folder, queue, and asset structure | TDD Sections 1.2, 4, 5 | ✅ Complete (Agent) |
| 3 | Define process-level design (Dispatcher/Performer model per process) | TDD Section 2 | ✅ Complete (Agent) |
| 4 | Design library architecture (MCH, RKS, HTTP, ESP, BusinessLogic, Infrastructure) | TDD Section 3 | ✅ Complete (Agent) |
| 5 | Define exception handling matrix | TDD Section 6 | ✅ Complete (Agent) |
| 6 | Define Config.xlsx schema (Settings, Constants, Assets) | TDD Section 9 | ✅ Complete (Agent) |
| 7 | Define naming conventions | TDD Section 10 | ✅ Complete (Agent) |
| 8 | Define queue schema and retry config | TDD Section 4 | ✅ Complete (Agent) |

---

## Phase 3 — Build

### Sub-Phase 3.1: Foundation (Sprint 1)

| # | Task | Owner | Size | SP | Reused from 058? |
|---|---|---|---|---|---|
| 1 | Clone REFramework + Config.xlsx from 058 | Agent + Dev | XS | 0 | ♻️ YES — 058 |
| 2 | Import SSC.Platform.Logging | Agent + Dev | XS | 0 | ♻️ YES — 058 |
| 3 | Import SSC.Platform.LoginAgent | Agent + Dev | XS | 0 | ♻️ YES — 058 |
| 4 | Import SSC.Custom.Environment | Agent + Dev | XS | 0 | ♻️ YES — 058 |
| 5 | Import Common Workflows (6 WF) | Agent + Dev | XS | 0 | ♻️ YES — 058 |
| 6 | Import WorkQueue Wrappers (6 WF) | Agent + Dev | XS | 0 | ♻️ YES — 058 |
| 7 | Import MCH Core (7 WF — Launch, Login, Navigate, Close, CleanUp, CheckIfLoggedIn, RetrievePassword) | Agent + Dev | XS | 0 | ♻️ YES — 058 |
| 8 | Setup Orchestrator DEV folder, 26 queues, initial assets | Dev + Lead | M | 4 | Pattern from 058 |
| 9 | Setup Config.xlsx with all 272 asset mappings | Agent + Dev | M | 4 | Pattern from 058 |
| 10 | Import Exception Handling Strategy | Agent + Dev | XS | 0 | ♻️ YES — 058 |

### Sub-Phase 3.2: Infrastructure & DB Libraries (Sprint 1–2)

| # | Task | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|
| 11 | Build DB_Interactions.xaml (12 actions, retry logic) | Dev | M | 4 | No — NEW |
| 12 | Build DB_Locking.xaml (6 actions, lock/unlock) | Dev | S | 2 | No — NEW |
| 13 | Build MailWrapper.xaml (11 actions) | Dev | M | 4 | No — NEW |
| 14 | Build CollectiveNotification.xaml (6 actions) | Dev | S | 2 | No — NEW |
| 15 | Build MainProcessTriggering.xaml (11 actions) | Dev | M | 4 | No — NEW |
| 16 | Build CSharp_Utilities.xaml (16 actions → VB.NET/Invoke Code) | Dev | M | 4 | No — NEW |
| 17 | Build WebserviceLog.xaml (3 actions) | Dev | XS | 1 | No — NEW |

### Sub-Phase 3.3: MCH 112-Specific Screens (Sprint 2)

| # | Task | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|
| 18 | Build MCH_CheckTransactionAccess.xaml | Dev | S | 2 | No — NEW |
| 19 | Build MCH_BFSE_DataRetrieval.xaml | Dev | M | 4 | No — NEW (first MCH screen build) |
| 20 | Build MCH_BSEC_DataRetrieval.xaml | Dev | M | 4 | No — NEW |
| 21 | Build MCH_BSEC_MultiLine.xaml | Dev | M | 4 | No — NEW |
| 22 | Build MCH_BSEC_IDF_GetData.xaml | Dev | M | 4 | No — NEW |
| 23 | Build MCH_BSEC_IDF_MainScreen.xaml | Dev | M | 4 | No — NEW |
| 24 | Build MCH_BFSE_IDF_GetData.xaml | Dev | S | 2 | ♻️ Pattern from MCH_BFSE |
| 25 | Build MCH_BFSE_IDF_Generic.xaml | Dev | S | 2 | ♻️ Pattern from MCH_BFSE |
| 26 | Build MCH_BGOV_IDF_GetData.xaml | Dev | M | 4 | No — NEW |

### Sub-Phase 3.4: HTTP/API Libraries (Sprint 2–3)

| # | Task | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|
| 27 | Build MDP_HTTP_DataRetriever.xaml (22 actions, ForgeRock auth) | Dev | L | 8 | No — NEW |
| 28 | Build MDP_HTTP_Extended.xaml (12 actions) | Dev | M | 4 | No — NEW |
| 29 | Build ECF_HTTP.xaml (19 actions) | Dev | L | 8 | No — NEW |
| 30 | Build OCF_HTTP.xaml (17 actions) | Dev | M | 4 | No — NEW |
| 31 | Build SSCD_HTTP.xaml (15 actions) | Dev | M | 4 | No — NEW |
| 32 | Build RMG_HTTP.xaml (14 actions) | Dev | M | 4 | No — NEW |
| 33 | Build ERP_HTTP.xaml (6 actions) | Dev | S | 2 | No — NEW |
| 34 | Build RDOC_HTTP.xaml (6 actions) | Dev | S | 2 | No — NEW |
| 35 | Build IMS_HUB.xaml (10 actions, SOAP) | Dev | M | 4 | No — NEW |

### Sub-Phase 3.5: Web Object Libraries (Sprint 3)

| # | Task | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|
| 36 | Build ERP_WebObject.xaml (23 actions, 2FA) | Dev | L | 8 | No — NEW |
| 37 | Build GTM_WebObject.xaml (20 actions, 2FA) | Dev | L | 8 | No — NEW |
| 38 | Build PSDL_WebObject.xaml (23 actions) | Dev | L | 8 | No — NEW |
| 39 | Build ECF_DataUpload.xaml (10 actions) | Dev | M | 4 | No — NEW |

### Sub-Phase 3.6: ESP Library (Sprint 2–3)

| # | Task | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|
| 40 | Build ESP_Main.xaml (20 actions) | Dev | L | 8 | No — NEW |
| 41 | Build ESP_Extended.xaml (41 actions) | Dev | XL | 16 | No — NEW |
| 42 | Build ESP_MDPValidation.xaml (5 actions) | Dev | S | 2 | No — NEW |

### Sub-Phase 3.7: RKS Library (Sprint 2–4)

| # | Task | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|
| 43 | Build RKS_LaunchHTTP.xaml (6 actions) | Dev | S | 2 | No — NEW |
| 44 | Build RKS_LaunchEdge.xaml (15 actions) | Dev | M | 4 | No — NEW |
| 45 | Build RKS_LaunchUIA.xaml (18 actions) | Dev | L | 8 | No — NEW |
| 46 | Build RKS_MainObject.xaml (24 actions) | Dev | L | 8 | No — NEW |
| 47 | Build RKS_Wrapper.xaml (46 actions) | Dev | XL | 16 | No — NEW |
| 48 | Build RKS_IssueSearch.xaml (9 actions) | Dev | S | 2 | No — NEW |
| 49 | Build RKS sub-objects (29 screen workflows) | Dev | Varies | ~60 | No — pattern reuse after first 2 |

### Sub-Phase 3.8: Business Logic Libraries (Sprint 3–4)

| # | Task | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|
| 50 | Build 3019/3334 Logic (16 workflows, 90 actions) | Dev | XL | 16 | No — NEW |
| 51 | Build 3077 Logic (1 WF, 12 actions) | Dev | M | 4 | No — NEW |
| 52 | Build 3122 Logic (4 WF, 44 actions) | Dev | L | 8 | No — NEW |
| 53 | Build 3917 Logic (1 WF, 8 actions) | Dev | S | 2 | No — NEW |
| 54 | Build MVP2/MVP4 Logic (3 WF, 71 actions) | Dev | L | 8 | No — NEW |
| 55 | Build Other Logic (FindUnderlying, GetFactorTable, Rounding, CPN_TYP, PrincipalType, MasterFile, DummySecurity) | Dev | L | 8 | No — NEW |

### Sub-Phase 3.9: Sentinel & Orchestrator (Sprint 2)

| # | Task | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|
| 56 | Build SentinelMain + sub-workflows (35 subsheets) | Dev | XL | 16 | No — NEW |
| 57 | Build OrchestratorMain + TriggerCheckProcess (4 subsheets) | Dev | M | 4 | No — NEW |

### Sub-Phase 3.10: Process Integration (Sprint 4–5)

| # | Task | Owner | Size | SP | Reused? |
|---|---|---|---|---|---|
| 58 | Wire Factor Generic Process (Dispatcher + Performer with 5 check types) | Dev + Lead | L | 8 | No — NEW |
| 59 | Wire Security Integrity Processes (7 processes) | Dev + Lead | L | 8 | No — pattern reuse |
| 60 | Wire Coupon & Date Processes (7 processes) | Dev + Lead | L | 8 | No — pattern reuse |
| 61 | Wire Specialized Processes (10 processes) | Dev + Lead | XL | 16 | No — NEW |
| 62 | Wire Support Processes (Status Report) | Dev | S | 2 | No — NEW |

---

## Phase 4 — Testing

| # | Test Type | Scope | Owner | Pass Criteria |
|---|---|---|---|---|
| 1 | Unit Tests | Each workflow in isolation | DEV-1 / DEV-2 | All actions execute without error; correct output for sample data |
| 2 | Integration Tests | Process-level (Orchestrator → Sentinel → Check Process → RKS/MCH/MDP/ESP) | LEAD | End-to-end queue item lifecycle completes successfully |
| 3 | Business Validation | Factor validation results match Blue Prism output | LEAD + Business | Results comparison — 100% match for same input data |
| 4 | Performance Tests | Queue throughput — 26 queues with N items each | LEAD | Processing completes within automation end time window |
| 5 | Exception Tests | Forced Business/System exceptions | DEV-1 / DEV-2 | Correct exception type, retry behavior, notification |
| 6 | Credential Tests | CyberArk login for all 12 credential domains | LEAD | Successful authentication for each system |
| 7 | Multi-Mode RKS Test | Test HTTP, Edge, UIA launch modes | DEV-1 | All 3 modes work; fallback triggers correctly |
| 8 | Deferred Processing Test | Null Factor, Factor Consistency, Principal Type, Security Details | DEV-2 | Items deferred and reprocessed within configured timeout |

---

## Phase 5 — Deployment

- [ ] Create Orchestrator production folder (MDS-OCFC-112)
- [ ] Create all 26 queues in production
- [ ] Create all 272 assets with production values
- [ ] Create all 12 credential assets linked to CyberArk production safes
- [ ] Publish UiPath packages (.nupkg) to Orchestrator feed
- [ ] Configure Unattended Robot machine assignment
- [ ] Configure triggers/schedules
- [ ] Verify network access from robot machine to all 14 external systems
- [ ] Run smoke test with production credentials
- [ ] Enable monitoring and alerts

---

## Phase 6 — Cutover

- [ ] Parallel run: Execute BP and UiPath side-by-side for 1 cycle
- [ ] Compare outputs: Factor validation results, notification emails, status reports
- [ ] Resolve any discrepancies found in parallel run
- [ ] Business sign-off on functional parity
- [ ] Switch production schedule from Blue Prism to UiPath
- [ ] Disable Blue Prism schedule
- [ ] Monitor first 3 production runs in UiPath
- [ ] Rollback procedure: Re-enable Blue Prism schedule if critical failure
- [ ] Post-go-live hypercare (1 week)
- [ ] Project closure and handover documentation
