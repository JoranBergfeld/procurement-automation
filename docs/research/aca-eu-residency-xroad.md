# Issue #22: ACA + Foundry inference, EU residency and private path to an X-Road Security Server

- **Question:** can Azure Container Apps (ACA: government workflow, agent workers, REST/MCP adapter) plus Foundry inference process and store procurement content in the EU while reaching the government's X-Road Security Server over a supported private network path?
- **Access date for every source:** 2026-11-24. Microsoft first-party sources only. Page text was read for every cited page unless listed under "Access failures" (section 11).
- **Out of scope:** the Modified Abuse Monitoring policy (not reopened, not interpreted). Foundry Agent Service is also out of scope because ACA workers are the selected runtime. It matters only if the design later adopts it.
- The ticket names no model, SKU or region, so section 10 lists what must be chosen and validated.

## 1. Verdict (conditional)

**Feasible, subject to the conditions below. Nothing here is a blanket guarantee.**

1. **Network path.** Microsoft documents every building block for a private path from an ACA workload-profiles environment to an on-premises or government network: custom VNet, UDRs, ExpressRoute, VNet peering and gateway transit (section 4). No Microsoft page walks through ACA to X-Road specifically. The absence of such a walkthrough does not make the path unsupported. Routing, DNS, port and protocol behaviour are customer validation items.
2. **ACA residency.** ACA is available in 12 EU regions per the ARM provider query (section 3). Microsoft states ACA "doesn't move or store customer data out of the deployed region" (FAQ), while the Azure data-residency page says customer data is "stored in Geo" for regions other than Southeast Asia and Brazil South. Ancillary services (Log Analytics, ACR, Azure Files, Event Hub, partner log sinks) follow their own settings (section 7).
3. **Foundry residency.** For the Azure-sold deployment types covered in section 6, ordinary at-rest storage is documented in the designated geography, with fine-tuning and preview exceptions recorded below. This is not a guarantee for every model or hosting option. *Processing* location depends on deployment type. Global can process anywhere, so it is excluded. Data Zone EU and Standard/Regional are the candidates (section 6).
4. **EU vs EU+EFTA is unresolved.** Microsoft pages use inconsistent wording for the EU Data Zone (section 2). A strict "EU member states only" requirement is not demonstrated for Data Zone EU. A Standard/Regional deployment in a named EU member-state region is the narrowest documented option, though Standard is also described as possibly processed "between regions within the geography".
5. **Region label is not a guarantee.** Each service has its own storage, processing, logging and support boundary. These are enumerated below, and the documented exceptions are recorded.

## 2. Strict EU vs EU+EFTA

