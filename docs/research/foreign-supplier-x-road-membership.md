# Foreign supplier eligibility for X-Road membership and hosted Security Servers

Research note for issue #21. Sources accessed 2026-11-24. This is a reading of primary sources, not legal advice.

## Question

Can a supplier established outside Estonia qualify as an X-Road (X-tee) member and use its own or a formally hosted/shared Security Server for supplier-to-government procurement exchanges under current Estonian rules and X-Road operational requirements?

## Short answer

- **Membership: yes, conditionally.** The governing regulation defines an X-tee member as "an agency or person that has joined X-tee" and sets no nationality or Estonian-registration requirement [R105 § 2 p 2]. RIA operates a dedicated member class for foreign organisations (`NEE`) [RIA-ENV], and the live EE production configuration lists 50 `NEE` members [GCONF]. Admission is at RIA's discretion: RIA may refuse an applicant that lacks a unique identifier to which a compliant e-seal certificate can be issued, does not provide documents proving its right of representation, submits untrue data or does not meet other requirements [R105 § 6]. A voluntary member that is not legally obliged to use X-tee can also be terminated on 30 days' notice [APP Annex 1, cl. 11.3].
- **Own Security Server: yes, but by default it must be hosted on territory under Estonian jurisdiction** [R105 § 8(3) p 4]. Hosting outside that territory is allowed "only with RIA's permission" (*üksnes keskuse loal*), and then the member must have an independent security audit at least every **2 years** instead of every 4 [R105 § 8(5); § 5(4) p 3]. The production configuration shows 8 Security Servers owned by foreign members [GCONF], but it does not show where they are hosted or whether § 8(5) permission was granted.
- **Hosted or shared Security Server: yes.** Sharing a Security Server with another member is expressly regulated [R105 § 8(6)], and RIA documents the hosted Security Server as an official usage model with a list of commercial hosts whose target group is "Piiranguteta" (unrestricted) [RIA-HOST]. The member keeps responsibility for the delegated duties [RIA-OBL]. In production, 24 foreign members are registered as clients on Security Servers owned by Estonian hosting companies [GCONF].
- **Procurement use is not settled by these sources.** Membership only gives access to the channel. Each exchange also needs a data service provider that offers a suitable service and agrees to serve the supplier [R105 § 12]. No primary source was found showing that an Estonian procurement authority or the public procurement register exposes X-tee services to suppliers.

## Legal requirements

Source: Government of the Republic Regulation No. 105 of 23.09.2016, "Infosüsteemide andmevahetuskiht", issued under Public Information Act § 43^9(1) p 5. The current wording has applied since 01.05.2026 (RT I, 28.04.2026, 13) [R105].

| Topic | Provision | Effect for a foreign supplier |
|---|---|---|
| Who can be a member | § 2 p 2: a member is "an agency or person that has joined X-tee" (*asutus või isik, kes on liitunud X-teega*) | The text has no establishment, nationality or Estonian-registry requirement. |
| Joining | § 5: application to RIA and a joining agreement | The same procedure applies to all applicants. |
| Member duties | § 5(4), including an independent security audit at least every 4 years and compliance with RIA instructions | Apply in full. |
| Refusal grounds | § 6: the applicant has no unique identifier to which an e-seal certificate meeting RIA's published requirements can be issued; does not provide documents proving its right of representation; submits untrue data; or does not meet other requirements | The identifier and e-seal conditions are the practical gate for foreign entities. |
| Security Server | § 8(1): install Security Server software and register its authentication certificate | Applies. |
| Jurisdiction of the server | § 8(3) p 4: host the Security Server "on territory under the jurisdiction of the Republic of Estonia" | This is the default rule. |
| Foreign hosting | § 8(5): outside that territory only with RIA permission, if § 5(4) duties are met and security measures are independently audited at least every 2 years | This is an exception and RIA decides it. |
| Shared server | § 8(6): when sharing its Security Server with another member, a member must use an encrypted connection and two-way authentication between Security Server and subsystem | A hosted/shared server is lawful. |
| RIA-provided server | § 8(4): additional software-update duties when using a Security Server offered by RIA | RIKS and Riigipilv are for state bodies only [RIA-HOST], so this does not apply to suppliers. |
| E-seal and trust services | § 9: integrity is ensured with an e-seal using eIDAS (EU 910/2014) trust services; § 9(1^1) allows an RIA-issued e-seal certificate | eIDAS applies across the EU, but see the unverified points on which certification authorities are configured. |
| Subsystems | § 10: register subsystems and name a responsible person and Security Server administrator | Applies. |
| Service use | § 12: a service-use agreement is needed; § 12(2) p 2: the provider checks a private-law legal person's security measures first | Each government service provider decides whether to serve the supplier. |
| Mediation | § 13: a member may give external natural or legal persons access through its subsystem under a published mediation procedure | Alternative route (see below). |
| Termination | § 14: RIA may end membership | Applies. |

