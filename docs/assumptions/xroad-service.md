# X-Road service scope and registration

**Status:** Planning assumption; ownership, registration, and authorization unconfirmed.

## Assumption

Plan to build both an authority-side registered procurement REST service and the ACA-hosted A2A/MCP gateway or adapter that uses it. The X-Road service is the registered member-subsystem REST API; it is distinct from the gateway. This is a planning choice, not confirmation that the authority has approved the service or that registration and access are available.

The existing architecture assumes supplier-to-government exchange uses X-Road and a synchronous registered REST adaptation. It does not assume native A2A or MCP transport over X-Road, streaming, or a non-X-Road fallback.

## Evidence and uncertainty

Research found a plausible synchronous REST adaptation for selected A2A/MCP operations, but no official A2A/MCP-over-X-Road integration guidance and no confirmed procurement-register supplier service. Authority ownership, service registration, membership, credentials, and authorization remain unresolved.

## Impact and validation

The service contract and operational ownership must be agreed before implementation can be treated as deployment-ready. Confirm the responsible authority-side owner, subsystem/service registration, and authorization path with the competent service owner and RIA before production. The owners and confirmation are unknown.

## Sources

- [Azure agent platform fit: hosting, interoperability and measurement](https://github.com/JoranBergfeld/procurement-automation/issues/18)
- [X-Road transport feasibility for supplier agent protocols](https://github.com/JoranBergfeld/procurement-automation/issues/19)
- [Planning assumptions: AI classification, records and X-Road prerequisites](https://github.com/JoranBergfeld/procurement-automation/issues/23)
