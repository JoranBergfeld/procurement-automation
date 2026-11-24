# Azure primary region and data boundary

**Status:** Partial planning decision, agreed 2026-11-24. The wider deployment envelope is not yet resolved or qualified.

## Decision

- Use **Sweden Central** as the single primary Azure region for the procurement platform.
- The acceptable geographic boundary for procurement content is the **EU Data Boundary**, including EU and EFTA countries. This supersedes the earlier EU-member-states-only planning restriction in the [Azure agent platform decision](https://github.com/JoranBergfeld/procurement-automation/issues/18).
- A single primary region does not imply that every service stores or processes all data only in that region. Service-managed replication and processing locations must be identified and assessed against the chosen boundary.
- A documented support-access or operational-metadata exception may be accepted only with an explicit record of why it is necessary, what data and purpose are involved, how exposure is minimized, and who reviewed and approved it.
- Use **EU Data Zone Standard** with **GPT-6.1 Sol**, version `2026-09-29`, as an initial inference candidate. This is conditional on model-specific residency, quota, security and cost qualification.
- Evaluate and approve model-version changes before use. Verify that the deployment can enforce this; an unavoidable automatic change is a qualification gap.
- Prefer in-region redundancy where available. Identify required service-managed pairs, then select the recovery region only after the service inventory and regional availability are qualified.
- Plan a separate recovery region within the EU Data Boundary. Discuss service-level agreements and objectives after feasible service and region options are known; no recovery region or pair has been selected.
- Keep prompts, responses and bid content out of operational logs by default. Preserve required evidence in its designated store under applicable retention rules.
- Plan the demonstration in the user's own tenant. This does not establish that the tenant, its home region or its service configurations meet the required controls.
- Use available public procurement data for the demonstration where appropriate, and synthetic data when needed. Review source permissions, privacy and handling requirements even for public data. A possible future synthetic generator could create a municipality's procurement and multiple fictional supplier bids; its design remains open.

## Provisional service inventory

The user approved this inventory for qualification before choosing region pairs. Listing a service does not confirm its regional availability, SKU, capacity or readiness in the demonstration tenant.

| Role | Services and qualification |
| --- | --- |
| Core runtime | Azure Container Apps (ACA) in Sweden Central for agent workers, orchestration and the X-Road adapter; Microsoft Foundry for inference. |
| Workflow and evidence | Azure SQL Database with SQL Ledger for workflow state and accepted events; immutable Azure Blob Storage for required procurement evidence. |
| Measurement | Microsoft Fabric for KPI/event analytics, conditional on verifying the tenant home region is within the EU Data Boundary. Bid content stays out of analytics. |
| Ledger digests | Azure Confidential Ledger is conditional on regional availability and qualification; immutable Blob Storage is the documented alternative. |
| Supporting infrastructure | A custom VNet and private routing/DNS to the government X-Road Security Server; Azure Container Registry (candidate); Key Vault; Azure Monitor/Log Analytics for content-free operational telemetry; Entra identity and managed identities. Qualify each selected component and its data/replication boundary. |
| Demonstration application | If built, the synthetic-scenario generator is application functionality hosted in ACA, not a separate Azure service. |

## Regional qualification findings

Microsoft documentation and the linked research were reviewed on 2026-11-24. These findings narrow the region work; they do not prove tenant-level availability, capacity, quota, network reachability or production readiness.

| Service | Finding relevant to Sweden Central | Remaining qualification |
| --- | --- | --- |
| ACA | Sweden Central was returned by the Container Apps region query. Microsoft says customer data is not moved or stored outside the deployed region. Workload profiles support the required VNet-routing building blocks ([FAQ](https://learn.microsoft.com/en-us/azure/container-apps/faq), [prior connectivity research](https://github.com/JoranBergfeld/procurement-automation/issues/22)). | Confirm tenant quota, workload-profile capacity and the actual private route, DNS, certificates and timeouts. ACA has no built-in cross-region failover. |
| Foundry | The [model availability grid](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure-region-availability) lists GPT-6.1 Sol `2026-09-29` for Data Zone Standard in Sweden Central. Microsoft's [deployment-types documentation](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types) says the EU Data Zone follows the EU Data Boundary, which includes EFTA. Other availability wording refers to EU member states; both scopes are within the user's accepted boundary. | Confirm tenant quota/capacity and exact deployment availability. Version upgrades need qualification and evaluation. |
| Azure SQL Database | [Backups](https://learn.microsoft.com/en-us/azure/azure-sql/database/automated-backups-overview) default to geo-redundant storage replicated to a paired region. Sweden Central's pair is Sweden South, which is within the accepted boundary ([region pairs](https://learn.microsoft.com/en-us/azure/reliability/regions-paired)). | Prefer ZRS backups if verified in Sweden Central. Otherwise use local redundancy for the demonstration and defer geo-redundant backups until the recovery design is qualified. ZRS backup availability was not found in the reviewed documentation. |
| Blob Storage | SQL Ledger digests can use immutable Blob Storage ([digest management](https://learn.microsoft.com/en-us/azure/azure-sql/database/ledger-digest-management-and-database-verification)). Immutability supports LRS, ZRS, GRS and GZRS ([immutable storage](https://learn.microsoft.com/en-us/azure/storage/blobs/immutable-storage-overview)); the chosen redundancy determines replication. | Verify regional redundancy options and policy-lock behavior. Object replication has constraints with active immutability policies. |
| Azure Confidential Ledger | The [service](https://learn.microsoft.com/en-us/azure/confidential-ledger/data-residency) automatically replicates to the paired region and does not allow this replication to be disabled. | Availability in Sweden Central was not found. Sweden South replication is inferred from the region-pair list. Immutable Blob is the approved fallback candidate for digests. |
| Microsoft Fabric | [Fabric capacity](https://learn.microsoft.com/en-us/fabric/admin/region-availability) and all Fabric workloads are listed in Sweden Central. Workspace content follows the capacity region; some tenant metadata follows the tenant home region ([multi-geo](https://learn.microsoft.com/en-us/fabric/admin/service-admin-premium-multi-geo)). Fabric is conditional, rebuildable analytics in the primary region; the procurement workflow and authoritative records remain outside Fabric. | Verify the existing tenant home region is within the boundary. Do not enable OneLake DR unless paired-region support and data location are verified. Fabric support in Sweden South was not found; qualify Eventstream/Eventhouse and rebuild behavior ([OneLake DR](https://learn.microsoft.com/en-us/fabric/onelake/onelake-disaster-recovery)). |
| Key Vault | Microsoft's [recovery guidance](https://learn.microsoft.com/en-us/azure/key-vault/general/disaster-recovery-guidance) says managed replication applies when a region has a same-geography pair; this implies Sweden South for Sweden Central. | Confirm exact service behavior and any selected key-service requirements. |
| Azure Container Registry | [Zone redundancy](https://learn.microsoft.com/en-us/azure/container-registry/zone-redundancy) is supported where available. Azure documentation also notes that registry data may be stored in a paired region in the same geography ([storage](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-storage)). | Confirm exact paired-region storage behavior and regional availability for the selected registry configuration. |
| Log Analytics | Workspaces are regional; cross-region workspace replication is a separately enabled feature ([replication](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/workspace-replication)). | Keep replication disabled unless its selected target is explicitly approved. The complete default residency behavior was not verified. |
| Network, identity and monitoring support | VNet/private routing is customer-configured; Entra identity and managed identities are supporting components, not region pairs selected here. | Qualify endpoint, DNS, routing, diagnostics and metadata boundaries for every selected component. |

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
- Qualify every service in the provisional inventory, including service-managed replication and available regional SKUs. Select region pairs only after this inventory is stable; decide the recovery mechanism and service-level objectives after feasible options are known.
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
