# Cost per procedure: buyer and supplier effort benchmarks (Estonia)

Ticket: #3 · Map: #1 · Compiled: 13 Nov 2026 · Prices: 2025 euros (Estonian labour costs)

## Answer

**Above the national threshold**, a typical Estonian procurement procedure costs about **€16,500 (range €8,900–€47,000)**. That covers the buyer and all bidders, from planning to contract signature, which is roughly **600 person-hours (range 400–1,200)**:

- The **buyer** spends about **160 h, or €4,900 (range 128–280 h; €3,100–€10,200)**.
- **Each bidder** spends about **96 h, or €2,500 (range 72–160 h; €1,500–€6,200)**.
- There are 4.4 bidders on average, so **suppliers carry about 70% of the total cost**.

A **simple procurement (lihthange or väikehange)** costs about **€5,600 (range €2,500–€18,400)**:

- The buyer spends about 70 h.
- Each bidder spends about 30 h.

Applied to the 9,137 procedures in the 2025 register, Estonia's **transaction cost is about €111M a year (range €58M–€323M)**:

- About €34M falls on contracting authorities, roughly 670 FTE.
- About €76M falls on suppliers.

This excludes mini-competitions, dynamic purchasing system (DPS) calls, contract management and litigation.

The evidence **supports** the case's claim that responding is costly for SMEs: suppliers bear most of the cost, and most of that is lost bids. The evidence **does not confirm** that clarification rounds take most of an officer's time. No primary source measures clarification separately. In the EU data, pre-award preparation (needs, specifications, documents) is the largest buyer stage, at 48–53% of effort.

## Scope and definitions

- **Procedure**: one procurement procedure registered in the Estonian public procurement register (riigihangete register). It runs from the start of planning to contract signature.
- **Excluded**:
  - contract management after signature
  - litigation (challenge rate 2.1% in 2025)
  - framework mini-competitions and DPS calls, which the register statistics do not count
  - purchases below the simple-procurement threshold
- **Procedure classes used**:

  | Class | Estonian law (in force until 31 Oct 2026) | Count 2025 |
  |---|---|---|
  | Full procedure (EU-level and national-level open/restricted/negotiated, incl. framework-award procedures) | Value ≥ national threshold (riigihanke piirmäär) | 5,444 (9,137 − 2,065 − 1,628) |
  | Simple procurement (lihthange) + utilities low-value (väikehange) | ≥ €30k goods/services, ≥ €60k works, below national threshold | 3,693 (2,065 + 1,628) |

  From **1 Nov 2026** the national threshold is abolished. Simple procurement then applies from €50k (goods/services) or €100k (works) up to the EU threshold. The Ministry of Finance estimates this moves about 49% of procedures into the simpler regime, cutting the average from about 87 days to about 50 ([Riigi Teataja summary][rt-sum]; [RT I, 03.07.2026, 3][rt-act]). **A business case for the production system must use the post-reform mix.** See open uncertainties.
- **Bidders**: Estonia averaged 4.4 bids per procedure in both 2024 and 2025. It was 3.6 in 2021 ([MoF 2025 stats][ee25], Table 1).

## Salary rates used

All rates are **employer labour cost per hour actually worked**, 2025, from Eurostat `lc_lci_lev`. That measure covers wages plus employer social contributions such as Estonia's 33% social tax and 0.8% unemployment insurance ([Eurostat API][eurostat-api]; [Eurostat news 31 Mar 2026][eurostat-news]).

| Party | Eurostat basis (EE, 2025) | Low | Base | High |
|---|---|---|---|---|
| Buyer (procurement officer, contracting authority) | NACE O, public administration: **€24.3/h** | €24.3/h (no overhead) | **€30.4/h** (+25% overhead, EC method) | €36.5/h (+50% overhead/seniority) |
| Supplier (bid team) | Business economy excl. O (B–S\_X\_O): **€21.1/h**; professional services (M): **€31.0/h** | €21.1/h (business average, no overhead) | **€26.4/h** (business avg +25%) | €38.8/h (professional services +25%) |

Cross-checks:

