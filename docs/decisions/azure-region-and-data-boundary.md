# Azure primary region and data boundary

**Status:** Partial planning decision, agreed 2026-11-24. The wider deployment envelope is not yet resolved or qualified.

## Decision

- Use **Sweden Central** as the single primary Azure region for the procurement platform.
- The acceptable geographic boundary for procurement content is the **EU Data Boundary**, including EU and EFTA countries. This supersedes the earlier EU-member-states-only planning restriction in the [Azure agent platform decision](https://github.com/JoranBergfeld/procurement-automation/issues/18).
- A single primary region does not imply that every service stores or processes all data only in that region. Service-managed replication and processing locations must be identified and assessed against the chosen boundary.
- Use **EU Data Zone Standard** as the initial inference candidate, subject to model-specific residency, quota and security qualification. The model and version remain undecided.
- Prefer in-region redundancy where available, while allowing required service-managed replication to Sweden South. A separate disaster-recovery strategy remains open.
- Plan a separately qualified recovery region within the EU Data Boundary. Exact service-level agreements and objectives will be discussed after they are identified; the recovery mechanism is not yet specified.
- Keep prompts, responses and bid content out of operational logs by default. Preserve required evidence in its designated store under applicable retention rules.
- Plan the demonstration in the user's own tenant. This does not establish that the tenant, its home region or its service configurations meet the required controls.

## Context and rationale

The platform needs a common primary region and a clear geographic constraint for procurement content, prompts, results, workflow state and evidence. Sweden Central is a candidate for the existing Container Apps and Foundry architecture. Allowing EFTA within the EU Data Boundary avoids treating EU membership as a separate geographic restriction.

Microsoft defines the EU Data Boundary to include EU and EFTA countries and documents limited transfers outside it. Its Foundry deployment-types documentation says the EU Data Zone follows that boundary and can include Norway and Switzerland. Data Zone Standard is therefore the initial candidate, not evidence that a specific model or deployment is qualified.

Microsoft lists Sweden South, a restricted-access region in Sweden, as Sweden Central's paired region. Azure Confidential Ledger automatically replicates to the paired region and cannot be configured to stay in a single region. Availability of that service in the selected region and the implications for access, failover and residency still need qualification.

## Consequences and open questions

- Check each selected service's actual processing, storage, replication, backup, diagnostics and support-data locations; a Sweden Central resource label alone does not establish the end-to-end boundary.
- Confirm service and model availability, version, deployment type, SKU, quota, network connectivity and capacity before deployment. No specific model or version has been selected.
- Qualify the separate recovery region and decide the recovery mechanism, service-level objectives, backup configuration and any service-managed use of Sweden South.
- Check the demonstration tenant's home region, service configuration and governance against the chosen boundary and security requirements. The demonstration does not itself establish production readiness.
- Identify and review documented transfers, remote access and metadata exceptions with the appropriate legal and security owners. The geographic decision does not itself approve exceptions or establish compliance.
- The previously agreed exclusion of Global inference and the modified abuse-monitoring requirement remain in force unless separately changed.

## Sources

- [Azure deployment envelope: regions, inference deployment and residency evidence](https://github.com/JoranBergfeld/procurement-automation/issues/24) - ongoing Wayfinder decision.
- [Azure agent platform fit: hosting, interoperability and measurement](https://github.com/JoranBergfeld/procurement-automation/issues/18) - previous architecture and narrower geographic restriction.
- [Azure ACA connectivity and EU data-residency boundaries for X-Road](https://github.com/JoranBergfeld/procurement-automation/issues/22) - prior research and qualification limits.
- Microsoft documentation, accessed 2026-11-24: [EU Data Boundary](https://learn.microsoft.com/en-us/privacy/eudb/eu-data-boundary-learn), [Foundry deployment types](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types), [Azure region pairs](https://learn.microsoft.com/en-us/azure/reliability/regions-paired), and [Azure Confidential Ledger data residency](https://learn.microsoft.com/en-us/azure/confidential-ledger/data-residency).
