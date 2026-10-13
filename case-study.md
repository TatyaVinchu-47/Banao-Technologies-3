# Case Study: Nesting Optimization for Structural Steel Fabrication

**Client:** Structural Steel Fabricator  
**Prepared by:** Forward Deployed Engineer  
**Date:** October 2026

---

## 1. Problem Understanding

### What the Client Asked For
The client requested AI integration across multiple operational areas: nesting optimization, drawing revision tracking, warehouse/yard storage, trailer loading plans, and equipment utilisation. They explicitly prioritised nesting as their first choice.

### What the Client Actually Needs
The client's core problem is **visibility, not optimization**. They cannot answer basic questions about their own operations:
- How much steel is actually wasted vs. theoretically preventable?
- Which offcuts exist and could be reused?
- How much loss comes from client revisions vs. internal engineering errors?

Before optimizing anything, they need instrumented data capture. The nesting team claims manual nesting beats automatic—but without data, no one can verify this. The client is asking for AI optimization when they first need a measurement system.

**The gap:** They want to jump to optimization; they need to start with baseline measurement and data infrastructure.

### The As-Is Process

```
Business Dev → Estimation → Planning → Engineering → Nesting → Factory → Dispatch
     ↓              ↓            ↓           ↓            ↓         ↓
   Bids         Pricing     Schedule    Drawings    Cut Plans   Cutting/
                                                    (Strumis)   Fab/Coat
```

**Nesting today:**
1. Engineering releases fabrication drawings
2. Material arrives in full sizes (plates, profiles)
3. Nesting team manually plans cuts in Strumis
4. Plans go to CNC machines for physical cutting
5. Offcuts recorded manually; checked ad-hoc for reuse on new projects

**Key constraints:**
- Nesting is done per-project (cost accounting requirement)
- Fixed budget given at project start; changes absorbed
- Material costs loaded to specific project codes
- Offcut reuse exists conceptually but is poorly executed

### Stakeholders and Their Interests

| Stakeholder | What They Want | What They Need to Hear |
|-------------|----------------|------------------------|
| **Cost Control (our contact)** | Data to report on wastage, attribution of losses | "We'll give you the numbers you can't get today" |
| **Management** | Reduce material cost, stop hiring extra trailers | "X% reduction in wastage = $Y savings per year" |
| **Nesting Team** | Validation that their manual work is valuable; not to be replaced | "We're measuring performance, not replacing people" |
| **Engineering** | Fewer revision headaches, less blame | Not directly in scope for Phase 1 |
| **Purchasing** | Predictable material requirements | Future benefit from better data |

---

## 2. What We Don't Know Yet

### Ranked Questions for Next Call

| # | Question | Why It Matters | Assumption Until Answered |
|---|----------|----------------|---------------------------|
| 1 | **Can we get historical nesting data from Strumis—cut plans, plate utilisation rates, offcut records—for the past 6-12 months?** | Determines if we can establish a baseline without manual data collection. No data = no before/after comparison. | Strumis has exportable data for at least 6 months. If not, Phase 1 extends by 2-3 weeks for manual baseline. |
| 2 | **What does the nesting team's workflow look like hour-by-hour? How long does a typical nest take? What information do they reference?** | Understanding the human process is essential before augmenting it. Their claim that manual beats auto may be true for complex shapes. | A single plate nest takes 15-30 minutes. Nesters reference drawings + material inventory + offcut list. |
| 3 | **Can we shadow the nesting team for 2-3 days?** | Ethnographic observation reveals unstated constraints and workarounds. The nesting team wasn't on the calls—we haven't heard their perspective. | Access will be granted. If not, we rely on secondhand descriptions and risk missing critical details. |
| 4 | **How is "wastage" currently defined and measured? Is there a difference between planned scrap, unplanned scrap, and offcuts awaiting reuse?** | The 20-25% plate wastage figure may conflate different categories. Optimization strategy differs for each. | Wastage = (material purchased - material in finished product) / material purchased. No distinction between scrap types today. |
| 5 | **What is the commercial model for material cost recovery on client revisions vs. internal errors? Who decides which is which?** | Determines if "error attribution" is a technical problem or a political one. May need contractual/process changes, not just data. | Engineering marks revisions as client-driven or internal. Currently no systematic tracking. |

