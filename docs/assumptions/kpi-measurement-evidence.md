# KPI measurement evidence

These are material data and governance premises for computing and reporting the settled KPIs. Missing observations are not treated as zero.

## Assumptions

### Sources can provide attributable, precise events

**Status:** Unverified; implementation evidence required.

The event model assumes source systems can provide sufficiently attributable event times and linked case records for applicable KPI calculations.

**Impact if false:** A KPI may be unavailable or have limited coverage; timestamps or values must not be invented or substituted.

**Validation:** Test source coverage, event linkage, and timestamp precision during implementation. Owner unknown.

### Supplier effort can be observed lawfully

**Status:** Conditional; lawful access and coverage unresolved.

Supplier effort and response activity may be reported only if access is lawful and observations have adequate coverage. Unobserved activity cannot stand for zero effort or non-use.

**Impact if false:** Supplier effort and response measures must be reported as unavailable or limited by coverage.

**Validation:** Establish lawful access and measure observability and coverage. Owner unknown.

### Required audit evidence can be checked

**Status:** Unverified.

The event model assumes applicable audit evidence can be made available to authorised checkers and its integrity verified.

**Impact if false:** The system cannot reliably demonstrate that required procurement evidence is complete and trustworthy.

**Validation:** Verify evidence presence, access, and integrity at workflow gates and case completion. Assurance owner unknown.

### Conflicting source observations can be reconciled

**Status:** Unverified; authority not identified.

Where authoritative sources disagree, the design assumes an authorised person or process can reconcile them and accept a value. Until reconciliation, observations remain contested.

**Impact if false:** The affected KPI or decision input remains disputed rather than being silently resolved.

**Validation:** Define and test reconciliation at the first source disagreement. Authorised reconciler unknown.

### Coverage and sample thresholds can be set before the controlled implementation

**Status:** Conditional; expected volumes not yet known.

The KPI plan assumes minimum coverage and sample-size criteria can be defined for each workflow once expected volumes are available.

**Impact if false:** Outcome comparisons may be too weak to interpret, and implementation success criteria need revision.

**Validation:** Set thresholds before the controlled implementation using expected workflow volumes. Owner unknown.

## Sources

- [KPI framework: outcome, agent-performance and guardrail KPIs with exact definitions](https://github.com/JoranBergfeld/procurement-automation/issues/8)
- [KPI baselines and targets: what we commit to and how it is validated](https://github.com/JoranBergfeld/procurement-automation/issues/10)
- [Measurement event model: events and fields needed to compute each KPI](https://github.com/JoranBergfeld/procurement-automation/issues/15)
