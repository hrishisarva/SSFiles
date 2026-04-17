# 112 MDS OCFC — Agent Utilization

| Field | Value |
|---|---|
| Document ID | 112-AU-001 |
| Project | MDS.112 – COB MDS |
| Source | 112_Project_Plan.md |
| Date | 2026-04-13 |
| Status | Draft v1.0 |

---

## 1. Agent Capabilities in RPA Migration Context

| # | Capability | Confidence % | Notes |
|---|---|---|---|
| 1 | BP .bprelease XML parsing and component inventory | 98% | Fully automated — process, object, queue, env var extraction |
| 2 | BRD generation from parsed BP data | 95% | Structured document generation from XML data |
| 3 | TDD generation (architecture, folder structure, queue design) | 93% | Requires human review for selector strategy specifics |
| 4 | XAML workflow scaffold generation | 85% | Activities, arguments, Try/Catch, logging — Dev must wire selectors |
| 5 | VB.NET expression generation (string, date, DataTable ops) | 90% | Validate in Studio against live data |
| 6 | Config.xlsx population (272 asset rows) | 95% | Generate all Settings/Constants/Assets rows from env vars |
| 7 | REFramework wiring scaffold (GetTransactionData, Process, SetTransactionStatus) | 88% | Dev must test queue flow in Orchestrator |
| 8 | Exception handling block generation | 90% | Per exception matrix — Dev confirms catch order |
| 9 | Test case/script generation | 85% | Generates test matrices; Dev executes in live environment |
| 10 | SOP/Runbook documentation | 95% | Fully automated from project artifacts |
| 11 | UI selector generation | 10% | Cannot — requires live application access |
| 12 | Live system testing | 0% | Cannot — requires robot machine execution |
| 13 | Runtime debugging | 0% | Cannot — requires Studio observation |
| 14 | Orchestrator administration | 0% | Cannot — requires Orchestrator portal access |

---

## 2. Agent Role per Sprint

| Sprint | Phase | Agent Tasks | Est. Agent Hours | Human Tasks | Handoff Point |
|---|---|---|---|---|---|
| Sprint 1 | Foundation | BRD, TDD, Reusable Components Doc, Plan, Dev Guide, Config.xlsx scaffold, DB/Mail/Utility XAML scaffolds | 40h | Import libraries, Orchestrator setup, DB wiring, spikes | Agent delivers scaffolds → Dev imports to Studio |
| Sprint 2 | System Libraries | MCH screen XAML scaffolds (9 WF), HTTP API scaffolds (9 WF), ESP scaffolds (3 WF) | 22h | Selector capture, auth flow wiring, integration test | Agent delivers XAML → Dev adds selectors + tests |
| Sprint 3 | RKS + Web Objects | RKS XAML scaffolds (35 WF), Web Object scaffolds (4 WF), test matrix generation | 18h | RKS selector capture (3 modes), 2FA wiring, ECF upload | Agent delivers scaffolds → Dev captures selectors |
| Sprint 4 | Business Logic | Business logic XAML scaffolds (32 WF), Sentinel scaffold, scenario test matrix | 20h | Logic validation against BP rules, Sentinel wiring, Orchestrator wiring | Agent delivers scaffolds → Dev validates decision logic |
| Sprint 5 | Integration | Process wiring scaffolds, exception regression checklist, test evidence template | 24h | Process wiring, integration tests, exception tests, credential tests | Agent delivers test scripts → Dev executes in DEV |
| Sprint 6 | Closure | SOP/Runbook, Test Evidence Report, Closure docs, Project Plan finalization, Handover docs, Utilization reporting | 50h | UAT execution, bug fixes, PROD deployment, go-live, hypercare | Agent delivers all closure docs → Lead reviews |

---

## 3. Agent + Developer Partnership Model

| Deliverable Type | Agent Responsibility | Developer Responsibility | Handoff Format |
|---|---|---|---|
| XAML Workflow Scaffold | Generate activities, arguments, Try/Catch, log messages, control flow | Import to Studio, add selectors, wire to live systems, debug | .xaml file or copy-paste XAML snippet |
| VB.NET Expressions | Generate complete expressions for string/date/DataTable operations | Validate in Studio Immediate panel against live data | Code block in Dev Guide step |
| Config.xlsx | Generate all 272+ rows with name, value, type from BP env vars | Copy into actual Config.xlsx file, verify in Studio | Markdown table → Excel paste |
| Test Cases | Generate test matrix (input → expected output) | Execute tests in DEV environment, record Pass/Fail | Test table in Dev Guide |
| Documentation | Generate full documents (BRD, TDD, Plan, SOP, etc.) | Review for accuracy, approve, distribute | Markdown file |
| Selectors | Cannot generate — no live app access | Capture using UiPath Explorer in live environment | N/A |
| Live Testing | Cannot execute — no robot access | Execute all tests on robot machine | N/A |

---

## 4. Agent Utilization by Phase

| Phase | Agent % | Developer % | Lead % | Notes |
|---|---|---|---|---|
| Assessment (S1 Week 1) | 70% | 15% | 15% | Agent dominant — parsing, BRD, TDD |
| Foundation (S1 Week 2) | 30% | 50% | 20% | Shift to Dev — imports, DB build |
| System Libraries (S2) | 15% | 65% | 20% | Dev dominant — selectors, auth flows |
| RKS + Web Objects (S3) | 12% | 68% | 20% | Dev dominant — Citrix/browser selectors |
| Business Logic (S4) | 13% | 65% | 22% | Dev dominant — logic validation |
| Integration (S5) | 15% | 55% | 30% | Lead increases — integration tests |
| UAT + Deployment (S6 Week 1) | 10% | 50% | 40% | LEAD dominant — deployment |
| Closure (S6 Week 2) | 60% | 15% | 25% | Agent dominant — documentation |

---

## 5. Agent ROI Summary

### Hours Saved vs. Manual Documentation

| Task | Manual Effort | Agent Effort | Hours Saved |
|---|---|---|---|
| BRD generation from .bprelease parsing | 40h (manual XML review + writing) | 4h (automated parsing + generation) | 36h |
| TDD generation | 32h | 4h | 28h |
| Reusable Components analysis | 16h | 2h | 14h |
| Plan + Dev Guide generation | 24h | 6h | 18h |
| XAML scaffold generation (100+ workflows) | 200h (manual Studio creation) | 50h (scaffold generation) | 150h |
| Config.xlsx population (272 rows) | 16h | 2h | 14h |
| Test case generation | 24h | 8h | 16h |
| SOP/Runbook generation | 16h | 4h | 12h |
| Closure documentation | 16h | 4h | 12h |
| **TOTAL** | **384h** | **84h** | **300h** |

> **Agent generates 300h of manual work in 84h** — roughly 3.6x productivity multiplier for documentation and scaffolding tasks.

### Quality Improvements

| Area | Without Agent | With Agent |
|---|---|---|
| BP component inventory | Manual counting — error-prone | Automated XML parsing — exact counts |
| Document consistency | Manual cross-referencing — drift over time | Single-source generation — always consistent |
| Config.xlsx completeness | Manual env var mapping — easy to miss | Automated extraction — 100% coverage |
| Test coverage | Ad hoc test case writing | Systematic test matrix from exception matrix |
| Sprint estimation accuracy | Gut-feel story sizing | Data-driven from BP complexity metrics |
