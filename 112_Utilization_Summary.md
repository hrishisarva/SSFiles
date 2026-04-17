# 112 MDS OCFC — Utilization Summary

| Field | Value |
|---|---|
| Document ID | 112-US-001 |
| Project | MDS.112 – COB MDS |
| Source | 112_Project_Plan.md |
| Date | 2026-04-13 |
| Status | Draft v1.0 |

---

## Section 1 — Team Utilization per Sprint

### Sprint 1 — Foundation + Infrastructure (Weeks 1–2)

| Resource | Capacity (hrs) | Allocated (hrs) | Utilization |
|---|---|---|---|
| DEV-1 | 80h | 64h | 80% |
| DEV-2 | 80h | 64h | 80% |
| LEAD | 64h | 52h | 81% |
| AGENT | — | 40h | — |
| **Total Planned** | **224h (human)** | **220h** | **—** |
| Lead Buffer | — | 12h | Reserved |

**DEV-1 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S1-01 | Clone REFramework + Config.xlsx from 058 | XS | 0 | ♻️ 0h |
| S1-02 | Import MCH Core (7 WF from 058) | XS | 0 | ♻️ 0h |
| S1-03 | Import Logging Library (3 WF from 058) | XS | 0 | ♻️ 0h |
| S1-04 | Import LoginAgent (2 WF from 058) | XS | 0 | ♻️ 0h |
| S1-05 | Import Environment (2 WF from 058) | XS | 0 | ♻️ 0h |
| S1-06 | Import Common Workflows (6 WF from 058) | XS | 0 | ♻️ 0h |
| S1-07 | Import WorkQueue Wrappers (6 WF from 058) | XS | 0 | ♻️ 0h |
| S1-08 | Import Exception Strategy from 058 | XS | 0 | ♻️ 0h |
| S1-09 | DB_Interactions Library (7 WF) | M | 4 | 16h |
| S1-10 | DB_Lock mechanism | M | 4 | 14h |
| S1-13 | Mail_Notifications (3 WF) | S | 2 | 8h |
| S1-14 | MDS_Triggering (2 WF) | S | 2 | 8h |
| S1-22 | Spike — MCH terminal | S | 2 | 6h |
| S1-23 | Spike — MDP ForgeRock | S | 2 | 6h |
| S1-24 | Spike — RKS Citrix | S | 2 | 6h |

**DEV-2 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S1-11 | MDS_Utilities Library (3 WF) | S | 2 | 8h |
| S1-12 | MDS_Config extended settings | M | 4 | 14h |
| S1-15 | ConcurrentAccess handler | M | 4 | 14h |
| S1-16 | Orchestrator folder structure | S | 2 | 8h |
| S1-17 | Queue setup (26 queues) | M | 4 | 12h |
| S1-18 | Asset creation (initial batch) | S | 2 | 8h |

**LEAD Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S1-19 | Architecture review + approval | M | 4 | 12h |
| S1-20 | Code review — DR infrastructure | S | 2 | 8h |
| S1-21 | Integration test — DB + queue flow | M | 4 | 16h |
| S1-22 | Spike review — MCH | XS | 1 | 4h |
| S1-23 | Spike review — MDP | XS | 1 | 4h |
| S1-24 | Spike review — RKS | XS | 1 | 4h |
| — | Sprint ceremonies | XS | — | 4h |

---

### Sprint 2 — MCH + HTTP/API + ESP (Weeks 3–4)

| Resource | Capacity (hrs) | Allocated (hrs) | Utilization |
|---|---|---|---|
| DEV-1 | 80h | 64h | 80% |
| DEV-2 | 80h | 64h | 80% |
| LEAD | 64h | 52h | 81% |
| AGENT | — | 22h | — |
| **Total Planned** | **224h (human)** | **202h** | **—** |
| Lead Buffer | — | 12h | Reserved |

**DEV-1 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S2-01 | MCH_BFSEDataRetrieval | M | 4 | 12h |
| S2-02 | MCH_BSECDataRetrieval | M | 4 | 12h |
| S2-03 | MCH_MultipleBSEC | S | 2 | 8h |
| S2-04 | MCH_BSECIDFGetData (2 WF) | M | 4 | 12h |
| S2-05 | MCH_BFSEIDFGetData (2 WF) | M | 4 | 12h |
| S2-06 | MCH_BGOVIDFGetData | S | 2 | 8h |

