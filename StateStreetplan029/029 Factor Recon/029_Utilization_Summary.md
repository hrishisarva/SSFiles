# 029 – UTILIZATION SUMMARY

| Field | Value |
|---|---|
| Document ID | 029-US-001 |
| Project | BOAT.029 – COB FACTOR RECON |
| Source | 029_Project_Plan.md |
| Date | 2026-04-08 |
| Status | Draft v1.0 |

---

## Section 1 — Team Utilization per Sprint

### Sprint 1 (Apr 8 – Apr 22, 2026)

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 52h | **81%** |
| DEV-2 | 64h | 52h | **81%** |
| LEAD-1 | 64h | 52h | **~81%** |

**DEV-1 Story Assignments — Sprint 1:**

| Story ID | Story | Size | Hours |
|---|---|---|---|
| S1-01 | Clone REFramework, create project scaffold | XS | 2h *(import only)* |
| S1-02 | Import 4 NuGet libraries | XS | 2h |
| S1-09 | MCH_StartUp, MCH_CheckIfLogged, MCH_Login, MCH_Logout | M | 22h |
| S1-12 | MasterScheduler: ReadFile, FieldExists, ValuesValidation | M | 14h |
| S1-14 | Unit test: MCH login, DB, MasterScheduler | S | 8h |
| Overhead (demo, planning, retro) | | | 4h |
| **Total** | | | **52h** |

**DEV-2 Story Assignments — Sprint 1:**

| Story ID | Story | Size | Hours |
|---|---|---|---|
| S1-03 | Import Common workflows (8 files) | XS | 2h |
| S1-04 | Import WorkQueue wrappers (9 files) | XS | 2h |
| S1-10 | MCH_BGMM, MCH_BGIR screen workflows | M | 26h |
| S1-11 | Database.Library — all 5 DB workflows | M | 14h |
| S1-14 | Unit test participation | S | 4h |
| Overhead | | | 4h |
| **Total** | | | **52h** |

**LEAD-1 Assignments — Sprint 1:**

| Story ID | Story | Hours |
|---|---|---|
| S1-05 | Configure 28 Orchestrator Assets in DEV | 8h |
| S1-06 | Configure 2 Orchestrator Queues | 4h |
| S1-07 | BRD review and sign-off | 6h |
| S1-08 | TDD review and sign-off | 12h |
| Code review (Sprint 1 PRs) | | 6h |
| Sprint planning + retro + demo | | 4h |
| Sprint 2 dependency analysis | | 6h |
| CyberArk provisioning coordination | | 2h |
| MasterScheduler requirements validation | | 4h |
| **Total** | | **52h** |

---

### Sprint 2 (Apr 22 – May 6, 2026)

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h | **100%** |
| DEV-2 | 64h | 64h | **100%** |
| LEAD-1 | 64h | 52h | **~81%** |

**DEV-1 Story Assignments — Sprint 2:**

| Story ID | Story | Size | Hours |
|---|---|---|---|
| S2-01 | MCH_BSEC, MCH_BPOP | M | 14h |
| S2-02 | MCH_PYDN posting screen | L | 20h |
| S2-05 | MasterScheduler: 4 regional holiday validators | M | 14h |
| S2-07 | Bloomberg HTTP WS extraction | L | 12h (partial; continues in S3) |
| S2-10 | Dispatcher_MainScheduler: 5 workflows | M | 14h partial |
| Overhead | | | 4h |

**DEV-2 Story Assignments — Sprint 2:**

| Story ID | Story | Size | Hours |
|---|---|---|---|
| S2-03 | MCH_PYUP posting screen | L | 20h |
| S2-04 | MCH_PYDA posting screen | L | 20h |
| S2-06 | Accounting_WS: GMM_WS + Group_CUSIP (partial) | L | 20h |
| S2-08 | RDR Login + RDR WS Extraction start | L | 8h start |
| S2-11 | Unit test participation | M | 8h |
| Overhead | | | 4h total gap absorbed |

**LEAD-1 Assignments — Sprint 2:**