### Secondary Questions (if time permits)
- What format are the fabrication drawings in? (DXF, DWG, PDF?)
- How are offcuts physically labelled and stored?
- What's the typical lag between material arrival and nesting?

---

## 3. Solution

### Options Considered

| Option | Description | Pros | Cons | Verdict |
|--------|-------------|------|------|---------|
| **A. AI-driven auto-nesting replacement** | Replace manual nesting with ML-optimised cutting plans | Maximum theoretical savings | High risk (nesting team resistance, untested claims), no baseline data, long timeline | **Rejected for Phase 1** |
| **B. Data instrumentation + baseline measurement** | Instrument Strumis exports, build dashboard, establish wastage baseline by project/material type | Low risk, answers fundamental questions, builds trust | No immediate optimisation | **Recommended Phase 1** |
| **C. Offcut matching system** | Build a system to match offcuts to upcoming cuts across projects | Quick win if offcuts are underutilised | Requires cross-project cost allocation (client unsure if feasible) | **Phase 2 candidate** |
| **D. Full ERP integration + yard optimisation** | Address storage, equipment, and nesting together | Comprehensive | Too broad, too expensive, too slow for first engagement | **Rejected** |

### Recommended Approach: Phased Instrumentation

**Phase 1: Measure** — Build the data foundation and establish baseline  
**Phase 2: Match** — Offcut reuse optimisation (if baseline justifies)  
**Phase 3: Optimize** — Nesting algorithm augmentation (if Phase 2 successful)

### What We Deliberately Won't Build

1. **Auto-nesting AI in Phase 1** — No baseline means no way to prove it works
2. **Drawing revision tracker** — Different problem, different stakeholders, dilutes focus
3. **Yard/equipment optimisation** — They already have software (CarTrack, booking system) they haven't fully exploited; not our problem to solve
4. **Cross-project material sharing** — Client unsure of cost allocation feasibility; park until confirmed

---

## 4. Solution Engineering

### Phase 1: Baseline Measurement System

#### Inputs
| Input | Source | Format | Frequency |
|-------|--------|--------|-----------|
| Cut plans | Strumis export | CSV/XML (TBC) | Per project completion |
| Material purchased | Strumis/ERP | CSV | Per project |
| Offcut inventory | Strumis or manual | Spreadsheet | Weekly snapshot |
| Finished product weights | Dispatch records | CSV | Per shipment |

#### Data Pipeline

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐     ┌────────────┐
│   Strumis   │────▶│  ETL Script  │────▶│  Data Lake  │────▶│ Dashboard  │
│   Exports   │     │  (Python)    │     │ (PostgreSQL)│     │ (Metabase) │
└─────────────┘     └──────────────┘     └─────────────┘     └────────────┘
                           │
                    ┌──────▼──────┐
                    │  Wastage    │
                    │  Calculation│
                    │  Engine     │
                    └─────────────┘