**DEV-2 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S2-07 | MCH_CheckTransactionAccess | S | 2 | 6h |
| S2-08 | HTTP_MDP_Core (4 WF) | L | 8 | 20h |
| S2-09 | HTTP_SSCD_API | S | 2 | 8h |
| S2-10 | HTTP_RMG_API | S | 2 | 8h |
| S2-11 | HTTP_ERP_API | S | 2 | 8h |
| S2-12 | HTTP_RDOC_API | S | 2 | 6h |
| S2-13 | HTTP_GTM_API | S | 2 | 8h |

**LEAD Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S2-14 | Code review — MCH screens | M | 4 | 12h |
| S2-15 | Code review — HTTP/API | M | 4 | 12h |
| S2-16 | Integration test — MCH E2E | M | 4 | 12h |
| S2-17 | Integration test — HTTP auth flows | S | 2 | 8h |
| S2-18 | ESP connectivity validation | S | 2 | 4h |
| — | Sprint ceremonies | XS | — | 4h |

---

### Sprint 3 — RKS + Web Objects (Weeks 5–6)

| Resource | Capacity (hrs) | Allocated (hrs) | Utilization |
|---|---|---|---|
| DEV-1 | 80h | 64h | 80% |
| DEV-2 | 80h | 64h | 80% |
| LEAD | 64h | 52h | 81% |
| AGENT | — | 18h | — |
| **Total Planned** | **224h (human)** | **198h** | **—** |
| Lead Buffer | — | 12h | Reserved |

**DEV-1 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S3-01 | RKS_LaunchHTTP | M | 4 | 12h |
| S3-02 | RKS_LaunchEdge | M | 4 | 12h |
| S3-03 | RKS_LaunchUIA | M | 4 | 12h |
| S3-04 | RKS_CoreNavigation (4 WF) | S | 2 | 8h |
| S3-05 | RKS_Screens batch 1 — FixedIncome, CrossRef, FactorHistory | L | 8 | 20h |

**DEV-2 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S3-06 | RKS_Screens batch 2 — EndDate, InterestRate, LookBack | L | 8 | 20h |
| S3-07 | RKS_Screens batch 3 — PaymentSchedule, Redemption, Security | L | 8 | 20h |
| S3-08 | ESP_Library (3 WF) | M | 4 | 12h |
| S3-09 | Web_ECF_Object (2FA) | M | 4 | 12h |

**LEAD Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S3-10 | Code review — RKS selectors | M | 4 | 12h |
| S3-11 | Code review — Web objects | S | 2 | 8h |
| S3-12 | Integration test — RKS E2E | L | 8 | 20h |
| S3-13 | RKS selector troubleshooting support | S | 2 | 8h |
| — | Sprint ceremonies | XS | — | 4h |

---

### Sprint 4 — Business Logic + Sentinel (Weeks 7–8)

| Resource | Capacity (hrs) | Allocated (hrs) | Utilization |
|---|---|---|---|
| DEV-1 | 80h | 64h | 80% |
| DEV-2 | 80h | 64h | 80% |
| LEAD | 64h | 52h | 81% |
| AGENT | — | 20h | — |
| **Total Planned** | **224h (human)** | **200h** | **—** |
| Lead Buffer | — | 12h | Reserved |

**DEV-1 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S4-01 | 3019_CheckLogic (8 WF) | L | 8 | 20h |
| S4-02 | 3334_CheckLogic (8 WF) | L | 8 | 20h |
| S4-03 | 3077_CheckLogic (4 WF) | M | 4 | 12h |
| S4-04 | 3122_CheckLogic (4 WF) | M | 4 | 12h |

**DEV-2 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S4-05 | 3917_CheckLogic (4 WF) | M | 4 | 12h |
| S4-06 | 3006_CheckLogic (4 WF) | M | 4 | 12h |
| S4-07 | RKS_Screens batch 4 — remaining screens | L | 8 | 20h |
| S4-08 | Web_IMS_Object | M | 4 | 12h |
| S4-09 | Web_PSDL_Object | S | 2 | 8h |