- Statistics Estonia puts the 2025 average gross monthly wage at **€2,092**, and **€2,662** in public administration and defence ([Statistics Estonia, 5 Mar 2026][stat]).
- €2,662 × 1.338 employer cost ÷ about 140 hours worked a month gives about €25/h, which is consistent with the €24.3/h Eurostat figure.
- The EC 2011 study used total labour cost plus a 25% overhead uplift, giving €72 a day for Estonia in 2009 ([PwC et al. 2011][pwc2011], fn. 41). The base case keeps that convention.

**[ASSUMPTION A1]** 1 person-day = 8 hours. **[A2]** 1 FTE = 1,680 hours worked a year. **[A3]** Overhead uplift: 0% / 25% / 50% (low / base / high). **[A4]** No external consultant or legal fees are priced in. EU evidence suggests these are material: Danish external consultants account for about 30% of transaction cost ([Oslo Economics 2023][oe], citing KFST 2019). The EC 2025 figure of about €11,400 per bidder implies roughly €1,000 per person-day, which suggests it also includes external or overhead cost.

## Per-stage effort tables

Low, base and high values are paired consistently: low hours use the low rate, and so on. EUR figures are rounded.

### 1. Full procedure (national or EU threshold and above)

**Buyer (contracting authority)**

Stage shares:

- Low and base use the EC 2025 evaluation burden ranking for one-off procedures: pre-award 53%, award 29%, post-award 18% ([SWD(2025) 332][swd332], Table 95).
- High uses the EC 2011 mean full-time-equivalent days: 48% / 33% / 19% ([PwC et al. 2011][pwc2011], Fig 2.3).

| Stage | Low h | Base h | High h | Low € | Base € | High € |
|---|---|---|---|---|---|---|
| Pre-award: needs, market consultation, specs, documents, notice | 68 | 85 | 134 | 1,650 | 2,580 | 4,910 |
| Award: clarifications Q&A, opening, qualification, evaluation, decision, standstill | 37 | 46 | 92 | 900 | 1,410 | 3,370 |
| Post-award to signature: contract, notices, reporting | 23 | 29 | 53 | 560 | 880 | 1,940 |
| **Total buyer** | **128** | **160** | **280** | **3,110** | **4,860** | **10,220** |

Where the person-days come from:

| Scenario | Person-days | Source |
|---|---|---|
| Base | 20 | EU median, 2019–24 ([SWD(2025) 332][swd332], Table 94) |
| Low | 16 | EC 2011 framework median. Cross-check: Oslo Economics measured 149 h for Norwegian EU-level buyers excluding contract follow-up ([OE 2023][oe], Table 3-3). |
| High | 35 | EC 2011 means summed across stages (16.9 + 11.7 + 6.9) |

Estonia's own 2011 median was 19 person-days ([PwC et al. 2011][pwc2011], Fig 2.9).

**Supplier (per bid)**

Stage shares use the EC 2025 ranking for economic operators in one-off procedures: pre-proposal 41%, proposal 43%, post-award 17% ([SWD(2025) 332][swd332], Table 95). The 41/43 split is renormalised to the bid stages. Post-award is added for the winner only.

| Stage | Low h | Base h | High h | Low € | Base € | High € |
|---|---|---|---|---|---|---|
| Pre-proposal: find notice, read documents, ask clarification questions, bid/no-bid, partners | 35 | 47 | 78 | 750 | 1,240 | 3,040 |
| Proposal: technical and price offer, ESPD/qualification docs, sign-off | 37 | 49 | 82 | 770 | 1,290 | 3,170 |
| **Total per bid** | **72** | **96** | **160** | **1,520** | **2,530** | **6,210** |
| Winner only: post-award (evidence, guarantees, contract) | 14 | 19 | 32 | 300 | 510 | 1,240 |

Where the person-days come from:

| Scenario | Person-days | Source |
|---|---|---|
| Low | 9 | EU median of 11 per tender for 2019–24, less the 17% post-award share ([SWD(2025) 332][swd332], Tables 94–95) |
| Base | 12 | Midpoint, consistent with the EC 2011 mean bid stages (4.3 + 10 = 14.3) |
| High | 20 | Above the 2008–10 median (16) and Estonia's 2011 median (16 per bid); close to the restricted-procedure median (19). Works procedures can reach 29 person-days ([PwC et al. 2011][pwc2011], Fig 2.1). |

**[A5]** Every bidder spends as much as the winner. The EC surveys measured winners only and made the same assumption.

