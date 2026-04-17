# 112 MDS OCFC — Honest Timeline Assessment

| Field | Value |
|---|---|
| Document ID | 112-TA-001 |
| Project | MDS.112 – COB MDS |
| Assessment Date | 2026-04-13 |
| Aligned With | 112_Project_Plan.md Rev 2026-04-13 |

---

## Section 1 — Planned vs. Realistic Timeline

### 1.1 Planned Timeline

**Committed: 12 weeks / 6 sprints / ~1,254 hours**

Why it is achievable:
1. **148h of reuse savings** from 058 — MCH Core (7 WF), Logging, LoginAgent, Environment, Common Workflows, WorkQueue Wrappers, REFramework, Exception Strategy all at 0 effort
2. **Full team from Day 1** — no ramp-up; DEV-1 and DEV-2 at 80% (64h/sprint), LEAD at 81% (52h/sprint planned, 12h buffer)
3. **Agent scaffolding** reduces manual XAML creation by ~150h — Dev imports and wires selectors
4. **Sprint-over-sprint pattern reuse** (~110h internal savings) — MCH, RKS, HTTP, business logic patterns compound
5. **3 critical spikes in Sprint 1** (MCH, MDP ForgeRock, RKS) de-risk the most complex integrations early

| Sprint | Weeks | Focus | ~Hours |
|---|---|---|---|
| Sprint 1 | 1–2 | Foundation + Infrastructure | 220h |
| Sprint 2 | 3–4 | MCH + HTTP/API + ESP | 202h |
| Sprint 3 | 5–6 | RKS + Web Objects | 198h |
| Sprint 4 | 7–8 | Business Logic + Sentinel | 200h |
| Sprint 5 | 9–10 | Process Integration + Testing | 204h |
| Sprint 6 | 11–12 | UAT + Deployment + Closure | 230h |
| **TOTAL** | **12** | | **1,254h** |

### 1.2 Realistic Timeline

| # | Risk Factor | Additional Time | Confidence % |
|---|---|---|---|
| 1 | RKS Citrix selector instability (35 workflows, 3 launch modes) | +1 week | 60% |
| 2 | MDP ForgeRock auth complexity exceeding estimate | +0.5 week | 70% |
| 3 | 3019/3334 business logic regression (16 scenarios) | +0.5 week | 65% |
| 4 | RKS DaaS login / Citrix workspace delays | +0.5 week | 50% |
| 5 | Defect resolution during UAT | +0.5 week | 75% |
| 6 | CyberArk robot account provisioning delays | +0.5 week | 40% |
| **Realistic Buffer** | | **+2 weeks** | |
| **Realistic Total** | | **14 weeks / 7 sprints / ~1,434h** | |

> **Highest density sprint:** Sprint 3 (RKS + Web Objects) — 35 RKS workflows with Citrix/browser selectors, 4 web objects with 2FA. This is the critical path sprint. If selectors are unstable, it could expand from 2 weeks to 3 weeks.

---

## Section 2 — Complexity Analysis

### 2.1 Complexity Factors

| Factor | Complexity Rating | Why |
|---|---|---|
| Process count (33) | **High** | 33 processes with up to 35 subsheets each — substantially more than 058 (7) or 057 (8) |
| Object count (101) | **Very High** | 101 objects with 1,100+ total actions — largest in portfolio |
| External systems (14) | **Very High** | 14 distinct systems with 12 credential domains, 5 with 2FA |
| Queue model (26 queues) | **High** | 26 separate queues with different retry configs, deferred processing |
| RKS automation (3 modes) | **Very High** | Citrix + HTTP + Edge — triple launch mode with ~35 screen workflows |
| MCH screens (BFSE/BSEC/BGOV) | **Medium** | 9 new terminal workflows — pattern from 058 MCH Core helps |
| Business logic (3019/3334/3077/3122/3917) | **High** | 32 logic workflows with 16+ coupon type scenarios |
| HTTP/API integration (9 APIs) | **Medium** | Standard REST/SOAP pattern — but ForgeRock/SecurID adds complexity |
| Reuse source (058) | **Helpful** | 148h saved — reduces foundation effort significantly |
| DB operations (locking, retry) | **Medium** | Custom locking mechanism with concurrent access handling |

### 2.2 Component Breakdown by Effort