```

#### Metrics Calculated
- **Utilisation rate** = Nested area / Plate area (per plate, per project, per material type)
- **Offcut reuse rate** = Offcuts consumed / Offcuts generated
- **Scrap rate** = Material scrapped / Material purchased
- **Attribution** = Client revision loss vs. Internal error loss (requires tagging input)

#### Human Touchpoints
1. **Nesting team** — No change to workflow; data pulled from existing Strumis outputs
2. **Cost Control (our contact)** — Reviews weekly dashboard, validates calculations
3. **Engineering** — Tags revisions as client-driven or internal (new 30-second step)

#### Integration with Existing Systems
- **Strumis** — Read-only export; no changes to their ERP
- **No new software for nesting team** — Invisible to their daily work
- **Dashboard access** — Web-based, view-only for management

### Phase 2: Offcut Matching (Contingent on Phase 1 Data)

If Phase 1 reveals significant offcut underutilisation (>30% of offcuts never reused), we build:

1. **Offcut registry** — Catalogued by dimensions, material grade, location
2. **Matching engine** — When new cut plan created, query offcuts that fit
3. **Suggestion interface** — Show nesting team "Offcut X in Yard Row 3 fits Cut Y"

This requires solving the cross-project cost allocation question with Finance.

---

## 5. Evaluation

### Baseline Metric
**Primary:** Material utilisation rate (%) = Nested material / Purchased material  
**Secondary:** Offcut reuse rate (%) = Offcuts consumed / Offcuts generated

### Current State (from client)
- Plate wastage: 20-25%
- Profile wastage: 5-10%
- Overall: 7-10%

### Success Threshold
Phase 1 is **successful** if we can reliably measure utilisation rate within ±2% accuracy, validated against 3 completed projects.

Phase 2 (if pursued) targets: **5 percentage point improvement** in offcut reuse rate within 6 months.

### Test Protocol
| Test | Method | Who Measures | Duration |
|------|--------|--------------|----------|
| Data accuracy | Compare system-calculated wastage vs. manual calculation for 3 historical projects | FDE + Cost Control | Week 4-5 |
| Dashboard reliability | System uptime, data freshness (target: <24hr lag) | FDE | Ongoing |
| User adoption | Weekly active users, report downloads | FDE | Week 6+ |

### Stop Criteria
We **stop or pivot** if:
1. Strumis data is too incomplete to calculate utilisation (<70% of projects have usable data)
2. Calculated wastage differs from manual calculation by >5% after reconciliation
3. Client cannot provide engineering time to tag revision attribution
4. After 8 weeks, no one is using the dashboard

---

## 6. Build Plan and Timeline

### Phase 1: Baseline Measurement (8 weeks)

| Week | Milestone | Deliverable | Client Dependency |
|------|-----------|-------------|-------------------|
| 1 | Kickoff + data access | Strumis credentials, sample exports | Client IT grants access |
| 1-2 | Nesting team shadowing | Process documentation | Nesting team availability |
| 2-3 | Data schema + ETL | Working data pipeline | None |
| 4 | Historical data load | 6-12 months of data in system | Data export from client |
| 5 | Dashboard v1 | Basic utilisation metrics visible | None |
| 5-6 | Validation | Manual vs. system comparison on 3 projects | Cost Control reviews |
| 7 | Iteration | Fixes based on validation | None |
| 8 | Go-live + training | Dashboard in production, training complete | 2-hour training session |

### Client Dependencies (Critical Path)
1. **Week 1:** Strumis access credentials
2. **Week 1-2:** Nesting team availability for shadowing (2-3 days)
3. **Week 4:** Historical data export
4. **Week 5-6:** Cost Control time for validation (4-6 hours)
5. **Week 8:** Stakeholder time for training (2 hours)

### Phase 2: Offcut Matching (Estimated 6-8 weeks, TBD after Phase 1)

Requires:
- Phase 1 data showing offcut underutilisation
- Finance confirmation on cross-project cost allocation
- Physical offcut labelling system (may need barcode/RFID)

---

## 7. Cost

### Our Delivery Cost

| Role | Hours (Phase 1) | Rate | Cost |
|------|-----------------|------|------|
| Forward Deployed Engineer | 160 hrs (8 wks × 20 hrs) | $150/hr | $24,000 |
| Data Engineer | 80 hrs | $120/hr | $9,600 |
| Technical oversight | 16 hrs | $200/hr | $3,200 |
| **Total Labour** | | | **$36,800** |

| Infrastructure | Monthly | Duration | Cost |
|----------------|---------|----------|------|
| Cloud hosting (PostgreSQL + Metabase) | $200 | 3 months | $600 |
| **Total Infrastructure** | | | **$600** |

| **Total Phase 1 Delivery Cost** | | | **$37,400** |

### Proposed Pricing to Client

| Component | Price |
|-----------|-------|
| Phase 1: Baseline Measurement System | $55,000 |
| Monthly support (post go-live) | $2,500/month |
| Phase 2: Offcut Matching (estimate) | $40,000-60,000 |

**Payment Structure:**
- 40% on contract signature ($22,000)
- 30% on dashboard v1 delivery ($16,500)
- 30% on validation sign-off ($16,500)

**Margin:** ~32% on Phase 1

### Value to Client

**Conservative scenario:** 1 percentage point reduction in overall wastage

- Annual tonnage: 80,000 tonnes
- Assume average steel cost: $800/tonne
- Annual material spend: $64,000,000
- 1% savings: **$640,000/year**

**Payback period:** ~1 month of operation

Even if Phase 1 only enables better reporting (no optimization), the attribution data for client revision claims could recover significant costs currently written off.

### Assumptions
- Steel price estimated at $800/tonne (market dependent)
- FDE rate reflects senior engineer with domain access
- Cloud costs assume standard PostgreSQL + Metabase SaaS; could reduce with client-hosted
- Phase 2 estimate is rough; depends on offcut labelling complexity

---

## 8. Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| **Strumis data quality is poor** | Medium | High | Week 1-2 data audit; adjust timeline or scope if data gaps >30% |
| **Nesting team resistance** | Medium | Medium | Position as "measurement" not "replacement"; involve them in validation |
| **Cross-project cost allocation not feasible** | Medium | Medium | Phase 2 contingent; don't promise until Finance confirms |
| **Client contact lacks authority to approve** | Low | High | One-page note designed for management escalation; request exec sponsor |
| **Manual nesting actually is optimal** | Low | Low | If true, we've proven it—still valuable; pivot to offcut matching |
| **Revision attribution requires process change** | Medium | Medium | Start with engineering tagging; fall back to automated heuristics if adoption fails |

### What I Don't Trust in These Notes
1. **The 20-25% plate wastage figure** — Self-reported, no methodology explained. Could be higher or lower.
2. **"Automatic nesting wastes more"** — Nesting team claim with no data. Could be true for complex shapes; could be defensive.
3. **Offcut reuse "could be done better"** — Vague. Need to see actual reuse rates.
4. **10-15 projects running at once** — Does this mean 10-15 in nesting, or 10-15 across all stages? Affects data volume.

---

## Appendix A: Glossary

| Term | Definition |
|------|------------|
| **Nesting** | Planning how to cut required shapes from raw material plates to minimize waste |
| **Offcut** | Leftover material after cutting that may be reusable |
| **Strumis** | ERP software used by client for project management and nesting |
| **Utilisation rate** | Percentage of raw material that ends up in finished product |
| **Profile** | Structural steel shapes (I-beams, channels, angles) vs. flat plates |

---

## Appendix B: Sample Dashboard Wireframe

```
┌────────────────────────────────────────────────────────────────┐
│  NESTING PERFORMANCE DASHBOARD                    [Date Range] │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│  │ Overall     │  │ Plate       │  │ Profile     │            │
│  │ Utilisation │  │ Utilisation │  │ Utilisation │            │
│  │   92.3%     │  │   78.2%     │  │   94.1%     │            │
│  └─────────────┘  └─────────────┘  └─────────────┘            │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │  UTILISATION BY PROJECT                                  │ │
│  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░  Project A (82%)                  │ │
│  │  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░  Project B (91%)                  │ │
│  │  ▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░  Project C (65%)  ← INVESTIGATE   │ │
│  └──────────────────────────────────────────────────────────┘ │
│                                                                │
│  ┌─────────────────────────┐  ┌─────────────────────────────┐ │
│  │ OFFCUT INVENTORY        │  │ LOSS ATTRIBUTION            │ │
│  │ 142 items               │  │ Client revisions: 62%       │ │
│  │ 12.4 tonnes             │  │ Internal errors:  24%       │ │
│  │ Avg age: 34 days        │  │ Unattributed:     14%       │ │
│  └─────────────────────────┘  └─────────────────────────────┘ │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

*End of Case Study*