RIA's general terms (Annex 1 of the membership application) state that "State and local government authorities, legal persons, and other subjects of law established on the basis of the law can subscribe to X-tee" [APP]. They do not say *Estonian* law. Clause 11.3 lets RIA terminate with 30 days' notice "if the member is not obliged by law to organise data exchange via X-tee" [APP]. A foreign supplier that joins voluntarily therefore has no assured right to remain a member.

## RIA operational requirements

- **Member class.** RIA lists `GOV`, `COM` and `NGO` for Estonian bodies and `NEE` for agencies and companies of other countries in the EE (production), ee-test and ee-dev environments [RIA-ENV]. The production global configuration describes `NEE` as "Foreign organizations" [GCONF].
- **Member code format (observed only).** Production `NEE` member codes follow the pattern `NTR<ISO country code>-<foreign registry number>`, for example `NTRDE-HRB…`, `NTRFI-…` and `NTRMT-C…` [GCONF; CAT]. RIA does not publish this convention on the pages reviewed; it is inferred from live data.
- **Joining route.** Heads of entities registered in the Estonian Business Register join through the x-tee.ee self-service portal. For "foreigners who cannot log in to x-tee.ee", RIA can exceptionally send the joining agreement by email [RIA-SIGN]. The English application form asks for name, registration number, address and representative [APP].
- **Certificates.** The certification services configured in EE are SK ID Solutions (qualified) and RIA (non-qualified). Timestamping comes from SK and RIA [RIA-ENV].
- **Hosted Security Server.** The client (the member) keeps member obligations, obtains its signing (SIGN) certificate and registers its subsystems. The host operates the server and obtains the authentication (AUTH) certificate and OCSP/TSA services. The two parties sign a contract that delegates the duties [RIA-HOST]. Hosts listed with target group "Piiranguteta" (unrestricted) are Novian Eesti OÜ, SpinTEK AS, OIXIO IT AS, Telia Eesti AS, Almic OÜ and Helmes AS. RIKS and Riigipilv serve state bodies only [RIA-HOST].
- **Responsibility.** Duties may be delegated but the member remains responsible, including for a hosted Security Server. Deploying an information system outside Estonia must be agreed with RIA. The member confirms compliance with information-security requirements [RIA-OBL].
- **Indicative costs.** Membership is free [APP]. RIA gives indicative annual costs: timestamping (TSA) about €430; OCSP about €450 per certificate; AUTH certificate about €90; SIGN certificate about €150; installation about €200; administration about €1,500 [RIA-COST].

### Observed production practice (EE global configuration, 2026-11-24)

| Item | Count | Source |
|---|---|---|
| `NEE` members in shared parameters | 50 (of 2,278 members) | [GCONF] |
| `NEE` members with catalogued subsystems | 30 (34 subsystems) | [CAT] |
| Security Servers owned by `NEE` members | 8 (of 220) | [GCONF] |
| `NEE` members registered as clients on Security Servers owned by Estonian `COM` hosts (Almic, Novian, Telia) | 24 | [GCONF] |

These figures show that RIA admits foreign members in practice, both with their own servers and with hosted servers. Most visible `NEE` members seem to be gambling or identity-service operators using regulator-facing subsystems. No procurement-related `NEE` subsystem was found.

## Alternatives that do not make the supplier an X-tee member

- **Service mediation.** A member acts as mediator for non-members [R105 § 13; RIA-MED], using the third-party representation extension of the X-Road protocol [XRD-3P]. For example, clients of TEHIK MISP "do not need to be X-tee members" [RIA-MED].
- **Federation with Finland.** EE and FI have been federated since 1 September 2017. Membership of Finland's Palveluväylä is for organisations registered in Finland, and RIA notes that Estonian X-tee membership is a precondition for cross-border exchange on the Estonian side [RIA-FI; FED]. This only helps suppliers registered in Finland.

## Implications for this project's end-to-end X-Road constraint

The points below follow from this project's design choice. They are not legal requirements.

- The law allows a foreign supplier to be a direct member. The project's end-to-end constraint is therefore compatible with Estonian rules, **provided** the supplier gets `NEE` membership and a Security Server (own or hosted) and RIA does not refuse it.
- **Mediation would break the constraint.** The supplier would not be an X-tee member, and a mediator would seal and send messages on its behalf. The law allows this, but the project's constraint excludes it.
- A **hosted Security Server** is the lowest-friction route that keeps the constraint. The supplier is the member, with its own SIGN key and subsystem, and the Estonian host runs the server within Estonian jurisdiction, so no § 8(5) permission is needed.
- An **own Security Server abroad** depends on RIA's discretionary permission and a 2-year audit cycle. The project should not assume this route is available.
- Every supplier-to-government exchange also needs a government data service provider that offers the service and agrees to it under § 12. The supplier does not control this.

