# 029 – AGENT UTILIZATION

| Field | Value |
|---|---|
| Document ID | 029-AU-001 |
| Project | BOAT.029 – COB FACTOR RECON |
| Source | 029_Project_Plan.md |
| Date | 2026-04-08 |
| Status | Draft v1.0 |

---

## 1. Agent Capabilities in RPA Migration Context

| Capability | Confidence % | Notes for 029 |
|---|---|---|
| Parse BP .bprelease XML files (processes, objects, queues, env vars) | 99% | Completed: 9 processes, 8 objects, 2 queues, 22 env vars extracted |
| Write BRD from BP analysis | 95% | BRD 029-BRD-001 fully generated; LEAD review required |
| Write TDD from BRD | 92% | TDD 029-TDD-001 fully generated; selector strategy requires DEV review |
| Scan reference projects for reusable components | 98% | 058 and 057 scanned; 160h savings documented |
| Write XAML workflow scaffolding (arguments, basic flow) | 90% | All workflow stubs generated; DEV must add real selectors and test |
| Generate Config.xlsx rows (Settings, Constants, Assets sheets) | 99% | All 28 assets + constants defined; DEV validates values against env |
| Write project plans, sprint plans, utilization calculations | 99% | All sprint math validated at 80% DEV and ~81% LEAD utilization |
| Generate VB.Net expression syntax for workflow logic | 88% | Suitable for standard patterns; complex MCH screen logic requires DEV |
| Write test cases | 95% | All step test cases generated; DEV executes and records pass/fail |
| Write handover docs (SOP, runbook, escalation matrix) | 95% | Generated in Sprint 4; LEAD reviews for accuracy |
| UI selector capture for MCH/AVIVA terminal | 0% | Agent cannot see screens; DEV must capture all selectors in Studio |
| Debug visual workflow wiring in UiPath Studio | 0% | DEV only; requires Studio IDE |
| Test against live MCH/Bloomberg/RDR systems | 0% | DEV only; requires system access |

---

## 2. Agent Role per Sprint

| Sprint | Phase | Agent Tasks | Est. Agent Hours | Human Tasks | Handoff Point |
|---|---|---|---|---|---|
| Sprint 1 | Assessment + Design | Parse .bprelease; generate BRD, TDD, RC Document, Plan, Dev Guide; generate Config.xlsx all rows; generate XAML scaffolds Steps 0–5; Orchestrator asset list | ~40h | LEAD: Review + sign off BRD/TDD; DEV: Import reusables, wire MCH login selectors, test | Agent delivers docs + scaffolds at start of Week 1; DEV imports and tests Week 1–2 |
| Sprint 2 | Core Libraries | Generate XAML scaffolds for MCH screen workflows (BGMM, BGIR, BSEC, BPOP, PYDN, PYUP, PYDA); generate Bloomberg HTTP WS Activity configuration; generate RDR DLL invocation VB.Net stubs | ~8h | DEV: Capture real MCH selectors; test against MCH test env; RDR DLL integration | Agent delivers scaffolds at sprint start; DEV wires selectors and tests Week 3–4 |
| Sprint 3 | Performers + Libraries | Generate XAML scaffolds for DataTransformation, Reporting, Posting library workflows; generate Posting_Main orchestration logic; generate DataExtraction performer workflow stubs | ~8h | DEV: Wire actual posting logic in MCH; test Posting Library on MCH dev; integrate with DataExtraction | Agent delivers scaffolds Week 5; DEV implements and tests Week 5–6 |
| Sprint 4 | Testing + Deployment + Docs | Generate Project Plan (029-PP-001), Agent Utilization (029-AU-001), Honest Timeline Assessment (029-TA-001), Utilization Summary (029-US-001); generate SOP/runbook/escalation matrix documents; generate NuGet package metadata | ~24h | LEAD: E2E test; UAT with business; production deployment; schedule config | Agent delivers closure docs Week 7; LEAD validates and signs off Week 8 |
| **Total** | | | **~80h** | | |

---

## 3. Agent + Developer Partnership Model