| Source | Exact text | Reading |
|---|---|---|
| [Deployment types](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types) | EU data zone "follows the Azure EU Data Boundary, which can include EFTA countries and regions such as Norway and Switzerland in addition to EU member states"; Microsoft "can add regions to either data zone without prior notice" | EU+EFTA, and mutable |
| [Data privacy](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy), "Understanding location of processing" | "If you create a DataZone deployment in a Foundry resource located in a European Union Member Nation, prompts and responses may be processed in that or any other European Union Member Nation." Also: DataZone prompts "may be processed in any geography within the specified data zone, as defined by Microsoft." | EU member states |
| [Region availability](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure-region-availability), Data Zone Standard | "European Union (data processed within any EU member nation)" | EU member states |
| Same page, EU Data Zone grid | Columns include `norwayeast` and `switzerlandnorth`, with availability marked for gpt-5.4 and later rows and model-router (my parse of the page text) | EFTA deployment locations appear in the "EU" grid; a deployment location alone does not establish where requests are processed |
| [EU Data Boundary](https://learn.microsoft.com/en-us/privacy/eudb/eu-data-boundary-learn) | EUDB "comprises EU and EFTA countries" (Liechtenstein, Iceland, Norway, Switzerland) | EUDB is EU+EFTA by definition |

**Result:** the Foundry pages do not agree on whether Data Zone EU can process in Norway or Switzerland. Treat Data Zone EU as EU+EFTA until Microsoft states otherwise in writing. Standard/Regional wording also differs: the region page says "in the region associated with your deployment", while the data-privacy page says "may be processed between regions within the geography for operational purposes".

**Geography definition.** Per [Azure regions overview](https://learn.microsoft.com/en-us/azure/reliability/regions-overview): "Every region is contained within a single geography that serves as a fixed data residency boundary." The "Americas / Europe / Asia Pacific / Middle East & Africa" headings on the Foundry region page are table groupings, not a statement of the at-rest geography. I could not retrieve a readable official region-to-geography mapping (the [geographies page](https://azure.microsoft.com/en-us/explore/global-infrastructure/geographies/) returned marketing content only). Which regions share the "Europe" geography is therefore a validation item. [Regions list](https://learn.microsoft.com/en-us/azure/reliability/regions-list) labels, for example, North Europe and West Europe as Europe (Ireland, Netherlands). UK South and UK West are neither EU nor EFTA.

## 3. ACA supported EU regions

**Observed provider metadata.** The command comes from the [ACA FAQ](https://learn.microsoft.com/en-us/azure/container-apps/faq). The research session that wrote this report (the `research/aca-eu-xroad` worktree session) ran it on 2026-11-24 at about 17:57 CET (UTC+1), using Azure CLI under its existing `az login` context:

```
az provider show --namespace Microsoft.App --query "resourceTypes[?resourceType=='managedEnvironments'].locations" --output json --only-show-errors
```

The command exited with code 0. No subscription or tenant identifiers are recorded here. It returned these EU locations for `Microsoft.App/managedEnvironments`:

| Region | Notes |
|---|---|
| Sweden Central, West Europe, North Europe, Germany West Central, France Central, Austria East, Belgium Central, Spain Central, Italy North, Poland Central | Returned. Open to general use per the [regions list](https://learn.microsoft.com/en-us/azure/reliability/regions-list) (not restricted) |
| Germany North, France South | Returned, but the regions list marks them **restricted-access**, so they are not general candidates |
| Denmark East | **Not returned**, so not available to this subscription/provider view |

Also returned but **not EU**: Norway East and Switzerland North (EFTA), and UK South/West (neither EU nor EFTA).

**Limits of this evidence:**
- It is the ARM provider's location metadata at one time and from one subscription's view. It does not guarantee capacity, workload-profile SKU availability, zone redundancy or a successful environment create. [Reliability](https://learn.microsoft.com/en-us/azure/reliability/reliability-container-apps) states that zone redundancy is set at creation and requires a VNet, so validate it per region.
- Microsoft publishes no static ACA region list in the pages I could read (section 11).
- Foundry model availability is separate. Intersecting ACA's returned EU regions with the Foundry Data Zone EU grid columns (france central, germany west central, italy north, north europe, poland central, spain central, sweden central, west europe) gives eight candidate shared deployment regions, not eight guaranteed model/SKU combinations. Austria East and Belgium Central have no Foundry column in the grids I read. ACA and Foundry need not share a region, but the cross-region call path must be validated.

## 4. Private path: workload profiles vs consumption-only, outbound vs ingress

Sources: [ACA networking](https://learn.microsoft.com/en-us/azure/container-apps/networking), [custom VNet](https://learn.microsoft.com/en-us/azure/container-apps/custom-virtual-networks), [FAQ](https://learn.microsoft.com/en-us/azure/container-apps/faq), [ingress overview](https://learn.microsoft.com/en-us/azure/container-apps/ingress-overview), [VNet peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview), [VPN Gateway](https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways), [ExpressRoute](https://learn.microsoft.com/en-us/azure/expressroute/expressroute-introduction).

**Generic Azure building blocks (verified):**
- Peering: a peered VNet's gateway can act as a transit point to on-premises. The VNet using a remote gateway "can't have its own gateway". "Both virtual network peering and global virtual network peering support gateway transit." Hub-and-spoke designs are documented.
- VPN Gateway: IPsec/IKE site-to-site tunnels connect Azure and on-premises, running **over the public Internet** but encrypted.
- ExpressRoute: a private connection via a connectivity provider. The page says "ExpressRoute connections don't go over the public internet".

**ACA specifics:**

| Capability | Workload profiles (Consumption and Dedicated plans) | Consumption-only (legacy) |
|---|---|---|
| UDR | Supported | Limited (one `Azure.<REGION>` service-tag route) |
| ExpressRoute | Supported ("full featured UDR and Express Route support", FAQ) | Not supported (FAQ) |
| Peering through a remote gateway | No limitation stated | Not supported (networking page) |
| NAT Gateway / other egress proxy | Supported (StandardV2 excluded) | Not supported |
| Private endpoint to the environment | Supported | Not supported |
| Subnet | `Microsoft.App/environments` delegation, /27 minimum | /23, not delegated |

The accurate reading is that workload profiles are needed *for designs that rely on UDR, ExpressRoute, remote-gateway transit or NAT*. These are the typical hub-and-spoke government connectivity patterns, so workload profiles are the recommended baseline. They are not a documented prerequisite for every private-connectivity scenario. Network type and subnet size cannot be changed after creation, so decide before building.

**Outbound ACA to Security Server.** The documented route is a custom-VNet workload-profiles environment, then UDR or peering with gateway transit to the VPN/ExpressRoute gateway, then the government network. Traffic that leaves via the environment's outbound public IP "aren't routed down a VPN" (networking page), so private-range destinations must be routed via UDR/gateway, not NAT.

**Inbound to ACA (clients, X-Road callbacks, adapters):**
- An Internal environment has no public endpoint and is reachable from "virtual machines, private endpoints, peered networks, and on-premises networks connected through a VPN or Azure ExpressRoute" (ingress overview). App ingress must be *external* (published on the internal load balancer) for VNet clients. *Internal* app ingress is for in-environment callers only.
- A [private endpoint](https://learn.microsoft.com/en-us/azure/container-apps/private-endpoints-with-dns) (workload profiles only) requires public network access Disabled, a private DNS zone `privatelink.{regionName}.azurecontainerapps.io`, and incurs extra charges. Its VNet can differ from the integrated VNet.

## 5. DNS, UDR and NSG requirements

Sources: [Private endpoints and DNS](https://learn.microsoft.com/en-us/azure/container-apps/private-endpoints-with-dns), [firewall integration](https://learn.microsoft.com/en-us/azure/container-apps/firewall-integration), [Azure Firewall](https://learn.microsoft.com/en-us/azure/container-apps/use-azure-firewall), [UDR](https://learn.microsoft.com/en-us/azure/container-apps/user-defined-routes).

- **Custom DNS:** forward unresolved queries to 168.63.129.16. Consumption plan: do not block `AzurePlatformDNS`. Dedicated profiles may. Do not override resolution of `*.hcp.<LOCATION>.azmk8s.io`, `mcr.microsoft.com` and other AKS-shared required names.
- **Internal environments:** a private DNS zone named for the environment default domain (or the custom apex domain) with a wildcard A record to the environment's static IP.
- **Outbound allow rules (workload profiles):** `MicrosoftContainerRegistry` and `AzureFrontDoor.FirstParty` (443); `AzureActiveDirectory` (443, managed identity); `AzureMonitor` (443, if used); ACR and `Storage.<Region>` tags (443, if ACR is used); 168.63.129.16 DNS (53); intra-subnet traffic.
- **UDR via firewall:** the Azure Firewall example uses a 0.0.0.0/0 route to a virtual appliance and "Propagate gateway routes = No". A forced-tunnel design to on-premises must still allow the above Microsoft endpoints, so keep a direct or firewall-permitted egress for them. The required FQDNs include `mcr.microsoft.com`, `*.data.mcr.microsoft.com`, `packages.aks.azure.com`, `acs-mirror.azureedge.net`, the ACR address, `*.blob.core.windows.net` and `login.microsoft.com`.
- Subnet constraints: delegation, no overlap with the reserved ranges (169.254.0.0/16, 172.30.0.0/16, 172.31.0.0/16, 192.0.2.0/24, and 100.100.x.x blocks). Address plans must avoid government-network collisions.
- External workload-profile environments do not support NSG/firewall lockdown of inbound traffic on the managed public IP. Use an Internal environment or a private endpoint for a private-only design.

## 6. Foundry inference: processing vs storage

Sources: [Deployment types](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types), [Data privacy](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy), [Region availability](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure-region-availability), [Azure data residency](https://azure.microsoft.com/en-us/explore/global-infrastructure/data-residency/).

| Deployment type | At rest | Processing (as documented) |
|---|---|---|
| Global (Standard, Provisioned, Batch) | Designated geography | Any Azure region where the model is deployed |
| Data Zone EU (Standard, Provisioned, Batch) | Designated geography | Within the Microsoft-defined zone (EU vs EU+EFTA conflict, section 2) |
| Standard / Regional Provisioned | Designated geography | In the deployment's region per the region page; "may be processed between regions within the geography" per the data-privacy page |
| Developer tier | n/a | Any region, no guarantee |
| Managed compute (open-source/custom) | Not covered by these types | Not covered here |

The data-residency page also says Foundry Global/DataZone fine-tuned data and weights "may be stored temporarily outside the selected Geo". This is an exception to the ordinary at-rest boundaries in the table, not covered by an EU resource label.

**State and telemetry (data-privacy page):**
- Stateful features (Responses API, Assistants threads, stored completions, Files API/vector stores, fine-tuning) store data "at rest in the Foundry resource in the customer's Azure tenant, within the same geography as the resource", subject to the separately documented fine-tuning exception above. For Global Batch, data at rest stays in the geography "until processing capacity becomes available", but processing may occur anywhere the model is deployed. Data Zone Batch is a distinct deployment type in the deployment-types table; do not treat every Batch SKU as Global.
- Preview models/features "might not support all of the above conditions".
- Responses API state is service-managed (Cosmos DB/Blob Storage) and cannot use bring-your-own storage. If the workers avoid stored state (stateless chat/embedding calls with state kept in ACA-owned storage), this exposure is avoided.
- Private connectivity: [private endpoint guidance](https://learn.microsoft.com/en-us/azure/foundry/how-to/configure-private-link) covers the public network access flag and Private Link to the Foundry project. The [Foundry Tools networking page](https://learn.microsoft.com/en-us/azure/ai-services/cognitive-services-virtual-networks) says to keep using the resource's custom subdomain endpoint, not the `*.privatelink.openai.azure.com` intermediary CNAME.

**EU Data Boundary telemetry exception.** [EUDB ongoing partial transfers](https://learn.microsoft.com/en-us/privacy/eudb/eu-data-boundary-ongoing-partial-transfers) says that for "AI Subprocessor services that offer EU Data Boundary commitments", the service "may process and store a pseudonymized user ID or session GUID outside the EU Data Boundary, including in the United States", and "does not store prompts, responses, or other Customer Data or Personal Data outside the EU Data Boundary, unless otherwise disclosed". Whether this clause applies to a specific Azure OpenAI or partner deployment is not stated on that page. Confirm with Microsoft before relying on it either way.

### 6.1 Azure OpenAI vs other direct vs partner models

- Models "sold by Azure" (the data-privacy page says this "includes Azure OpenAI models") follow the deployment-type rules above.
- **Anthropic Claude** ([Claude data privacy](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/claude-models/data-privacy)): "Hosted on Azure" means prompts and outputs are processed on Azure infrastructure and "never leave Azure"; "Hosted on Anthropic Infrastructure" means data "might be processed outside of Azure including outside of your selected Azure geography". Anthropic acts as an independent data processor for both options. The page states no EU region or data-zone commitment, so Claude must not be assumed EU-resident.
- Other partner and community models are not covered by the Azure OpenAI statements. Each needs its own data-handling page. I verified only the Claude page.

### 6.2 Representative EU availability (my parse of the grids; not exhaustive, verify each combination)

Region columns in the grids: Data Zone Standard: francecentral, germanywestcentral, italynorth, northeurope, norwayeast, polandcentral, spaincentral, swedencentral, switzerlandnorth, westeurope. Standard/Regional has fewer columns (france, germany west central, norway east, poland, spain, sweden, switzerland north, uk south, west europe).

| Grid | Representative findings |
|---|---|
| Data Zone Standard (EU) | gpt-5.4 ... gpt-6.2-sol and model-router are marked available in both EFTA columns (`norwayeast`, `switzerlandnorth`) in my parse. gpt-4.1 family has gaps in north europe, norway east and switzerland north |
| Standard/Regional | Uneven: for example gpt-4.1 is marked in swedencentral and switzerlandnorth only; gpt-4.1-mini in france central, sweden central, switzerland north, uk south, west europe; text-embedding-3-large in eight regions; gpt-5.1 in sweden central and switzerland north. Newer gpt-5.x/6.x rows did not appear as available in the Standard/Regional grid in my parse |
| Data Zone Provisioned (EU) | gpt-5, 5.1, 5.4, 5.5 are marked in france central, germany west central, italy north, poland central, spain central, sweden central, west europe (not Norway or Switzerland) |
| Regional Provisioned | Sparse: for example gpt-5.4 in germany west central, sweden central, uk south; gpt-5 in uk south only |

Model names above (for example "gpt-6.2-sol 2026-11-17") appear exactly as written in the page text I retrieved. I did not validate them against another source.

## 7. ACA content, log and storage boundaries

| Item | Documented boundary | Source |
|---|---|---|
| ACA customer data | "doesn't move or store customer data out of the deployed region" (FAQ). Residency page: for regions other than Southeast Asia and Brazil South, "customer data is stored in Geo" (footnote 1). ACA is on the single-region list only for those two regions | [FAQ](https://learn.microsoft.com/en-us/azure/container-apps/faq), [residency](https://azure.microsoft.com/en-us/explore/global-infrastructure/data-residency/) |
| Resilience | Single region, no cross-region failover or built-in backup. Control-plane metadata replicated across zones | [Reliability](https://learn.microsoft.com/en-us/azure/reliability/reliability-container-apps) |
| Container logs | Customer-chosen destination: Log Analytics (default), diagnostic settings (Log Analytics, storage account, Event Hub, partner solutions), or none | [Log options](https://learn.microsoft.com/en-us/azure/container-apps/log-options) |
| Log Analytics | "Each workspace resides in a particular Azure region" ([workspace design](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/workspace-design)). Export storage must be in the same region; workspace replication copies logs "to another region" if enabled ([best practices](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/best-practices-logs)) | as listed |
| Application Insights | Workspace-based: "sends telemetry to a common Log Analytics workspace" ([create workspace resource](https://learn.microsoft.com/en-us/azure/azure-monitor/app/create-workspace-resource)). ACA supports it via SDK only ([observability](https://learn.microsoft.com/en-us/azure/container-apps/observability)) | as listed |
| ACR | "stores data in the region where the registry is created"; "Azure may also store registry data in a paired region in the same geography" except Brazil South and Southeast Asia. Geo-replication adds customer-chosen regions ([ACR storage](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-storage)) | as listed |
| Azure Files / Storage | Redundancy choice decides: LRS/ZRS stay in the region. GRS/GZRS copy to the paired region, and "In some cases, the paired regions across which the data is geo-replicated might be in another country or region" ([Azure Files redundancy](https://learn.microsoft.com/en-us/azure/storage/files/files-redundancy)). Residency footnote 4: "ZRS Classic, GRS/RA-GRS, GZRS/RA-GZRS stores data in multiple regions" | as listed |
| Cloud-build deployments | `containerapp up` defaults to US East unless `--location` is set, and docker-less builds use the ACA default registry. Avoid for residency-sensitive builds | [FAQ](https://learn.microsoft.com/en-us/azure/container-apps/faq) |
| Platform/support/ARM metadata | EUDB: remote access by Microsoft personnel from outside the boundary is "considered a transfer of data under European privacy law although the data remains within Microsoft datacenter infrastructure" ([EUDB transfers](https://learn.microsoft.com/en-us/privacy/eudb/eu-data-boundary-transfers-for-all-services)). Azure Resource Manager "may store Customer Data and pseudonymized personal data outside of the EU Data Boundary" (IP addresses, PUIDs, object IDs, session IDs) ([partial transfers](https://learn.microsoft.com/en-us/privacy/eudb/eu-data-boundary-temporary-partial-transfers)). Professional Services Data in the EUDB needs a tenant-level ARM EU Data Boundary configuration, possible only in new tenants with no resources ([non-regional config](https://learn.microsoft.com/en-us/privacy/eudb/eu-data-boundary-configure-azure-nonregional-services)). Azure Monitor is available in the EUDB "except for specific scenarios", and Application Change Analysis is excluded | as listed |
| Azure-wide exceptions | Microsoft may copy data between regions within a Geo. Documented exceptions include Foundry Global/DataZone processing, preview services that typically store in the US, and non-regional services | [Residency](https://azure.microsoft.com/en-us/explore/global-infrastructure/data-residency/) |

The ACA pages I read do not list a service-specific platform-metadata or support-access exception. The absence of such a statement is not a guarantee, so the platform-level EUDB items above apply as the documented exceptions.

## 8. X-Road applicability

- Microsoft documents no X-Road integration, reference architecture or certification. That is not the same as unsupported: standard Azure networking applies. Nothing in the Microsoft sources I read prohibits an X-Road adapter on ACA.
- Platform constraints that matter for an adapter: ACA HTTP ingress is ports 80/443 (TLS 1.2/1.3, terminated at ingress, 240 s request timeout); TCP ingress is supported as a second protocol type ([ingress overview](https://learn.microsoft.com/en-us/azure/container-apps/ingress-overview)).
- **Customer validation (routing/protocol, not Microsoft-documented):** connection direction (adapter to Security Server vs the reverse); ports and mutual-TLS and certificate handling when TLS terminates at ACA ingress; source-IP/NAT as seen by the government network; DNS resolution of the Security Server name from the ACA subnet; MTU, latency and timeout behaviour over VPN/ExpressRoute.
- X-Road's own requirements are non-Microsoft documentation and were not used.

## 9. Evidence matrix

| # | Claim | Strength | Source |
|---|---|---|---|
| 1 | Workload profiles support UDR, NAT, private endpoints; consumption-only does not | Verified | ACA networking |
| 2 | ExpressRoute unsupported on consumption-only; supported on workload profiles | Verified | ACA FAQ |
| 3 | Peering gateway transit and VPN/ExpressRoute building blocks exist | Verified | VNet peering, VPN Gateway, ExpressRoute |
| 4 | An ACA to on-premises walkthrough exists | Not found; customer validation, not unsupported | n/a |
| 5 | 12 EU ACA regions returned; Denmark East did not | Observed ARM metadata, 2026-11-24 | FAQ command, executed by the research session (section 3) |
| 6 | ACA data stays in region / Geo | Vendor statement, no enumerated ACA exceptions | FAQ, residency page |
| 7 | Foundry Global can process anywhere | Verified | Deployment types, data privacy |
| 8 | EU Data Zone is EU-only | **Conflicting** (EU+EFTA vs EU) | Section 2 |
| 9 | Standard processed only in the deployed region | **Conflicting** wording | Section 2 |
| 10 | Stateful Foundry data stays in the resource's geography | Verified (non-preview) | Data privacy |
| 11 | Claude is EU-resident | **Not established**; depends on hosting option | Claude data privacy |
| 12 | Log Analytics, ACR, Azure Files are regional with replication/redundancy exceptions | Verified | Section 7 sources |
| 13 | EUDB transfer exceptions apply (remote access, ARM metadata) | Verified | EUDB pages |
| 14 | X-Road works on ACA | Not addressed by Microsoft | n/a |

## 10. Unresolved combinations and operational validation prerequisites

The ticket names no model, SKU or region, so none can be signed off. Decisions needed:
1. **Scope:** EU member states only vs EU+EFTA (drives Data Zone EU eligibility).
2. **Deployment type and region:** Data Zone EU vs Standard/Regional in one named member-state region. Global is excluded.
3. **(model, version, deployment type, region)** checked against the grids. Standard/Regional availability for current-generation models is sparse in my parse.
4. **Partner models:** each needs its own data-handling statement (only Claude was checked).
5. **ACA region:** pick from the 12 returned. Avoid Germany North and France South (restricted). Confirm workload-profile SKUs, zone redundancy and quota by creating a pilot environment.

Validation checklist:
- Pilot a workload-profiles, custom-VNet environment. Test DNS, UDR/gateway routing and a real connection to a Security Server test endpoint.
- Place Log Analytics, Application Insights, ACR, storage, Key Vault and any diagnostic sinks in chosen EU regions. Avoid geo-replication/GRS/GZRS to non-EU pairs, and avoid Event Hub or partner sinks outside the EU.
- Block Global SKUs with Azure Policy (the deployment-types page includes a `sku.name` policy example).
- Avoid Responses API state, Global Batch, Developer tier and preview features unless their residency is separately confirmed. Evaluate Data Zone Batch separately against its zone definition and exact model/SKU availability.
- Obtain written Microsoft confirmation on the EU vs EFTA definition and on the EUDB telemetry clause (section 6).

## 11. Access failures and remaining verification gaps

- **Products-by-region** ([page](https://azure.microsoft.com/en-us/explore/global-infrastructure/products-by-region/)): client-rendered. The text retrieved (about 5 KB) held only navigation, with no ACA region data. The ARM provider metadata observed on 2026-11-24 is the ACA region evidence instead.
- **Geographies page:** marketing content only. No official region-to-geography table was retrievable, so "Europe" geography membership is unverified.
- **Application Insights:** I confirmed that workspace-based telemetry is sent to Log Analytics, but did not locate an Application Insights page establishing a separate end-to-end processing/residency guarantee. The destination workspace's region is known; that alone does not prove every telemetry-processing or metadata boundary.
- **Partner models:** only Claude data handling was verified. No claim is made for other partner or community models.
- **Foundry Agent Service:** excluded as the runtime is ACA. It would need its own review.
- **Grid parsing:** the Foundry availability tables were parsed from page text with a script. Re-check any chosen combination in the live page, since availability changes.
- **EUDB page text:** the overview page body says "Last updated: February 26, 2025" while its metadata date is 2026-07-22.
- **Sources not used:** Microsoft Q&A answers were not used as evidence.
