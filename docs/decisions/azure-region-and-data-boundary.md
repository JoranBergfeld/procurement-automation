# Azure primary region and data boundary

**Status:** Partial planning decision, agreed 2026-11-24. The wider deployment envelope is not yet resolved or qualified.

## Decision

- Use **Sweden Central** as the single primary Azure region for the procurement platform.
- The acceptable geographic boundary for procurement content is the **EU Data Boundary**, including EU and EFTA countries. This supersedes the earlier EU-member-states-only planning restriction in the [Azure agent platform decision](https://github.com/JoranBergfeld/procurement-automation/issues/18).
- A single primary region does not imply that every service stores or processes all data only in that region. Service-managed replication and processing locations must be identified and assessed against the chosen boundary.
- Use **EU Data Zone Standard** with **GPT-6.1 Sol**, version `2026-09-29`, as an initial inference candidate. This is conditional on model-specific residency, quota, security and cost qualification.
- Evaluate and approve model-version changes before use. Verify that the deployment can enforce this; an unavoidable automatic change is a qualification gap.
- Prefer in-region redundancy where available, while allowing required service-managed replication to Sweden South. A separate disaster-recovery strategy remains open.
- Plan a separately qualified recovery region within the EU Data Boundary. Exact service-level agreements and objectives will be discussed after they are identified; the recovery mechanism is not yet specified.
- Keep prompts, responses and bid content out of operational logs by default. Preserve required evidence in its designated store under applicable retention rules.
- Plan the demonstration in the user's own tenant. This does not establish that the tenant, its home region or its service configurations meet the required controls.
- Use available public procurement data for the demonstration where appropriate, and synthetic data when needed. Review source permissions, privacy and handling requirements even for public data. A possible future synthetic generator could create a municipality's procurement and multiple fictional supplier bids; its design remains open.

## Context and rationale

The platform needs a common primary region and a clear geographic constraint for procurement content, prompts, results, workflow state and evidence. Sweden Central is a candidate for the existing Container Apps and Foundry architecture. Allowing EFTA within the EU Data Boundary avoids treating EU membership as a separate geographic restriction.

Microsoft defines the EU Data Boundary to include EU and EFTA countries and documents limited transfers outside it. Its Foundry deployment-types documentation says the EU Data Zone follows that boundary and can include Norway and Switzerland. Data Zone Standard is therefore the initial candidate, not evidence that a specific model or deployment is qualified.

Microsoft lists Sweden South, a restricted-access region in Sweden, as Sweden Central's paired region. Azure Confidential Ledger automatically replicates to the paired region and cannot be configured to stay in a single region. Availability of that service in the selected region and the implications for access, failover and residency still need qualification.

## Cost evidence

Microsoft's retail pricing API, queried on 2026-11-24 with `serviceName eq 'Foundry Models'`, `armRegionName eq 'swedencentral'` and `productName eq 'Azure OpenAI GPT6'`, lists the following Sweden Central Data Zone Standard short-context token meters in USD per million tokens. Actual charges can differ by agreement, currency and usage; long-context and cached-token meters differ.

| Model | Input | Output |
| --- | ---: | ---: |
| GPT-6.1 Sol | $2.40 | $12.00 |
| GPT-6 Luna | $0.12 | $0.60 |

For illustration only, 100 demonstration runs with five calls each, 3,000 input tokens and 1,000 output tokens per call, and no caching would cost about $9.00 for Sol or $0.48 for Luna in model inference. This excludes retries, other models, long-context rates, taxes and all infrastructure costs. It is not a forecast or an assertion that the models are equally capable.

Container Apps, SQL, Fabric, storage, networking and monitoring can add material costs, including while a demonstration is idle. Azure budget alerts do not by themselves stop consumption. Determine expected usage, service SKUs, running hours and suitable spending controls before deployment.

## Consequences and open questions

- Check each selected service's actual processing, storage, replication, backup, diagnostics and support-data locations; a Sweden Central resource label alone does not establish the end-to-end boundary.
- Confirm service and model availability, deployment type, quota, network connectivity and capacity before deployment. Compare the candidate's cost and performance with alternatives using representative workloads; its cost-effectiveness is not established.
- Qualify demonstration data sources and, if needed, decide how to generate synthetic buyer and supplier scenarios.
- Qualify the separate recovery region and decide the recovery mechanism, service-level objectives, backup configuration and any service-managed use of Sweden South.
- Check the demonstration tenant's home region, service configuration and governance against the chosen boundary and security requirements. The demonstration does not itself establish production readiness.
- Identify and review documented transfers, remote access and metadata exceptions with the appropriate legal and security owners. The geographic decision does not itself approve exceptions or establish compliance.
- Global inference remains excluded.

## Sources

- [Azure deployment envelope: regions, inference deployment and residency evidence](https://github.com/JoranBergfeld/procurement-automation/issues/24) - ongoing Wayfinder decision.
- [Azure agent platform fit: hosting, interoperability and measurement](https://github.com/JoranBergfeld/procurement-automation/issues/18) - previous architecture and narrower geographic restriction.
- [Azure ACA connectivity and EU data-residency boundaries for X-Road](https://github.com/JoranBergfeld/procurement-automation/issues/22) - prior research and qualification limits.
- Microsoft documentation, accessed 2026-11-24: [EU Data Boundary](https://learn.microsoft.com/en-us/privacy/eudb/eu-data-boundary-learn), [Foundry deployment types](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types), [Azure region pairs](https://learn.microsoft.com/en-us/azure/reliability/regions-paired), and [Azure Confidential Ledger data residency](https://learn.microsoft.com/en-us/azure/confidential-ledger/data-residency).
- Microsoft documentation, accessed 2026-11-24: [Foundry model availability](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure-region-availability) and [model lifecycle and automatic upgrades](https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/model-retirements).
- Microsoft documentation and data, accessed 2026-11-24: [Azure Retail Prices API](https://learn.microsoft.com/en-us/rest/api/cost-management/retail-prices/azure-retail-prices) (Foundry Models, Sweden Central, Data Zone Standard short-context meters) and [Azure budget alerts](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/tutorial-acm-create-budgets).