| Area | Workflow Count | % of Total Build Effort | Sprint |
|---|---|---|---|
| RKS Library | 35 | 28% | Sprint 3 |
| Business Logic | 32 | 22% | Sprint 4 |
| HTTP/API Libraries | 9 | 10% | Sprint 2 |
| MCH 112-Specific | 9 | 8% | Sprint 2 |
| ESP Library | 3 | 7% | Sprint 2–3 |
| Web Objects | 4 | 7% | Sprint 3 |
| Infrastructure/DB | 7 | 6% | Sprint 1 |
| Sentinel + Orchestrator | 2 | 5% | Sprint 4 |
| Process Integration | 33 processes | 7% | Sprint 5 |
| **TOTAL** | ~134 workflows | **100%** | |

---

## Section 3 — What Could Go Wrong

### 3.1 Risk Register

| # | Risk | Probability | Impact | Worst Case Delay | Mitigation |
|---|---|---|---|---|---|
| 1 | RKS Citrix selectors break across sessions/builds | 70% | High | +2 weeks | Build all 3 launch modes in S3 (S3-02/03/04); selector tuning allocation in S3 buffer; Lead reviews every RKS selector in S3-18 |
| 2 | MDP ForgeRock authentication flow doesn't work in UiPath HTTP | 40% | High | +1 week | Sprint 1 spike (S1-23) validates by Week 2; SecurID fallback ready; Lead architects alternative in S1-21 |
| 3 | 3019/3334 business logic regression — coupon type scenarios produce wrong results | 50% | High | +1 week | Agent generates scenario test matrix in S4; DEV-2 validates each scenario against BP output in S4-16; Lead reviews in S4-15 |
| 4 | ESP ODBC connection fails from robot machine | 30% | Medium | +0.5 week | Sprint 1 connectivity test; HTTP fallback architecture in TDD; DEV-2 tests in S2-18 |
| 5 | CyberArk robot account not provisioned on time | 50% | High | +1 week | Request submitted before Sprint 1; escalation path defined; use dev credentials as fallback for spikes S1-22/23/24 |
| 6 | 2FA token timing issues (ERP/GTM) — token expires mid-automation | 40% | Medium | +0.5 week | Configurable retry delays in S3-14/15; Lead tests timing in S3-20 integration test |
| 7 | DB locking deadlocks under concurrent performers | 20% | High | +0.5 week | Unit test locking in S1-15; concurrent test in S5-10 with 2 performers; lock timeout configurable |
| 8 | Robot machine network access blocked to one or more systems | 40% | High | +1 week | Submit all network requests before Sprint 1; validate in S1 spikes; escalate by Week 2 if blocked |
| 9 | Parallel run reveals unexpected BP-vs-UiPath output differences | 60% | Medium | +1 week | Budget bug fix time in S6-05 (8 SP); parallel run in S6-02; rollback procedure ready |
| 10 | RKS DaaS login workspace pop-up handling varies by environment | 50% | Medium | +0.5 week | Configurable wait times (MDS_OCFC_RKS CITRIX WORKSPACE LAUNCH POP UP WAITING TIME); S3-04 UIA mode handles variations |

### 3.2 Critical Path Sprint

**Sprint 3 (RKS + Web Objects)** is the critical path sprint:
- 35 RKS workflows — most depend on Citrix/browser selectors that can only be captured in the live environment
- RKS_Wrapper alone has 46 actions — the largest single workflow
- If RKS selectors are unstable → entire Sprint 4+ is delayed (Business Logic depends on RKS_Wrapper)

**Mitigation if Sprint 3 slips:**
1. LEAD pivots from code review to RKS selector troubleshooting — uses 12h contingency buffer
2. Business Logic development (Sprint 4) starts with non-RKS workflows first (3019/3334 logic → pure data comparison, no RKS)
3. RKS screen workflows batched — complete highest-priority screens (CrossReference, FactorHistory, FixedIncome) first
4. If 1+ week slip → compress Sprint 6 testing scope to critical-path processes only

---

## Section 4 — What Will Definitely Take Longer Than Expected

1. **RKS_Wrapper.xaml (46 actions)** — Planned: 24h in Sprint 3 (S3-08). Realistic: 32h. The wrapper orchestrates all RKS sub-objects, handling window state management, screen transitions, and session recovery. The size and complexity guarantee debugging time. **Plan for 32h, not 24h.**

2. **MDP ForgeRock Authentication** — Planned: 16h in Sprint 2 (S2-10). Realistic: 24h. ForgeRock OAuth2 flows with token refresh are notoriously tricky in HTTP activities. Multi-step auth (access token → resource server → API call) adds debugging time. **Plan for 24h — start in Week 3 Day 1.**

3. **3019/3334 Scenario Testing (16 scenarios × test data)** — Planned: 12h in Sprint 4 (S4-16). Realistic: 20h. Each coupon type scenario (CORP/GOVT, MUNI, MTGE, Float, Step, Fixed-to-Float, etc.) needs specific test data that must be constructed or found in DEV. **Plan for 20h of test data preparation + execution.**