**LEAD Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S4-10 | Code review — Business Logic | L | 8 | 16h |
| S4-11 | Sentinel process wiring | M | 4 | 12h |
| S4-12 | Orchestrator process configuration | S | 2 | 8h |
| S4-13 | Integration test — business logic scenarios | M | 4 | 12h |
| — | Sprint ceremonies | XS | — | 4h |

---

### Sprint 5 — Process Integration + Testing (Weeks 9–10)

| Resource | Capacity (hrs) | Allocated (hrs) | Utilization |
|---|---|---|---|
| DEV-1 | 80h | 64h | 80% |
| DEV-2 | 80h | 64h | 80% |
| LEAD | 64h | 52h | 81% |
| AGENT | — | 24h | — |
| **Total Planned** | **224h (human)** | **204h** | **—** |
| Lead Buffer | — | 12h | Reserved |

**DEV-1 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S5-01 | Factor_Generic process wiring | L | 8 | 20h |
| S5-02 | Null_Factor process wiring | M | 4 | 12h |
| S5-03 | Security_Integrity process | M | 4 | 12h |
| S5-04 | Unit test — all MCH workflows | M | 4 | 12h |
| S5-05 | Unit test — all RKS workflows | S | 2 | 8h |

**DEV-2 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S5-06 | Coupon/Date processes | M | 4 | 12h |
| S5-07 | Specialized processes (6) | L | 8 | 20h |
| S5-08 | Unit test — HTTP/API | S | 2 | 8h |
| S5-09 | Unit test — business logic | M | 4 | 12h |
| S5-10 | Unit test — ESP + Web | S | 2 | 8h |
| S5-11 | Exception regression test | XS | 1 | 4h |

**LEAD Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S5-12 | Integration test — full Dispatcher flow | L | 8 | 16h |
| S5-13 | Integration test — full Performer flow | L | 8 | 16h |
| S5-14 | Cross-process dependency test | M | 4 | 12h |
| S5-15 | Performance comparison vs. BP baseline | S | 2 | 4h |
| — | Sprint ceremonies | XS | — | 4h |

---

### Sprint 6 — UAT + Deployment + Closure (Weeks 11–12)

| Resource | Capacity (hrs) | Allocated (hrs) | Utilization |
|---|---|---|---|
| DEV-1 | 80h | 64h | 80% |
| DEV-2 | 80h | 64h | 80% |
| LEAD | 64h | 52h | 81% |
| AGENT | — | 50h | — |
| **Total Planned** | **224h (human)** | **230h** | **—** |
| Lead Buffer | — | 12h | Reserved |

**DEV-1 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S6-01 | UAT support — process group A | L | 8 | 20h |
| S6-02 | Parallel run execution | M | 4 | 12h |
| S6-03 | Bug fix — critical defects | M | 4 | 12h |
| S6-04 | PROD deployment — packages | M | 4 | 12h |
| S6-05 | PROD deployment — assets + queues | S | 2 | 8h |

**DEV-2 Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S6-06 | UAT support — process group B | L | 8 | 20h |
| S6-07 | Bug fix — medium defects | M | 4 | 12h |
| S6-08 | PROD deployment — credentials (CyberArk) | M | 4 | 12h |
| S6-09 | Schedule configuration | S | 2 | 8h |
| S6-10 | Smoke test — PROD | M | 4 | 12h |

**LEAD Story Assignments:**
| Story ID | Story | Size | SP | Hours |
|---|---|---|---|---|
| S6-11 | UAT coordination + sign-off | L | 8 | 16h |
| S6-12 | PROD checklist execution | M | 4 | 12h |
| S6-13 | Rollback plan validation | S | 2 | 8h |
| S6-14 | Go-live approval | S | 2 | 8h |
| S6-15 | Knowledge transfer | S | 2 | 4h |
| — | Sprint ceremonies | XS | — | 4h |

---

## Section 2 — Overall Utilization Summary