**Per procedure (buyer + n × bid + winner post-award)**

| | Low | Base | High |
|---|---|---|---|
| Bidders | 3.6 (EE 2021) | 4.4 (EE 2024–25) | 5.7 (EU open procedures, 2011) |
| Hours | ~400 | ~600 | ~1,220 |
| EUR | **€8,900** | **€16,500** | **€46,800** |
| Supplier share | 65% | 71% | 78% |

Sanity checks:

- The EU-wide average for above-threshold procedures in 2019–24 is **€43,200** (2022 prices): €6,000 for the authority and €11,400 per bidder ([SWD(2025) 332][swd332], Table 96). Estonia's 2026 draft-law explanatory memorandum quotes this figure directly ([788 SE seletuskiri][sk], network-sector section).
- The Estonian base case is lower because Estonian labour costs are about 65% of the EU level in public administration (€24.3 against €37.1 an hour) and because Estonia historically needed fewer person-days: 62 against an EU average of 108 in 2011.
- The 2009 Estonian figure was €7,500 per procedure. Scaled by wage growth since then (about 2.7×), it gives about €20k, which sits within the range.

### 2. Simple procurement (lihthange / väikehange)

No Estonian time measurement exists, so the evidence is Nordic:

- Oslo Economics measured Norwegian procurements below the EU threshold. Buyer preparation plus running the competition took **70 h** below the national threshold (unpublished) and **140 h** between the national and EU thresholds. Contract follow-up is excluded. Suppliers spent **20–40 h per bid** ([OE 2023][oe], Table 3-3 and §3.3).
- Denmark's National Audit Office found that ministries' advertised below-threshold purchases cost **DKK 26k for the buyer and DKK 44k per bidder** ([Rigsrevisionen 18/2018][rr]). That is a higher-wage benchmark, and notably the bidder cost exceeds the buyer cost.

| Party / stage | Low h | Base h | High h | Low € | Base € | High € |
|---|---|---|---|---|---|---|
| Buyer: preparation (1/3) | 13 | 23 | 46 | 320 | 710 | 1,680 |
| Buyer: running the competition and award (2/3) | 27 | 47 | 94 | 650 | 1,420 | 3,430 |
| **Buyer total** | **40** | **70** | **140** | **970** | **2,130** | **5,110** |
| Supplier per bid (pre-proposal + proposal, about 50/50) | 20 | 30 | 60 | 420 | 790 | 2,330 |
| **Per procedure** (3.6 / 4.4 / 5.7 bidders) | ~110 | ~200 | ~480 | **€2,500** | **€5,600** | **€18,400** |

**[A6]** The Norwegian hour counts transfer to Estonian simple procurement. **[A7]** The high supplier value of 60 h widens Oslo Economics' 20–40 h to reflect the Danish audit evidence.

### 3. Framework call-offs (not in the register counts)

- The EC 2011 study found a call-off under a framework costs about half of a full competition ([PwC et al. 2011][pwc2011]).
- The EC 2025 evaluation puts the buyer's cost for frameworks at €3,500, against €6,000 for all procedures ([SWD(2025) 332][swd332], Table 96).
- Estonia ran 2,091 framework-award procedures in 2025, worth 56% of value ([MoF 2025 stats][ee25]). Their mini-competitions are uncounted.
- **[A8]** Treat a call-off as 30–50% of the full-procedure buyer effort until the number of call-offs is known.

## Annual system cost (2025 volumes)

| | Low | Base | High |
|---|---|---|---|
| Full procedures (5,444 ×) | €48M | €90M | €255M |
| Simple procurements (3,693 ×) | €9M | €21M | €68M |
| **Total** | **€58M** | **€111M** | **€323M** |
| of which buyers | — | €34M (~1.13M h, about 670 FTE) | — |
| of which suppliers | — | €76M | — |
| As % of 2025 procurement value (€5.1bn) | 1.1% | 2.2% | 6.3% |

The total procurement value is from the [MoF 2025 stats][ee25], Figure 2.

## Does clarification dominate officer time?

**Unproven.**