## Still unverified

1. **Identifier and code scheme.** RIA does not publish a rule for `NEE` member codes. The `NTR<CC>-<registry no.>` pattern is inferred from production data. It is unknown whether an entity without a national business-register number (for example, one with only a VAT number) is accepted.
2. **E-seal certificates for foreign entities.** It is not confirmed whether SK ID Solutions or RIA issue SIGN certificates to any foreign entity, or what identity checks apply. It is also open whether a qualified e-seal certificate from a non-Estonian eIDAS trust service provider is accepted, since only SK and RIA are listed as configured CAs [RIA-ENV].
3. **Foreign hosting permission (§ 8(5)).** RIA does not publish its criteria, process or grant record. The global configuration does not show where `NEE`-owned servers are located or whether permission was granted.
4. **Host acceptance.** All listed hosts are marked unrestricted. Production data confirms Almic, Novian and Telia host foreign members. Whether SpinTEK, OIXIO and Helmes would accept a foreign client is unverified.
5. **"Established on the basis of the law" (APP Annex 1).** The terms do not say whether this means Estonian law or any law. Production practice suggests foreign incorporation is enough.
6. **Procurement services.** No primary source was found showing that the public procurement register (*riigihangete register*) or any contracting authority offers X-tee services to suppliers, or what agreements they would require.
7. **Retention risk.** How RIA applies clause 11.3 to voluntary foreign members is unknown.
8. **Freshness.** The EE–FI federation page and the list of hosting providers were each read once, on 2026-11-24. They were not checked against the appendices of the federation agreement.

## Sources

- **[R105]** Vabariigi Valitsus, määrus nr 105 "Infosüsteemide andmevahetuskiht", RT I, 28.04.2026, 13 (in force from 01.05.2026): https://www.riigiteataja.ee/akt/128042026013 — current version: https://www.riigiteataja.ee/akt/127092016004?leiaKehtiv
- **[APP]** RIA, X-tee application for membership and general terms (Annex 1): https://abi.ria.ee/__attachments/a_c76a12f04fb529184c2bb493713d9091790b17517f4515846e3ff1d902e0c3b7/X-tee%20application%20for%20membership.docx
- **[RIA-SIGN]** RIA, "Liitumislepingu allkirjastamine": https://abi.ria.ee/xtee/liitumislepingu-allkirjastamine
- **[RIA-ENV]** RIA, "X-tee keskkonnad": https://abi.ria.ee/xtee/x-tee-keskkonnad
- **[RIA-HOST]** RIA, "Majutatud turvaserver": https://abi.ria.ee/xtee/majutatud-turvaserver
- **[RIA-OBL]** RIA, "X-tee liikme kohustused": https://abi.ria.ee/xtee/x-tee-liikme-kohustused
- **[RIA-MED]** RIA, "Teenuste vahendamine": https://abi.ria.ee/xtee/teenuste-vahendamine
- **[RIA-COST]** RIA, "X-tee kasutamisvõimaluste ja kulude ülevaade": https://abi.ria.ee/xtee/x-tee-kasutamisvoimaluste-ja-kulude-ulevaade
- **[RIA-FI]** RIA, "Usaldusföderatsioon Soomega": https://abi.ria.ee/xtee/usaldusfoderatsioon-soomega
- **[FED]** RIA–VRK Trust Federation Agreement: https://www.ria.ee/sites/default/files/documents/2022-11/RIA-VRK-Trust-federation-Agreement.pdf
- **[RIA-EN]** RIA, "Data exchange layer X-tee" ("Companies and agencies in the private and public sector can join X-tee"): https://www.ria.ee/en/state-information-system/data-exchange-platforms/data-exchange-layer-x-tee
- **[XRD-3P]** X-Road, "Third party representation extension for the X-Road protocol": https://www.x-tee.ee/docs/live/xroad/pr-third_party_representation_extension_for_the_x-road_protocol.html
- **[GCONF]** EE production global configuration, shared parameters. Directory: http://cs1.ee.x-tee.ee/internalconf; file `/V2/20261124162841419644000/shared-params.xml`; retrieved 2026-11-24.
- **[CAT]** X-tee subsystems and methods catalogue data: https://x-tee.ee/catalogue-data/EE/index.json (UI: https://x-tee.ee/catalogue/EE); retrieved 2026-11-24.
