# Agent-to-Agent Public Procurement Negotiation for the Estonian Government

**Azure Master Architect · EMEA**

FY27 Cohort 2 · Agentic Case Study Compendium · Case 16

| | |
|---|---|
| **Industry** | Public Sector (Procurement / Digital Government) |
| **Headquarters** | Estonia |
| **Operating region** | Estonia, with Nordic-Baltic e-procurement interoperability |
| **Regulatory context** | EU Public Procurement Directives · eForms · X-Road · GDPR · EU AI Act |

## Business Challenge

Estonia reported 9,137 published procurements nationwide in 2025. Below-threshold purchases and framework call-offs are distinct workflows, with different coverage and legal constraints. The proposed system explores machine-readable procurement where buyer and supplier agents support permitted work within legally defined parameters. Whether this reduces effort or attracts more SME participation remains to be tested.

## Transformation Objective

Create a framework in which a government buying agent publishes structured requirements, supplier agents respond and clarify, agents negotiate within rule-bounded envelopes, and the evaluation agent produces a legally compliant award recommendation for the human procurement officer.

## Agentic Architecture Brief

| Agent | Owns end-to-end | Autonomy level |
|---|---|---|
| Requirement Structuring Agent | Turns need into eForms-compliant machine-readable specification | L3 — acts, officer approves |
| Buyer Negotiation Agent | Runs clarification and negotiation within statutory rules | L3 — acts within envelope |
| Supplier Agent SDK | Reference agent SMEs can adopt to respond and negotiate | external |
| Evaluation Agent | Scores offers against published criteria with full traceability | L2 — recommend |
| Audit & Transparency Agent | Publishes negotiation logs and award rationale | L4 — autonomous |

**Orchestration:** Protocol-based A2A interaction (MCP/A2A) over X-Road with signed messages; statutory rules encoded as a negotiation envelope that neither agent may exceed; officer signs the award.

**Governance, safety and evaluation:** Equal treatment and transparency principles enforced by protocol; complete logs available to bidders and courts; EU AI Act rules for public-sector AI; open-source supplier SDK.

## Expected Outcomes

The hypothesis is that agent assistance can reduce buyer effort and, for published tenders, time to signature versus post-reform procurement without agents, while meeting procurement safeguards. These are outcomes to measure, not promised reductions. SME participation, competition and disputes are tracked separately; no numeric improvement is assumed.

## Why Now (2026–2028)

The Interoperable Europe Act (in force since April 2024) mandates interoperability assessments and supports regulatory sandboxes; Estonia's X-Road and eForms already make procurement machine-readable, so agent-to-agent negotiation can be piloted now.

## Business Case

The OECD reports public procurement at 15.3% of Estonia's GDP in 2023. This is historical context, not the programme's eligible spend or an estimate of agent benefits. The claim that a 2% efficiency gain would exceed EUR 30 million a year is not established.

The current value model is a planning thesis, not a validated national investment case. It compares agent-assisted work with post-reform procurement without agents. Its in-scope streams are buyer productivity, publication-to-signature time for published tenders, and specifically evidenced public cost savings. Officer time freed is capacity, not cash. Contract prices may be measured and analysed, but projected purchase-price savings are excluded. The 20%, 35% and 50% effort cases and three-, five- and seven-year adoption paths are unweighted planning scenarios, not forecasts.

Programme costs, payback and NPV are not yet established. The sponsor and audience are to be confirmed; the Centre of Registers and Information Systems is not the procurement authority. See the [current business-case model and evidence](business-case-model.md) for the decisions, assumptions, sources and model status.

**Capstone artifact:** alongside the architecture, produce one thought-leadership artifact — a patent disclosure, a white paper, a reference architecture, or a recorded podcast episode. Confirm your choice with your mentor by week three.

---

*Source images: [Page 1](images/case-study1.png) · [Page 2](images/case-study2.png).*

*Microsoft Azure · EMEA Solution Architecture · September 2026*