- **Against the claim**: The EC evaluation ranks the **pre-award** stage (specification and documents) as the most burdensome for authorities, at 53%. The award stage, which contains clarification Q&A along with qualification and evaluation, is 29% ([SWD(2025) 332][swd332], Table 95). The 2011 study agrees: pre-award has the highest cost ([PwC et al. 2011][pwc2011]).
- **Partly for the claim**: Denmark's 2019 measurement put **75% of 616 internal buyer hours** in "running the competition" (gennemførelse), which includes Q&A ([OE 2023][oe], citing KFST 2019). Stage boundaries differ between studies.
- **Missing**: No source separates clarification time. Estonia's simplification work (the TGS Baltic report and the 788 SE memorandum) targets formal requirements (documents, ESPD/hankepass checks, notifications), not Q&A ([788 SE seletuskiri][sk] §6.2). The Estonian business input to the EC evaluation also points to over-zealous practice rather than the law itself ([SWD(2025) 332][swd332]).

## Assumptions (flagged)

| ID | Assumption | Effect if wrong |
|---|---|---|
| A1 | 8 h per person-day | ±12% on hours if 7 h |
| A2 | 1,680 hours worked per FTE a year | FTE count only |
| A3 | Overhead uplift 0 / 25 / 50% | Scales EUR linearly |
| A4 | No external advisers priced | Understates cost; Denmark suggests +30% |
| A5 | Losing bidders spend as much as winners | May overstate supplier cost if losers bid lighter |
| A6 | Norwegian hours apply to Estonian simple procurement | Unknown direction |
| A7 | High supplier value of 60 h for simple bids | Wide high tail |
| A8 | Call-offs cost 30–50% of a full buyer effort | Excluded from totals |
| A9 | EU-wide stage shares (Table 95) are burden rankings, used here as time shares | Stage split only, not totals |
| A10 | The 2019–24 EU medians describe Estonian practice in 2025 | Estonia was below the EU median in 2011 |

## Open uncertainties

1. **Post-reform mix.** From 1 Nov 2026, the national-level class (3,497 procedures in 2024, 40% of the count) moves to simple procurement, and the €30–50k goods/services band (about 800 procedures a year) leaves regulated procurement ([788 SE seletuskiri][sk] §6.1–6.2).
   - If national-level procedures currently cost as much as the full-procedure benchmark, the base-case annual total would fall by **up to about €40M**. That is an upper bound. They are smaller contracts, so the real drop is probably lower.
   - The production business case should re-weight the totals to the post-reform mix.
2. **Volume mismatch.** The case says there are 25,000 procedures a year. The register shows 9,137 in 2025, excluding mini-competitions, DPS calls, grant-recipient purchases (724) and purchases below €30k. The figure should be reconciled before any per-procedure saving is scaled.
3. **No Estonian time-motion data.** Every hour figure is transferred from EU or Nordic surveys. A small timesheet sample from RTK and two or three large buyers, plus an SME bidder survey, would turn these benchmarks into a measured baseline.
4. **Clarification share is unmeasured.** The case's "most time on clarification rounds" claim is untestable with public data.
5. **EC 2025 monetised supplier cost** of €11,400 per bidder for about 11 person-days is internally inconsistent unless it includes external costs. The underlying Ecorys (2025) cost-benefit study is not yet published.
6. **Contract management** (Oslo Economics: 85–114 h per contract) and **litigation** are excluded but material for agents that also handle post-award work.

## Sources

