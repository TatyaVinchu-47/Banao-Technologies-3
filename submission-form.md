# Submission Form — Structural Steel Case Study

---

## 1. In two sentences: what is the client's real problem, and how is it different from what they asked for?

The client's real problem is **visibility, not optimization**—they cannot measure how much steel is wasted, what portion is preventable, or which losses should be claimed back from clients versus absorbed as internal errors. They asked for AI-driven nesting optimization, but you cannot optimize what you cannot measure; they need a data foundation before any meaningful optimization is possible.

---

## 2. Who are the people in this deal, and what does each of them need to hear from you?

| Stakeholder | Role | What They Need to Hear |
|-------------|------|------------------------|
| **Cost Control Engineer (our contact)** | Data reporter, internal champion | "We'll give you the numbers you can't get today—utilisation rates, offcut inventory, and loss attribution you can report to management." |
| **Management** | Budget holder, final decision maker | "1% reduction in wastage = $640K/year savings. Payback in under a month. Low risk—we're measuring first, not replacing your team." |
| **Nesting Team** | Operators, potential resisters | "We're here to prove your manual work is valuable, not to replace you. You'll keep doing what you do; we just make your performance visible." |
| **Engineering** | Revision source, data input needed | "We need one small thing from you: tag revisions as client-driven or internal. 30 seconds per revision. This protects you from blame for client-caused losses." |
| **Finance/Purchasing** | Cost allocation gatekeepers | "If you want Phase 2 (offcut reuse across projects), we need you to confirm cross-project cost allocation is feasible. Until then, we stay within single-project boundaries." |

---

## 3. The five questions you would ask on the next call, ranked.

| Rank | Question | Why It Matters | Assumption Until Answered |
|------|----------|----------------|---------------------------|
| **1** | Can we get historical nesting data from Strumis—cut plans, plate utilisation rates, offcut records—for the past 6-12 months? | No data = no baseline = no way to prove anything works. This determines if Phase 1 is viable without extensive manual data collection. | Strumis has exportable data for at least 6 months. If not, we add 2-3 weeks for manual baseline collection. |
| **2** | What does the nesting team's workflow look like hour-by-hour? How long does a typical nest take, and what information do they reference? | The nesting team claims manual beats automatic. We need to understand why before we can validate or improve. Their perspective hasn't been heard yet. | A single plate nest takes 15-30 minutes. Nesters reference drawings + material inventory + offcut list. |
| **3** | Can we shadow the nesting team for 2-3 days? | Ethnographic observation reveals unstated constraints and workarounds that interviews miss. The nesting team wasn't on either call. | Access will be granted. If denied, we rely on secondhand descriptions and risk missing critical workflow details. |
| **4** | How is "wastage" currently defined and measured? Is there a distinction between planned scrap, unplanned scrap, and offcuts awaiting reuse? | The 20-25% plate wastage figure may conflate different categories. Optimization strategy differs for unavoidable trim waste vs. poor nesting vs. unused offcuts. | Wastage = (material purchased - material in finished product) / material purchased. No systematic distinction between scrap types today. |
| **5** | What is the commercial model for attributing losses to client revisions vs. internal errors? Who decides which is which? | If attribution is political rather than technical, we may need process changes, not just data. Determines if "loss tracking" is actually achievable. | Engineering informally marks revisions. There's no systematic tracking. We can implement tagging, but adoption is uncertain. |

---

## 4. Which options did you consider, and why did you pick the one you did?

**Options Considered:**

| Option | Description | Verdict |
|--------|-------------|---------|
| **A. AI-driven auto-nesting replacement** | Replace manual nesting with ML-optimised cutting plans | **Rejected** — No baseline data to prove improvement. Nesting team will resist. High risk, long timeline. |
| **B. Data instrumentation + baseline measurement** | Instrument Strumis exports, build dashboard, establish wastage baseline | **Selected** — Low risk, answers fundamental questions, builds trust, enables future phases. |
| **C. Offcut matching system** | Match offcuts to upcoming cuts across projects | **Phase 2 candidate** — Requires cross-project cost allocation (client unsure if feasible). Needs baseline first. |
| **D. Full ERP integration + yard/equipment optimization** | Address storage, equipment utilisation, and nesting together | **Rejected** — Too broad, too expensive, too slow. They already have underutilised software for equipment (CarTrack, booking system). |

**Why Option B:**
1. **You can't optimize what you can't measure.** The client admits they don't have data to answer basic questions about their own wastage.
2. **Low risk, high information value.** Worst case: we confirm manual nesting is actually optimal, and the data enables better client revision claims. That's still valuable.
3. **Builds trust.** A successful measurement phase earns the right to propose optimization. Jumping to AI replacement on call 3 would be irresponsible.
4. **De-risks Phase 2.** If baseline shows offcuts are underutilised, we have evidence to justify the offcut matching system.

**What I ruled out and why:**
- **Auto-nesting AI:** The nesting team says automatic wastes more. Without data, I can't prove otherwise—and if they're right, I'd look foolish.
- **Drawing revision tracking:** Different problem, different stakeholders, dilutes focus from nesting.
- **Yard/equipment work:** They have CarTrack and a booking system they admit they haven't fully exploited. That's not our problem to solve; they should use what they bought.