4. **RKS Selector Capture (35 screen workflows)** — Planned: distributed across Sprint 3 builds. Realistic: +16h total. Each screen has unique element hierarchies in the Citrix session. First selector capture will take 2h per screen; subsequent similar screens ~1h. 35 screens × 1.5h average = 52h vs. planned 36h. **Plan for 52h of total selector work.**

5. **PROD Deployment (26 queues + 272 assets + 12 credentials)** — Planned: 20h in Sprint 6. Realistic: 28h. Creating 272 Orchestrator assets manually is tedious and error-prone. Each credential requires CyberArk safe mapping verification. **Plan for 28h — consider automation script for asset creation.**

---

## Section 5 — What Will Go Faster Than Expected

1. **058 Library Imports (Sprint 1)** — The 8 import stories are pure copy-paste operations. 058 is proven and tested. Sprint 1 will have ~32h of genuinely freed capacity from these 0-effort stories that is already redeployed to DB infrastructure and spikes.

2. **Agent XAML Scaffolding** — Agent generates complete workflow scaffolds with activities, arguments, Try/Catch, and logging. This saves ~2–4h per workflow vs. manual Studio creation. Across 100+ workflows = ~150h saved. DEV only needs to add selectors and test.

3. **HTTP API Libraries after MDP (Sprint 2)** — Once MDP_HTTP (the most complex API with ForgeRock) is working, the remaining 8 HTTP APIs follow an identical pattern. SSCD, RMG, ERP, RDOC each take 4–6h instead of 8h = ~20h saved.

4. **LEAD as Active Developer (52h/sprint)** — LEAD is not just reviewing — actively performing integration testing, architecture decisions, and deployment. This prevents the common bottleneck of "all code done, waiting for review." Code review happens concurrent with build.

5. **RKS Screen Workflows After First 3 (Sprint 3)** — After CrossReference (6h), FactorHistory (6h), and FixedIncome (6h), the pattern is established. Remaining 25 screens take 3h each vs. 6h = ~75h saved vs. building each from scratch.

---

## Section 6 — Honest Effort Estimate

### 6.1 Bottom-Up Estimate

| Component | Optimistic (hrs) | Realistic (hrs) | Pessimistic (hrs) | Sprint |
|---|---|---|---|---|
| Foundation + Imports | 40h | 50h | 60h | S1 |
| DB Infrastructure (Interactions + Locking) | 20h | 28h | 36h | S1 |
| Infrastructure (Mail, Triggering, Utilities) | 28h | 36h | 44h | S1 |
| Spikes (MCH, MDP, RKS) | 12h | 18h | 24h | S1 |
| MCH 112-Specific (9 WF) | 32h | 44h | 56h | S2 |
| HTTP/API Libraries (9 WF) | 48h | 64h | 80h | S2 |
| ESP Library (3 WF) | 32h | 44h | 56h | S2–3 |
| RKS Library (35 WF) | 120h | 160h | 200h | S3 |
| Web Objects (4 WF) | 28h | 40h | 52h | S3 |
| Business Logic (32 WF) | 80h | 108h | 136h | S4 |
| Sentinel + Orchestrator | 28h | 40h | 52h | S4 |
| Process Integration (33 processes) | 48h | 64h | 80h | S5 |
| Testing (unit + integration + exception) | 64h | 88h | 112h | S5 |
| UAT + Parallel Run | 32h | 48h | 64h | S6 |
| Deployment + Go-Live | 28h | 40h | 52h | S6 |
| Documentation + Closure | 32h | 40h | 48h | S1+S6 |
| Reuse from 058 | **0h** | **0h** | **0h** | S1 (148h saved) |
| **TOTAL** | **692h** | **912h** | **1,152h** | |

> Note: Bottom-up estimates above are DEV+LEAD effort only. Adding Agent time (174h) and LEAD buffer (72h) reconciles with the committed plan total.

### 6.2 Three-Point Estimate (PERT)

$$E = \frac{O + 4M + P}{6} = \frac{692 + 4(912) + 1152}{6} = \frac{692 + 3648 + 1152}{6} = \frac{5492}{6} = 915h$$

PERT estimate: **~915h** (human effort)

Committed plan human effort: **1,080h** (DEV-1 384h + DEV-2 384h + LEAD 312h)

Gap: 1,080h - 915h = **+165h** (18% above PERT)