| Resource | Sprint 1 (hrs / %) | Sprint 2 (hrs / %) | Sprint 3 (hrs / %) | Sprint 4 (hrs / %) | Sprint 5 (hrs / %) | Sprint 6 (hrs / %) | Total Hrs | Avg % |
|---|---|---|---|---|---|---|---|---|
| DEV-1 | 64h / 80% | 64h / 80% | 64h / 80% | 64h / 80% | 64h / 80% | 64h / 80% | **384h** | **80%** |
| DEV-2 | 64h / 80% | 64h / 80% | 64h / 80% | 64h / 80% | 64h / 80% | 64h / 80% | **384h** | **80%** |
| LEAD | 52h / 81% | 52h / 81% | 52h / 81% | 52h / 81% | 52h / 81% | 52h / 81% | **312h** | **81%** |
| AGENT | 40h / — | 22h / — | 18h / — | 20h / — | 24h / — | 50h / — | **174h** | **—** |
| **Total Planned** | **220h** | **202h** | **198h** | **200h** | **204h** | **230h** | **1,254h** | **—** |
| Lead Buffer | 12h | 12h | 12h | 12h | 12h | 12h | 72h | Reserved |

---

## Section 3 — Story Point Distribution

| Resource | Stories | Total SP | % Share |
|---|---|---|---|
| DEV-1 | 48 | 217 | 39.5% |
| DEV-2 | 39 | 210 | 38.3% |
| LEAD | 20 | 122 | 22.2% |
| **TOTAL** | **107** | **549** | **100%** |

### Story Point Distribution by Size

| Size | SP | Count | % of Total SP |
|---|---|---|---|
| XS (1 SP) | 1 | 9 | 0.2% |
| S (2 SP) | 44 | 24 | 8.0% |
| M (4 SP) | 160 | 44 | 29.1% |
| L (8 SP) | 264 | 33 | 48.1% |
| XL (16 SP) | 80 | 5 | 14.6% |
| 0 (Reused) | 0 | 8 | — |
| **TOTAL** | **549** | **123** | **100%** |

---

## Section 4 — Reuse Impact on Utilization

### Zero-Effort Stories

| Story ID | Description | Original Effort (058) | 112 Effort | Hours Saved |
|---|---|---|---|---|
| S1-04 | REFramework + Config.xlsx | 24h | 0h | 24h |
| S1-05 | Logging (3 WF) | 20h | 0h | 20h |
| S1-06 | LoginAgent (2 WF) | 16h | 0h | 16h |
| S1-07 | Environment (2 WF) | 8h | 0h | 8h |
| S1-08 | Common Workflows (6 WF) | 12h | 0h | 12h |
| S1-09 | WorkQueue Wrappers (6 WF) | 16h | 0h | 16h |
| S1-10 | MCH Core (7 WF) | 32h | 0h | 32h |
| S1-11 | Exception Strategy | 12h | 0h | 12h |
| PAT-01 | Orchestrator config naming pattern reuse | 8h | 0h | 8h |
| S1-09+ | Sprint-over-sprint pattern reuse | ~110h | — | ~110h internal |
| **TOTAL** | | **148h** | **0h** | **148h external** |

### How Freed Capacity Was Redeployed

Because the 8 import stories in Sprint 1 required ~0h of actual effort, the freed capacity was allocated to:

| Redeployed To | Story IDs | Hours Used | Rationale |
|---|---|---|---|
| DB Infrastructure (Interactions + Locking) | S1-14, S1-15 | 30h | Complex custom locking mechanism — earliest start reduces risk |
| Infrastructure Utilities | S1-16, S1-17, S1-18, S1-19, S1-20 | 38h | Mail, Triggering, Config, Utilities — all needed by Sprint 2 |
| Orchestrator Setup | S1-12, S1-13 | 28h | 26 queues + initial assets — Sprint 2 processes require these |
| Technology Spikes | S1-22, S1-23, S1-24 | 18h | De-risk MCH, MDP, RKS — critical path validation |
| Architecture + Integration Testing | S1-21, S1-22, S1-23, S1-24 | 36h | LEAD validates foundation before Sprint 2 build begins |

> Without reuse, Sprint 1 would have been consumed entirely by framework setup + library builds (~148h). A 7th sprint would have been needed, adding 2 weeks and ~180h to the timeline.

### Sprint Reduction Impact

| Metric | Without Reuse | With Reuse | Savings |
|---|---|---|---|
| Sprints | 7 | 6 | **1 sprint** |
| Duration | 14 weeks | 12 weeks | **2 weeks** |
| Hours | ~1,402h | ~1,254h | **~148h** |
| Cost | 1,402 × $X/hr | 1,254 × $X/hr | **148 × $X/hr** |
