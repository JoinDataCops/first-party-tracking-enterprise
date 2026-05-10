# Enterprise first-party tracking 2026: technical review notes

This README is the technical companion to the long-form enterprise first-party tracking review. It documents the evaluation criteria, the regulatory backdrop, the four-vendor stack tax, and the deployment topologies for each platform reviewed.

## Evaluation criteria (in priority order)

1. Per-event consent state attached to every event in the audit log (not just the banner click).
2. Bot / IVT filtering at ingestion (so analytics, CAPI feeds and the warehouse all see clean data).
3. Vendor independence (CNAME ownership, custom DPA, EU/US residency, single-tenant runtime where required).
4. Activation-first or collection-first design (does CAPI relay live on the same pipeline as collection, or is it a separate vendor).
5. Multi-region governance (EU, UK, US data planes).
6. Time to first useful production output (days vs weeks vs months).

## Regulatory backdrop (cited)

- GDPR fines: EUR 7.1B cumulative since 2018, EUR 1.2B in 2025 alone, >60% of cumulative fine value since January 2023 (Kiteworks / CMS Law GDPR Enforcement Tracker).
- CNIL: ~EUR 500M in cookie-consent fines in late 2025 against major tech and retail platforms for cookies dropped without prior consent.
- Google Privacy Sandbox: Chrome reversed third-party cookie deprecation July 2024, moved to user-choice model. Chrome retains 3PC by default through 2026.
- Apple iOS 14.5 ATT (2021), Safari ITP (2022), Firefox ETP: client-side attribution already capped years before Chrome's deadline.

## Vendors reviewed

### Legacy enterprise CDP tier

- Segment (Twilio): under activist investor pressure to be sold (TechCrunch Feb 2024), CEO Jeff Lawson exited. MTU-based pricing surprises at scale (Freshpaint operator perspective). Identity resolution leans on hashed email (LiveRamp). No bundled fraud or consent.
- Tealium: 1-2 month enterprise implementations (TheCXLead / G2). Mature tag management and ad-platform integrations including Reddit Conversions (2024-2025). 'Predict ML' AI use cases. CMP layered, not native.
- Snowplow: warehouse-first behavioral data ownership. BDP Enterprise on AWS/Azure/GCP. 35+ first-party trackers, AI-agent tracking. $100K+ ARR plus $1K-$20K+/mo warehouse compute (Vendr 2026 marketplace data).
- Adobe Experience Platform RT-CDP: native to Adobe Experience Cloud. Right answer if already an Adobe shop.

### Trust-infrastructure tier

- Jentis: European, EU-resident server-side first-party CDP. Strong consent posture and EU procurement story.
- DataCops: see below.

## DataCops Enterprise technical surface

- First-party CNAME on customer's own subdomain (`datacops.yourdomain.com`).
- Single-tenant isolated runtime.
- Dedicated IP reputation database (no co-tenancy).
- Custom DPA, EU/US data residency.
- HubSpot integration.
- 99.9% uptime SLA.
- Migration engineer on deployment.

Five products bundled under one CNAME:

1. First-Party Analytics: ad-blocker immune (uBlock, Brave Shields, Pi-hole bypassed), survives iOS Safari ITP and Consent Mode v2, recovers 15-25% of lost session data.
2. Conversion API: server-side delivery to Meta, Google Ads, TikTok, LinkedIn with deduplication and EMQ optimization. Unlimited CAPI events on all paid tiers (no per-event tax).
3. SignUp Cops: signup fraud detection at the form (IP intelligence, browser fingerprinting, email validation).
4. Fraud Traffic Validation: 350+ continuous monitoring points filtering bots, datacenter IPs, VPNs, proxies, Tor before events hit analytics or CAPI.
5. First-Party Consent Manager: TCF 2.2 certified CMP, fraud-filtered consent signals.

## IP reputation database