| Deliverable Type | Agent Contribution | Developer Contribution | Lead Contribution |
|---|---|---|---|
| BRD from .bprelease | Generates complete BRD from parsed XML | Validates external system details with SMEs | Reviews and signs off |
| TDD from BRD | Generates complete architecture, library design, queue schema, asset list | Validates selector strategy, DLL integration approach | Architecture sign-off |
| XAML workflow scaffolds | Generates argument definitions, basic sequence structure, Try/Catch wrappers, VB.Net expressions | Adds real UI selectors in UiPath Studio; wires application-specific logic; debugs | Code review |
| Config.xlsx | Generates all rows for all 3 sheets | Updates actual values per environment (URLs, paths) | Validates completeness |
| Test cases | Generates test case table for every step | Executes tests; records Pass/Fail; reports failures | Reviews exception scenarios |
| Sprint plan / utilization | Calculates all effort, SP, utilization math | Confirms story complexity estimates | Reviews allocation |
| SOP / runbook | Drafts operational procedures from TDD | Validates steps against actual system behaviour | Signs off as operational |

**Collaboration Rule:** Agent generates → Developer validates/wires → Lead reviews/approves. No Agent output goes to production without Developer validation for code, and Lead sign-off for design.

---

## 4. Agent Utilization by Phase

| Phase | Agent % | Developer % | Lead % | Notes |
|---|---|---|---|---|
| Phase 1: Assessment | **90%** | 10% | 10% | Agent does all BP parsing and BRD generation; LEAD confirms scope |
| Phase 2: Design | **80%** | 15% | 30% | Agent writes TDD; LEAD drives architecture decisions; DEV reviews selector strategy |
| Phase 3A: Foundation (Sprint 1) | **40%** | 80% | 20% | Agent generates scaffolds; DEV imports and wires reusables |
| Phase 3B: Core Libraries (Sprint 2) | **15%** | 100% | 30% | DEV-heavy: MCH screen selectors, Bloomberg/RDR integration |
| Phase 3C: Performers (Sprint 3) | **15%** | 100% | 30% | DEV-heavy: Posting logic, DataExtraction wiring |
| Phase 3D: Integration (Sprint 4 first half) | **10%** | 100% | 60% | DEV+LEAD-heavy: E2E testing, exception handling |
| Phase 4: Testing | **5%** | 100% | 50% | Agent assists with test doc updates only |
| Phase 5: Deployment | **10%** | 60% | 100% | LEAD-heavy: Orchestrator config, CyberArk, schedules |
| Phase 6: Cutover + Closure | **70%** | 20% | 50% | Agent generates all closure documents; LEAD drives go-live |

---

## 5. Agent ROI Summary

### Hours Saved by Agent vs. Manual Documentation

| Document | Manual Effort (est.) | Agent Output Time | Hours Saved |
|---|---|---|---|
| BRD (029-BRD-001) — parsing 46 BP components | 16h | Instant | 16h |
| TDD (029-TDD-001) — full architecture design | 14h | Instant | 14h |
| RC Document (029-RC-001) — reuse scanning | 8h | Instant | 8h |
| Plan (029-PLAN-001) — 6 phases, all stories | 6h | Instant | 6h |
| Development Guide (029-DEV-001) — 11 steps, 10+ test tables | 12h | Instant | 12h |
| Project Plan (029-PP-001) — 4 sprints, full utilization math | 10h | Instant | 10h |
| Agent Utilization (029-AU-001) | 4h | Instant | 4h |
| Honest Timeline Assessment (029-TA-001) | 6h | Instant | 6h |
| Utilization Summary (029-US-001) | 4h | Instant | 4h |
| XAML scaffolds (~35 workflows generated) | 35h (1h each) | ~4h total | 31h |
| Config.xlsx rows (28 assets + constants) | 4h | Instant | 4h |
| **TOTAL** | **119h** | **~4h** | **~115h** |

### Quality Improvements from Agent

| Improvement | Impact |
|---|---|
| Consistent cross-document references (same process names, story IDs, effort figures) | Reduces review rework by ~8h |
| Complete exception matrix (12 exception scenarios) | Reduces missed edge cases in DEV testing |
| All BP narratives and subsheets documented | Provides DEV with complete context before touching Studio |
| Queue schemas fully defined before Sprint 2 | Prevents late-sprint queue design rework |
| Security-first design (no hardcoded credentials, PII exclusion, Titus classification) | Reduces audit findings |
| Calculator-accurate sprint utilization (80%/100% targets confirmed each sprint) | Prevents under/over-allocation surprises |