| Activity | Hours |
|---|---|
| S2-09 Architecture mid-sprint review (MCH screens) | 10h |
| Code review (MCH PYDN/PYUP/PYDA + Accounting_WS) | 16h |
| RDR DLL compatibility investigation support | 8h |
| MCH selector validation & sign-off | 8h |
| Sprint 3 Posting architecture prep | 4h |
| Sprint planning + retro + demo | 6h |
| **Total** | **52h** |

---

### Sprint 3 (May 6 – May 20, 2026)

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h | **100%** |
| DEV-2 | 64h | 64h | **100%** |
| LEAD-1 | 64h | 52h | **~81%** |

**DEV-1 Story Assignments — Sprint 3:**

| Story ID | Story | Size | Hours |
|---|---|---|---|
| S3-01 | Accounting_WS: MergeBB_LotLevel + Aggregate_Filter | M | 12h |
| S3-03 | DataTransformation: BGIR_WS_Call + BGI_Loop + Write_ToExcel | M | 14h |
| S3-05 | Posting: CreateInstance + DeleteSheet + CreateSheet + CheckPostingType | M | 14h |
| S3-07 | Posting: FR.PYDN/PYUP/PYDA_Posting (3 standard) | L | 20h partial |
| S3-09 | DataExtraction performer start | L | 8h start |
| Overhead | | | 4h |

**DEV-2 Story Assignments — Sprint 3:**

| Story ID | Story | Size | Hours |
|---|---|---|---|
| S3-02 | DataTransformation: Filter_Calculate + Set_Column_Data + Change_Col_Position | M | 14h |
| S3-04 | Reporting: CreateReport + CreateWorksheet + WriteDailyReport | M | 14h |
| S3-06 | Posting: Main_Action + Posting orchestration | L | 20h |
| S3-08 | Posting: PYDN/PYUP/PYDA UMAS variants | L | 12h start |
| S3-10 | Sentinel process (6 workflows) | M | 14h partial |
| Overhead | | | 4h |

**LEAD-1 Assignments — Sprint 3:**

| Activity | Hours |
|---|---|
| S3-12 Code review: Sprint 2+3 libraries | 24h |
| S3-10 Sentinel oversight and integration support | 10h |
| Integration pre-testing | 8h |
| Exception matrix validation | 4h |
| Sprint planning + retro + demo | 6h |
| **Total** | **52h** |

---

### Sprint 4 (May 20 – Jun 3, 2026)

| Resource | Capacity | Allocated | Utilization |
|---|---|---|---|
| DEV-1 | 64h | 64h | **100%** |
| DEV-2 | 64h | 64h | **100%** |
| LEAD-1 | 64h | 52h | **~81%** |

**DEV-1 Story Assignments — Sprint 4:**

| Story ID | Story | Size | Hours |
|---|---|---|---|
| S4-01 | TransformPosting performer (5 workflows) | L | 22h |
| S4-03 | SLAMonitor process | S | 8h |
| S4-05 | Integration test: Dispatcher→DataExtraction | M | 8h |
| S4-06 | Integration test: DataExtraction→TransformPosting | M | 8h |
| S4-09 | Publish 7 library NuPkgs + deploy 8 processes | S | 8h |
| S4-12 | Exception regression testing (12 types) | M | 6h |
| Overhead | | | 4h |

**DEV-2 Story Assignments — Sprint 4:**

| Story ID | Story | Size | Hours |
|---|---|---|---|
| S4-02 | Adhoc + NOVAL dispatchers | S | 8h |
| S4-04 | DatabaseCleanup process | S | 8h |
| S4-05 | Integration test participation | M | 8h |
| S4-06 | Integration test participation | M | 8h |
| S4-08 | UAT support | M | 8h |
| S4-11 | Orchestrator schedule config support | M | 8h |
| S4-12 | Exception regression testing | M | 10h |
| Overhead | | | 6h |

**LEAD-1 Assignments — Sprint 4:**

| Activity | Hours |
|---|---|
| S4-07 End-to-end test with 3 fund list samples | 18h |
| S4-08 UAT facilitation with business | 12h |
| S4-11 Production schedule configuration + smoke test | 6h |
| S4-13 Handover walkthrough | 4h |
| Production smoke testing | 4h |
| SOP/runbook review | 4h |
| Post-deployment monitoring | 4h |
| **Total** | **52h** |

---

## Section 2 — Overall Utilization Summary