---

## 5. How will the client know the pilot worked?

**Primary Metric:** Material utilisation rate (%) = Nested material weight / Purchased material weight

**Baseline:** Client-reported figures:
- Plate wastage: 20-25% (utilisation: 75-80%)
- Profile wastage: 5-10% (utilisation: 90-95%)
- Overall: 7-10% (utilisation: 90-93%)

**Success Threshold for Phase 1:**
- We can calculate utilisation rate within **±2% accuracy** compared to manual calculation
- Validated against **3 completed projects**
- Dashboard operational with **<24 hour data lag**

**Test Protocol:**

| Test | Method | Who Measures | Duration | Pass Criteria |
|------|--------|--------------|----------|---------------|
| Data accuracy | Compare system-calculated wastage vs. manual calculation for 3 historical projects | FDE + Cost Control engineer | Week 4-5 | Variance <2% |
| System reliability | Monitor uptime and data freshness | FDE | Weeks 5-8 | >99% uptime, <24hr lag |
| User adoption | Track weekly active users, report downloads | FDE | Weeks 6-8 | At least 2 users accessing weekly |

**Stop Criteria (what result makes us stop):**
1. Strumis data is too incomplete to calculate utilisation (<70% of projects have usable data)
2. Calculated wastage differs from manual by >5% after reconciliation attempts
3. Engineering cannot/will not tag revision attribution
4. After 8 weeks, no stakeholder is using the dashboard

If any stop criterion is hit, we pause, diagnose, and decide whether to pivot, extend, or terminate.

---

## 6. What does this cost the client, and what does it cost us to deliver?

### Our Delivery Cost

| Role | Hours | Rate | Cost |
|------|-------|------|------|
| Forward Deployed Engineer | 160 hrs (8 wks × 20 hrs/wk) | $150/hr | $24,000 |
| Data Engineer | 80 hrs | $120/hr | $9,600 |
| Technical oversight/review | 16 hrs | $200/hr | $3,200 |
| **Labour subtotal** | | | **$36,800** |

| Infrastructure | Monthly | Months | Cost |
|----------------|---------|--------|------|
| Cloud hosting (PostgreSQL + Metabase) | $200 | 3 | $600 |
| **Infrastructure subtotal** | | | **$600** |

| **Total delivery cost** | | | **$37,400** |

### Price to Client

| Component | Price |
|-----------|-------|
| Phase 1: Nesting Performance Dashboard | **$55,000** |
| Monthly support (post go-live, optional) | $2,500/month |
| Phase 2: Offcut Matching (estimate, contingent) | $40,000-60,000 |

**Pricing Structure:** Fixed price, milestone-based payments
- 40% on contract signature: $22,000
- 30% on dashboard v1 delivery (Week 5): $16,500
- 30% on validation sign-off (Week 8): $16,500

**Gross Margin:** ($55,000 - $37,400) / $55,000 = **32%**

### Value to Client

**Assumptions:**
- Annual tonnage: 80,000 tonnes
- Average steel price: $800/tonne
- Annual material spend: 80,000 × $800 = $64,000,000

**Conservative scenario:** 1 percentage point improvement in overall utilisation
- Savings: $64,000,000 × 1% = **$640,000/year**
- Payback: $55,000 / $640,000 = **~1 month**

**Even without optimization:** Better attribution data enables claiming back losses from client revisions that are currently absorbed. If 5% of "internal" losses are actually client-caused and recoverable, that's potentially hundreds of thousands in recovered costs.

---

## 7. Timeline

**First Result the Client Sees:** Week 5 — Dashboard v1 with historical utilisation data visualised

**Full Timeline:**

| Week | Milestone | Client Dependency |
|------|-----------|-------------------|
| 1 | Kickoff + data access | Client IT grants Strumis credentials |
| 1-2 | Nesting team shadowing | Nesting team available 2-3 days |
| 2-3 | Data schema + ETL pipeline built | None |
| 4 | Historical data loaded (6-12 months) | Client provides data export |
| **5** | **Dashboard v1 live** (first visible result) | None |
| 5-6 | Validation against manual calculations | Cost Control reviews (4-6 hrs) |
| 7 | Iteration based on feedback | None |
| 8 | Go-live + training | 2-hour training session |

**Steps That Depend on Client (Critical Path):**
1. **Week 1:** Strumis access credentials — cannot start ETL without this
2. **Week 1-2:** Nesting team availability — shadowing is essential for understanding workflow
3. **Week 4:** Historical data export — need this to populate the dashboard
4. **Week 5-6:** Cost Control time (4-6 hours) — validation requires their domain knowledge
5. **Week 8:** Stakeholders available for training (2 hours)

If any client dependency slips by more than 3 days, the timeline extends accordingly.

---

## 8. Where did you push back on the client, narrow their ask, or tell them not to do something?