- [pwc2011] PwC, London Economics & Ecorys (2011), *Public procurement in Europe: Cost and effectiveness*, for DG MARKT. Figs 2.1, 2.3, 2.9; fn. 41. http://publications.europa.eu/resource/cellar/0cfa3445-7724-4af5-8c2b-d657cd690c03.0001.01/DOC_1
- [swd332] European Commission (2025), SWD(2025) 332 final, *Evaluation of Directives 2014/23/EU, 2014/24/EU and 2014/25/EU*, Tables 94–96. https://ec.europa.eu/transparency/documents-register/api/files/SWD(2025)332_0/090166e5238b9bf1?rendition=false (Executive summary SWD(2025) 333: https://ec.europa.eu/transparency/documents-register/api/files/SWD(2025)333_0/090166e5238b9bf3?rendition=false)
- [oe] Oslo Economics (2023), *Offentlige anskaffelser i 2022*, report 2023-51, Table 3-3. https://osloeconomics.no/wp-content/uploads/2023/11/OE-rapport-2023-51.-Rapport-til-anskaffelsesutvalget.-Offentlige-anskaffelser-i-2022.pdf
- [rr] Rigsrevisionen (2019), *Beretning 18/2018 om transaktionsomkostninger ved udbud i staten*. https://www.rigsrevisionen.dk/revisionssager-arkiv/2019/jun/beretning-om-transaktionsomkostninger-ved-udbud-i-staten
- [kfst] Erhvervsministeriet / KFST (2019), *Transaktionsomkostninger ved EU-udbud* (news). https://www.em.dk/aktuelt/nyheder/2019/apr/ny-analyse-ser-naermere-paa-omkostningerne-ved-offentlige-indkoeb
- [ee24] Rahandusministeerium (2025), *2024. a riigihangete valdkonna statistika ja kokkuvõte*. https://www.fin.ee/sites/default/files/documents/2025-06/2024.a%20riigihangete%20valdkonna%20statistika%20ja%20kokkuv%C3%B5te.pdf
- [ee25] Rahandusministeerium (2026), *2025. a riigihangete valdkonna statistika ja kokkuvõte* (08.05.2026). https://www.fin.ee/sites/default/files/documents/2026-05/2025.a%20riigihangete%20valdkonna%20statistika%20ja%20kokkuv%C3%B5te_0.pdf
- [sk] Seletuskiri, 788 SE *Riigihangete seaduse ja teiste seaduste muutmise seadus* (VV, Jan 2026). https://api.riigikogu.ee/api/files/ff76e93b-3732-4a84-bf32-2fe0365f239a/download
- [rt-act] Riigi Teataja, RT I, 03.07.2026, 3 (in force 01.11.2026). https://www.riigiteataja.ee/akt/103072026003
- [rt-sum] Riigi Teataja law summary. https://www.riigiteataja.ee/et/seadus/seaduse_kokkuvote/2566
- [stat] Statistics Estonia (5 Mar 2026), *Average wages increased by 5.6% last year*. https://www.stat.ee/en/news/average-wages-increased-by-56-last-year
- [eurostat-api] Eurostat `lc_lci_lev`, EE, D1_D4_MD5, EUR, 2025. https://ec.europa.eu/eurostat/api/dissemination/statistics/1.0/data/lc_lci_lev?geo=EE&lcstruct=D1_D4_MD5&unit=EUR&sinceTimePeriod=2024
- [eurostat-news] Eurostat (31 Mar 2026), *EU hourly labour costs ranged from €12 to €57 in 2025*. https://ec.europa.eu/eurostat/web/products-eurostat-news/w/ddn-20260331-2

[pwc2011]: http://publications.europa.eu/resource/cellar/0cfa3445-7724-4af5-8c2b-d657cd690c03.0001.01/DOC_1
[swd332]: https://ec.europa.eu/transparency/documents-register/api/files/SWD(2025)332_0/090166e5238b9bf1?rendition=false
[oe]: https://osloeconomics.no/wp-content/uploads/2023/11/OE-rapport-2023-51.-Rapport-til-anskaffelsesutvalget.-Offentlige-anskaffelser-i-2022.pdf
[rr]: https://www.rigsrevisionen.dk/revisionssager-arkiv/2019/jun/beretning-om-transaktionsomkostninger-ved-udbud-i-staten
[ee25]: https://www.fin.ee/sites/default/files/documents/2026-05/2025.a%20riigihangete%20valdkonna%20statistika%20ja%20kokkuv%C3%B5te_0.pdf
[sk]: https://api.riigikogu.ee/api/files/ff76e93b-3732-4a84-bf32-2fe0365f239a/download
[rt-act]: https://www.riigiteataja.ee/akt/103072026003
[rt-sum]: https://www.riigiteataja.ee/et/seadus/seaduse_kokkuvote/2566
[stat]: https://www.stat.ee/en/news/average-wages-increased-by-56-last-year
[eurostat-api]: https://ec.europa.eu/eurostat/api/dissemination/statistics/1.0/data/lc_lci_lev?geo=EE&lcstruct=D1_D4_MD5&unit=EUR&sinceTimePeriod=2024
[eurostat-news]: https://ec.europa.eu/eurostat/web/products-eurostat-news/w/ddn-20260331-2