**Explanation of gap:** The committed plan allocates 64h/sprint to each DEV and 52h/sprint to LEAD as utilization targets. The PERT estimate is task-driven. The 165h gap represents: (a) sprint ceremonies (18h total), (b) rework buffer (~60h at 10% per sprint), (c) code review time embedded in LEAD hours (~48h), (d) context-switching and coordination overhead (~40h). This is healthy — the plan has built-in resilience without explicit "buffer" rows.

### 6.3 Timeline Translation

| Scenario | Hours | Calendar (3 people × 180h/sprint + Agent) | Weeks |
|---|---|---|---|
| Best case | 1,100h | 5 sprints | 10 weeks |
| **Committed Plan** | **1,254h** | **6 sprints** | **12 weeks** |
| Realistic | 1,434h | 7 sprints | 14 weeks |
| Pessimistic | 1,600h | 8 sprints | 16 weeks |

### 6.4 Recommendation

**Hold the committed plan of 12 weeks / 6 sprints / 1,254h.** The plan has sufficient resilience through LEAD's 12h/sprint contingency buffer and the 18% margin above PERT. The specific risk that would trigger a revision is: **if RKS Citrix automation is not functioning by end of Sprint 3 (Week 6)**. If the RKS library is not at least 70% complete by Week 6, extend the plan by 1 sprint (2 weeks) to 14 weeks. This is observable and binary — check at the Sprint 3 demo.

---

## Section 7 — Key Recommendations

1. **Submit all CyberArk and network access requests before Sprint 1 Day 1** — Robot account provisioning (12 domains) and network access (14 systems) are the #1 blocker. LEAD must own this and escalate by end of Week 1 if any are missing.

2. **Execute all 3 spikes (MCH, MDP ForgeRock, RKS) in Sprint 1 Week 2** (S1-22, S1-23, S1-24) — These spikes validate the 3 riskiest integrations. If any spike fails, architecture pivots happen in Sprint 1 rather than Sprint 3.

3. **Build RKS_LaunchHTTP first, not Edge or UIA** (S3-02) — HTTP mode has the simplest authentication and no Citrix dependency. Once HTTP works, Edge and UIA add incremental complexity. If HTTP fails, the whole RKS approach needs reassessment.

4. **Batch RKS screen selector capture into 2-day sessions** — Rather than capturing selectors one-at-a-time, DEV-1 should batch-capture all RKS screen selectors in focused sessions (e.g., Morning: Fixed Income group, Afternoon: End Date group). This maintains Citrix session state and avoids repeated login overhead.

5. **Test 3019/3334 scenarios on paper before coding** (Sprint 4, before S4-01) — The 16 coupon type scenarios are the most complex business logic. DEV-2 should trace each scenario through the BP source and document input→output before writing any UiPath code. Agent generates the trace template.

6. **Automate Orchestrator asset creation for PROD** (Sprint 6, S6-06) — Creating 272 assets manually is error-prone. Agent generates a PowerShell script (Orchestrator API) to create all assets from Config.xlsx. DEV-1 runs the script, saving ~8h and eliminating typos.

7. **Schedule UAT validation sessions with Business Owner in Sprint 5** (before Sprint 6) — Don't wait until Sprint 6 to discover UAT availability. LEAD schedules 3–4 sessions in Sprint 5 so any feedback informs Sprint 6 bug fixes immediately.

8. **Keep ESP ODBC fallback to HTTP architecture ready** — If ESP ODBC fails from the robot machine, the TDD already has ESP HTTP endpoints mapped. DEV-2 should have the HTTP alternative scaffolded (by Agent) before committing fully to ODBC.

---

## Section 8 — Summary

| Metric | Value |
|---|---|
| Committed Timeline | 12 weeks / 6 sprints / 1,254h |
| Realistic Timeline | 14 weeks / 7 sprints / ~1,434h |
| Pessimistic Timeline | 16 weeks / 8 sprints / ~1,600h |
| Recommended Commitment | **12 weeks — hold** (review if RKS not 70% by Week 6) |
| PERT Estimate | 915h human effort + 174h Agent = 1,089h |
| Reuse Savings | 148h from 058 (MCH Core, Logging, LoginAgent, Environment, REF, Common, WQ, Exception Strategy) |
| Biggest Risk | RKS Citrix automation instability (35 workflows, 3 launch modes) |
| Second Biggest Risk | MDP ForgeRock authentication complexity |
| Biggest Accelerator | 058 library reuse (148h at 0 effort) + Agent scaffolding (150h saved) |
| Confidence Level | **75%** — achievable if RKS and MDP ForgeRock validated by Week 4 |
| Pioneer Value | RKS Library (35 WF), HTTP template, ESP Library, Business Logic patterns — saves ~420h per future project |
