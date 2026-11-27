# Research: SQL Ledger digest recovery within the 15-minute RPO

- **Question (issue #29):** Can a supported, tamper-resistant SQL Ledger digest design meet the provisional 15-minute recovery-point requirement across the Sweden Central primary and West Europe warm standby, while preserving the separately required Sweden South paired-region posture and the EU Data Boundary?
- **Access date for every source:** 2026-11-27. Microsoft Learn pages (page text read unless marked otherwise) and the public Microsoft Online Services SLA document (read only through a search excerpt; see section 7).
- **Out of scope / not done:** no tenant, subscription or environment inspection, no deployment, no measurement. Nothing was assumed about digest-policy replication.
- **Hygiene:** searches used only public product terms.

## 1. Verdict

**The 15-minute digest target is not supportable as a documented or SLA-backed guarantee on any option examined. It remains unverified as an engineering target, and treating it as a requirement needs a separately approved scope decision.** Reasons, each sourced below:

1. The automatic Blob design needs an immutability policy that "allows protected append writes to append blobs" [S1], so the digest account takes append-blob writes.
2. The Microsoft 15-minute storage commitments found are for block blobs. The geo priority replication SLA "applies exclusively to Block Blob data" and excludes accounts with Append or Page Blob API calls in the last 30 days [S2]. Object replication supports block blobs only [S3]; its priority replication feature is documented for block blobs [S4].
3. Azure Confidential Ledger (ACL) replicates to the region pair with no option to disable [S5]. No replication-lag figure was found, and the SLA excerpt read is about availability, not recovery point [S9].
4. Independent SLAs do not combine into an end-to-end RPO. Region, pairing and failover behaviour still have to be designed and tested.
5. A manual/custom export path could write digests as block blobs, which is eligible for the 15-minute geo-priority SLA in principle. That is a custom build with its own gaps (section 5).

Evidence ends at documentation. Whether the proposed accounts and configuration meet any figure cannot be proven from documents.

## 2. What actually bounds the digest RPO

- Automatic digests are generated at a 30-second interval and only if transactions occurred [S1].
- **With active geo-replication or failover groups, "Ledger will only issue database digests for data that has been replicated to geographic secondaries"**, so digests never reference data that could be lost in a geo-failover. This applies to automatic generation and storage only [S1]. Consequence: SQL replication lag, not storage, bounds how current stored digests can be.
- Active geo-replication is asynchronous and data loss is "unavoidable if the primary fails". Lag is observable via `replication_lag_sec`, and `sp_wait_for_database_copy_sync` blocks until the last commit is hardened on the secondary [S10] (read through a search excerpt only).
- In a failover group, "both primary and secondary database will have the same digest path. Even when you perform a failover, the digest path doesn't change". Secondaries need the same RBAC on the Azure Storage account [S1]. The docs do **not** state how the digest-upload setting itself is replicated to a geo-secondary. That must be tested, not assumed (section 7).
- Each point-in-time restore is tagged with a new create time, giving separate "incarnations" in digest paths [S1].

Implication: there are two separate gaps. (a) A data-loss gap, bounded by SQL replication or backup, outside this ticket. (b) A digest-coverage gap: committed ledger blocks after the last digest that survives in storage. Gap (b) loses no data but shortens the history that can be verified against a stored digest. Documentation gives no guarantee that gap (b) stays below 15 minutes.

## 3. Option comparison

| Dimension | Automatic digest to immutable Blob (append pattern) | Automatic digest to Azure Confidential Ledger | Manual / custom export |
|---|---|---|---|
| Supported by SQL Database | Yes, portal/PowerShell/CLI. Container `sqldbledgerdigests`, path `ServerName/DatabaseName/CreationTime` [S1], [S6] | Yes, same configuration; server identity needs the ACL Contributor role [S1], [S6] | Yes. `sp_generate_database_ledger_digest` returns a JSON row to store in "any service or device that you consider a trusted storage destination" (example: on-premises WORM) [S1] |
| Blob type written | Policy must allow "protected append writes to append blobs" and be locked [S1]. Docs do not describe each digest file's blob type directly; append use is inferred from this wording | Not blob storage. ACL transactions with Merkle receipts [S7] | Your choice. Block blobs make the 15-minute SLAs possible |
| Tamper resistance | Locked time-based retention blocks modify/delete even for account admins [S8]. Microsoft says ACL has "a stronger integrity guarantee" against privileged administrators [S1] | TEE-backed, append-only; the service "ensures no one — not even Microsoft — is 'above' the ledger" [S7] | Depends on the destination and your controls |
| Cross-region replication | GRS/GZRS asynchronous to the pair; RPO can be non-zero [S11] | Automatic replication to the regional pair via GRS and Azure Files. "You can't modify or disable this replication behavior" [S5] | Your design |
| Documented 15-minute commitment | **No for appended data.** Geo priority replication SLA (15 min, 99.0%) is block-blob only and excludes accounts with Append/Page Blob calls in the previous 30 days [S2]. Object replication does not support append blobs [S3] | **None found.** Overview says files "are automatically replicated to a secondary storage account periodically" with no figure [S7]. SLA excerpt defines unavailable minutes for transactions [S9] | Possible only if every digest is a block blob in a geo-priority account with no append calls, and the eligibility conditions hold [S2], [S4] |
| Effect of failover | Customer-managed unplanned storage failover disables geo priority replication and converts GZRS to LRS [S2], [S12] | "Microsoft might initiate a failover to the paired region" [S5] | n/a |
| Pair / EU posture | Pair of Sweden Central is restricted-access Sweden South [S13] | Same pair, automatic [S5], [S13] | Your choice, subject to section 6 |

## 4. Storage facts

- **GRS/GZRS** copy asynchronously to the paired region; the gap is the RPO. "Azure Storage offers Geo priority replication, which ensures the RPO for Block Blobs is less than or equal to 15 minutes" [S11].
- **Geo priority replication SLA:** Last Sync Time within 15 minutes for 99.0% of the billing month, block blob data only [S2]. Exclusions: feature not enabled; **Append or Page Blob API calls within the last 30 days**; initial Last Sync Time above 15 minutes; transfer above 35 Gbps or 100 CopyBlob requests per second [S2]. An unplanned failover automatically disables it [S2].
- **Object replication** supports "block blobs only; it doesn't support append blobs and page blobs". Customer-managed failover is not supported for source or destination, nor hierarchical-namespace accounts [S3]. Priority replication gives 99.0% within 15 minutes for "supported workloads", with exclusions such as objects over 5 GB [S4].
- **Immutability** is supported on all redundancy configurations and recommended mainly for block and append blobs [S8].
- After an unplanned storage failover the account becomes LRS and geo-redundancy must be re-enabled; re-copy "might take a significant amount of time" [S12].

## 5. Custom export path (documented hook, but a build)

The only documented hook is `sp_generate_database_ledger_digest` plus "any service or device that you consider a trusted storage destination" [S1]. A possible shape, **not a Microsoft-documented pattern**: write each digest as a separate block blob in a GZRS geo-priority account under a locked policy, with no append calls. Open points:

- Whether such an account meets all SLA eligibility including the 30-day append look-back [S2].
- Whether manual digests carry the replicated-data guard. The docs say it "only applies for automatic generation and storage" [S1], so a manual digest could reference data not yet on the secondary.
- Who runs the exporter, its identity and failure modes. The storage SLA covers replication lag, not the export schedule.

## 6. Location and EU Data Boundary

- Sweden Central is paired with **Sweden South**, a restricted-access region [S13].
- West Europe is not the pair of Sweden Central; the pair table lists North Europe with West Europe [S13].
- ACL pair replication "can't be modified or disabled", and the docs tell readers to ensure the primary and paired regions meet compliance requirements [S5].
- **Not verified:** ACL availability in Sweden Central and West Europe (products-by-region page [S14] was not readable here). Do not treat the proposed regions as eligible until checked.
- **Not verified:** EU Data Boundary status of ACL and of Sweden South as an ACL replica location. No EUDB page was read for this ticket.

## 7. Cannot be proven from documentation

1. An end-to-end figure combining SQL replication, digest upload and storage geo lag. Independent SLAs are not an end-to-end guarantee. The Business Critical geo-replication RPO SLA figure was taken from a sibling research note, not re-read here, and concerns replicated data, not digests.
2. Whether the digest-upload configuration replicates to a geo-secondary or must be re-enabled after failover. Docs cover digest path continuity [S1], not the setting. Needs a non-production test.
3. ACL replication lag to its pair.
4. Whether append blobs count in GRS Last Sync Time and how that relates to the block-blob-only Geo Blob Lag metric [S2].
5. The current ACL SLA text. Only a June 2024 excerpt was read (a minute is unavailable if transactions fail or take over 5 seconds) [S9]. No RPO term appeared in the excerpt; the full document was not read.
6. ACL regional availability and EUDB status (section 6).

## 8. Recommendation

1. **Do not state "digests within 15 minutes" as a requirement or SLA-backed property.** Mark it *unverified*: the 15-minute storage SLA does not cover append-blob digests, and the other options have no recovery-point commitment.
2. **Needs an explicit scope decision**, one of: (a) keep digests outside the 15-minute RPO and define a separate, non-SLA digest objective with a post-failover step (generate a fresh digest on the new primary and verify from the last surviving digest); (b) fund a custom block-blob export path and its proof; (c) drop the digest scope. Suggested default: (a), unless the business case needs (b).
3. Keep the records and evidence RPO separate from the digest objective in #24; data RPO follows SQL replication, not storage.
4. Before relying on any option, verify in a non-production test: digest-setting behaviour on failover, ACL region and EUDB eligibility, and measured Last Sync Time on the digest account.
5. Prefer automatic Blob with a locked immutability policy as the baseline when pair control matters, since ACL pair replication cannot be disabled [S5]. Revisit if ACL is confirmed available and EUDB-eligible.

## 9. Sources

- [S1] Digest management (SQL Server / Azure SQL): https://learn.microsoft.com/en-us/sql/relational-databases/security/ledger/ledger-digest-management (updated 2026-10-19)
- [S2] Azure Storage Geo Priority Replication: https://learn.microsoft.com/en-us/azure/storage/common/storage-redundancy-priority-replication
- [S3] Object replication overview: https://learn.microsoft.com/en-us/azure/storage/blobs/object-replication-overview
- [S4] Object replication priority replication: https://learn.microsoft.com/en-us/azure/storage/blobs/object-replication-priority-replication
- [S5] Data residency for Azure confidential ledger: https://learn.microsoft.com/en-us/azure/confidential-ledger/data-residency ; FAQ: https://learn.microsoft.com/en-us/azure/confidential-ledger/faq
- [S6] Enable automatic digest storage: https://learn.microsoft.com/en-us/sql/relational-databases/security/ledger/ledger-how-to-enable-automatic-digest-storage
- [S7] About Azure confidential ledger: https://learn.microsoft.com/en-us/azure/confidential-ledger/overview
- [S8] Immutable storage for Blob data: https://learn.microsoft.com/en-us/azure/storage/blobs/immutable-storage-overview
- [S9] Microsoft Online Services consolidated SLA (excerpt via search, June 2024 edition): https://www.microsoft.com/licensing/docs/documents/download/OnlineSvcsConsolidatedSLA(WW)(English)(June2024)(CR).docx
- [S10] Active geo-replication overview: https://learn.microsoft.com/en-us/azure/azure-sql/database/active-geo-replication-overview?view=azuresql
- [S11] Azure Storage data redundancy: https://learn.microsoft.com/en-us/azure/storage/common/storage-redundancy
- [S12] Customer-managed unplanned failover: https://learn.microsoft.com/en-us/azure/storage/common/storage-failover-customer-managed-unplanned
- [S13] Azure region pairs: https://learn.microsoft.com/en-us/azure/reliability/regions-paired
- [S14] Products by region, ACL filter (not read): https://azure.microsoft.com/en-gb/explore/global-infrastructure/products-by-region/?products=confidential-ledger