1. **Pushed back on jumping to AI optimization.** The client's first instinct was AI-powered nesting optimization. I narrowed this to measurement first. You cannot optimize what you cannot measure, and the nesting team's claim that manual beats automatic cannot be validated without data. Building an AI nesting system without a baseline would be irresponsible consulting.

2. **Told them not to address yard/equipment utilisation with us.** They mentioned CarTrack and a booking system they brought in six months ago and "haven't made full use of yet." That's not our problem to solve. They should use what they already bought before hiring someone to build more software. I explicitly excluded this from scope.

3. **Narrowed cross-project material sharing out of Phase 1.** The client mentioned offcuts could be used across projects, but our contact admitted "I'm not sure how feasible it is to split the same material across projects. I'd have to check with my team." Until Finance confirms cross-project cost allocation is possible, I won't build a system that requires it. This is a Phase 2 item, contingent on that confirmation.

4. **Excluded drawing revision tracking.** The client raised this as a pain point, but it's a different problem with different stakeholders (Engineering) and would dilute focus. One problem at a time.

5. **Pushed back on the assumption that the nesting team is wrong.** The client seemed to doubt their nesting team's claim that manual nesting beats automatic. Rather than take the client's side, I insisted we collect evidence before making that judgment. The nesting team may be right—complex shapes and real-world constraints may defeat algorithmic approaches. Phase 1 will tell us.

---

## 9. What is most likely to go wrong with your plan, and what in these notes did you not trust?

### Most Likely Failure Modes

1. **Strumis data quality is poor.** If historical data is incomplete, inconsistently formatted, or missing key fields, we can't establish a baseline without manual data collection—adding weeks and cost. Likelihood: Medium. Impact: High.

2. **Nesting team stonewalls.** They weren't on the calls. They may see this project as a threat to their jobs or an implicit criticism of their work. If they refuse to cooperate with shadowing or validation, we're working blind. Likelihood: Medium. Impact: Medium-High.

3. **The 20-25% wastage figure is wrong.** Self-reported, methodology unclear. If actual wastage is 10%, the savings opportunity is halved. If it's 30%, there's more opportunity but also more dysfunction to navigate. Likelihood: Medium. Impact: Medium.

4. **Cross-project cost allocation is genuinely impossible.** If Finance says no, Phase 2 (offcut matching across projects) is dead. Phase 1 is still valuable, but the growth path is limited. Likelihood: Medium. Impact: Medium (for future phases).

5. **Our contact lacks authority.** They're in Cost Control, not a decision-maker. If management doesn't buy in, the project may stall after Phase 1. Mitigation: the one-page note is designed for exactly this escalation. Likelihood: Low-Medium. Impact: High.

### What I Did Not Trust in These Notes

| Claim | Why I Don't Trust It |
|-------|---------------------|
| "Plate wastage is 20-25%" | Self-reported with no methodology. Could be an estimate, a target, or a number from one bad project. |
| "Automatic nesting wastes more" | Nesting team claim with obvious self-interest. May be true for some shapes, but "we save more by hand" is hard to verify without data they don't have. |
| "We check offcuts first" | Vague. "The idea is already there, but it could be done better" suggests the process exists on paper but not in practice. |
| "10-15 projects at a time" | Unclear if this means 10-15 in nesting specifically, or across all stages. Affects data volume estimates. |
| "7-10% overall wastage" | Suspiciously lower than the 20-25% plate figure. May be weighted by profile volume, or may be a different calculation entirely. |

---

## 10. What did you use AI for?

**Tools Used:** Claude (this conversation)

**Where It Helped:**
- Structuring the case study format and ensuring all required sections were covered
- Generating the stakeholder analysis matrix
- Creating the dashboard wireframe in ASCII
- Checking arithmetic consistency in the cost section
- Drafting the client note in appropriate non-technical language

**Where It Could Have Misled (and what I verified):**
- **Steel pricing assumptions:** I used $800/tonne as a rough estimate. In reality, structural steel prices vary significantly by grade, region, and market conditions (in 2024-2026, prices have ranged from $600-1200/tonne depending on type). The ROI calculation is illustrative, not precise.
- **Strumis capabilities:** I assumed Strumis can export the data we need. I don't actually know Strumis's export formats or data model. This would need verification on the next call.
- **Industry-standard utilisation rates:** I accepted the client's figures without external benchmarking. Industry norms for structural steel utilisation could validate or challenge these numbers.

**What I Threw Away:**
- Initial drafts that were too long and academic
- A section on "AI/ML model architecture" that was premature given we don't even know if we need ML in Phase 1
- Detailed offcut matching algorithms—that's Phase 2 design, not Phase 1 scope
- Comparisons to other nesting software products—not relevant when the focus is measurement, not tool replacement

**Honest Assessment:**
AI was useful for structuring, drafting, and checking completeness. The strategic decisions—phased approach, scope narrowing, risk identification, pushback on client assumptions—came from interpreting the discovery notes and applying consulting judgment. The AI didn't have access to nesting domain expertise or Strumis specifics, so those remain assumptions to validate.

---

*End of Submission Form*