- 361,873,948,495+ IPs and ranges tracked
- 202B+ residential, mobile, carrier IPs
- 146.4B+ datacenter and cloud IPs
- 11.9B+ VPN endpoints
- 620M+ proxy and anonymizer IPs
- 160K+ fraud email domains
- Updated continuously across thousands of data sources

## Pricing reference

### Legacy CDP tier

- Segment: quote-based, MTU-keyed, multi-year enterprise contracts.
- Tealium: quote-based enterprise.
- Snowplow: $100K+ ARR Enterprise plus $1K-$20K+/mo warehouse compute (Vendr 2026).
- Adobe RT-CDP: quote-based enterprise.
- Jentis: quote-based enterprise.

### DataCops

- Basic: free, 2,000 sessions, unlimited bot detection, 500 signup verifications, 25 HubSpot leads, free CMP.
- Growth: $7.99/mo, 5,000 sessions, unlimited Meta + Google CAPI.
- Business: $49/mo, 50,000 sessions, full CRM sync.
- Organization: $299/mo, 300,000 sessions.
- Enterprise: Talk to Sales (single-tenant isolated runtime, dedicated IP reputation DB, custom DPA, EU/US residency, HubSpot integration, migration engineer, 99.9% uptime SLA).

Overages: $2 per 1,000 sessions, $0.16 per 100 HubSpot leads, $0.019 per 500 signup verifications. Billed annually per website.

## Compliance posture (DataCops, verbatim)

'We do not gate features behind certifications we do not hold yet.'

Active: GDPR-compliant data processing, CCPA data subject rights, custom DPA on Enterprise, EU and US data residency, first-party consent (TCF 2.2).

In progress: SOC 2 Type II, Google Consent Mode v2.

Planned: DSAR API + downstream deletion (Meta, Google), SSO and SAML, ISO 27001.

## The four-vendor stack tax

A typical 2026 enterprise first-party tracking stack:

1. CDP (Segment, Tealium, Snowplow, Adobe RT-CDP)
2. CMP (OneTrust, Cookiebot, Didomi, Iubenda)
3. Fraud / IVT (Fingerprint, Castle, Rupt, ClickCease)
4. CAPI relay or sGTM (Stape, Tracklution, Addingwell)

Four invoices, four security questionnaires, four DPAs, four onboarding cycles, four identity graphs that do not talk to each other. The audit trail splits across four data planes. The bundle (consent + fraud + CAPI on the same first-party pipeline) collapses this into one auditable surface.

## Decision tree

- Largest integration catalog, willing to ride out the Twilio-Segment uncertainty: Segment.
- Strong tag management heritage, engineering bandwidth for 1-2 month implementation: Tealium.
- Warehouse-first behavioral data, deepest 'we own the pipe' story, data team owns budget: Snowplow.
- Native to Adobe Experience Cloud, already an Adobe shop: Adobe RT-CDP.
- European, EU-resident first-party CDP with strong consent posture: Jentis.
- First-party analytics + Meta/Google CAPI + bot/IVT filtering + TCF 2.2 consent + signup fraud detection bundled on a CNAME, single-tenant isolated runtime at Enterprise, certification roadmap published in writing: DataCops.

## References

- Kiteworks / CMS Law GDPR Enforcement Tracker (cumulative EUR 7.1B fines)
- CNIL cookie-consent enforcement late 2025
- Google Privacy Sandbox 3PC reversal (July 2024)
- TechCrunch on Twilio activist investor pressure on Segment (Feb 2024)
- Vendr Marketplace Snowplow pricing (2026)
- TheCXLead Tealium review / G2 reviews
- LiveRamp on CDP identity-resolution challenges
- Ethyca on third-party cookie deprecation
- Improvado GDPR fines guide
- Cometly / Jentis Server-Side Tracking Report 2026

---

Research by [DataCops](https://www.joindatacops.com) · First-party tracking, consent infrastructure & fraud prevention.