| Resource | Sprint 1 | Sprint 2 | Sprint 3 | Sprint 4 | Total Hrs | Avg Util% |
|---|---|---|---|---|---|---|
| DEV-1 | 52h (81%) | 64h (80%) | 64h (80%) | 64h (80%) | **244h** | **~80%** |
| DEV-2 | 52h (81%) | 64h (80%) | 64h (80%) | 64h (80%) | **244h** | **~80%** |
| LEAD-1 | 52h (~81%) | 52h (~81%) | 52h (~81%) | 52h (~81%) | **208h** | **~81%** |
| AGENT | 40h | 8h | 8h | 24h | **80h** | — |
| **Total Planned** | **~196h** | **~188h** | **~188h** | **~204h** | **~776h** | |
| **Lead Buffer** | +12h | +12h | +12h | +12h | **+48h** | _contingency_ |

---

## Section 3 — Story Point Distribution

| Resource | Stories Owned | Total SP | % Share |
|---|---|---|---|
| DEV-1 | S1-01~02, S1-09, S1-12, S1-14, S2-01~02, S2-05, S2-07, S2-10, S3-01, S3-03, S3-05, S3-07, S3-09, S4-01, S4-03, S4-05~06, S4-09, S4-12 | 91 SP | **46%** |
| DEV-2 | S1-03~04, S1-10~11, S1-14, S2-03~04, S2-06, S2-08, S2-11, S3-02, S3-04, S3-06, S3-08, S3-10, S4-02, S4-04, S4-05~06, S4-08, S4-11~12 | 85 SP | **43%** |
| LEAD-1 | S1-05~08, S2-09, S3-12, S4-07~08, S4-11, S4-13 | 24 SP | **12%** |
| AGENT | S1-13, S4-10 (documentation) | – (not SP-counted) | Documentation |
| **Effective Team Total** | | **200 SP** | |

*Note: Stories with multiple owners (integration tests) are counted once in primary owner's column.*

---

## Section 4 — Reuse Impact on Utilization

### How 0-Effort Stories Freed Sprint 1 Capacity

Without the 4 reuse stories (S1-01 through S1-04), Sprint 1 would have consumed 66h of DEV time just on REFramework + Config + Common + WorkQueue setup. With reuse, those 4 stories take a combined 8h (import only), freeing 58h of DEV capacity in Sprint 1.

**How that capacity was redeployed:**

| Freed From | Hours Freed | Redeployed To | Value Delivered |
|---|---|---|---|
| REFramework clone + Config.xlsx (0-effort) | 22h | S1-09 MCH Login Library | Full MCH login/logoff done by end of Sprint 1 |
| 4 NuGet imports (0-effort) | 2h | S1-12 MasterScheduler Library | 3 of 8 MasterScheduler workflows complete in Sprint 1 |
| Common workflows import (0-effort) | 30h | S1-10 MCH BGMM/BGIR | BGMM and BGIR screens started in Sprint 1 instead of Sprint 2 |
| WorkQueue wrappers import (0-effort) | 26h | S1-11 Database Library + S1-14 Unit Testing | Database Library fully complete and tested in Sprint 1 |
| **Total** | **80h freed** | **All redeployed** | **Sprint 1 delivers equivalent of 1.5 sprints of new build** |

**Net effect:** Sprint 1 produces the project scaffold **plus** MCH Login/Logout, BGMM/BGIR start, a complete Database Library, partial MasterScheduler Library, and BRD/TDD sign-off — in 2 weeks. Without reuse, this would have required 3 weeks.

### Sprint-Level Reuse Benefit Summary

| Sprint | 0-Effort Value Absorbed | Impact |
|---|---|---|
| Sprint 1 | 160h saved (imports + REFramework) | Sprint 1 completes MCH Login and DB Library that would otherwise require Sprint 2 |
| Sprint 2 | 0 (all new build) | Full sprint on highest-complexity MCH posting screens; no context switching |
| Sprint 3 | 0 (all new build) | Full sprint on Posting Library — most complex component — with no dependencies creating delays |
| Sprint 4 | ~8h (Adhoc/NOVAL pattern) | Integration testing allocated 16h extra; exception regression testing expanded from 40 to 60+ test scenarios |
