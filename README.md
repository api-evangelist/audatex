# Audatex (audatex)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Audatex (part of Solera Holdings) provides automotive claims and repair solutions with data and technology services for the automotive insurance, collision repair, and fleet management industries. It offers the AudaConnect API platform for third-party integration with claims processing, damage assessment, repair cost estimation, and vehicle data workflows. APIs are RESTful with JSON/XML support and OAuth 2.0 authentication.

**URL:** [https://www.audatex.com](https://www.audatex.com)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - Automotive, Claims Processing, Insurance, Repair Management, Vehicle Data

## Timestamps

- **Created:** 2025-01-01
- **Modified:** 2026-04-19

## APIs

### Audatex AudaConnect API
The AudaConnect API enables third-party software developers to access, query, and update the Audatex platform including assessments, vehicle reference data, repair orders, and photo management using RESTful methods with OAuth 2.0.

**Human URL:** [https://audaconnect-demo.ax-aee.co.uk/AudaAPI.Portal/Home/About](https://audaconnect-demo.ax-aee.co.uk/AudaAPI.Portal/Home/About)

#### Tags:

 - Assessments, Claims, Insurance, Repair

#### Properties

- [Documentation](https://audaconnect-demo.ax-aee.co.uk/AudaAPI.Portal/Home/About)
- [OpenAPI](https://audaconnect-demo.ax-aee.co.uk/AudaAPI.Bmsapi/)
- [Authentication](https://audaconnect-demo.ax-aee.co.uk/AudaAPI.Portal/Home/About)

### Audatex GIC API
The Audatex GIC (Global Integration Component) API provides integration capabilities for claims processing and vehicle damage assessment workflows in the insurance and collision repair industries.

**Human URL:** [https://api-demo.audatex.com/TestGICapi/docs/index.html](https://api-demo.audatex.com/TestGICapi/docs/index.html)

#### Tags:

 - Claims, GIC, Insurance, Integration

#### Properties

- [Documentation](https://api-demo.audatex.com/TestGICapi/docs/index.html)

### Solera API Gateway
The Solera API Gateway provides access to Audatex and Solera claims processing services including ClaimImage document return and other automotive claims data APIs for North American insurance markets.

**Human URL:** [https://na.api.solera.com/](https://na.api.solera.com/)

#### Tags:

 - Claims, Documents, Insurance, Solera

#### Properties

- [Documentation](https://na.api.solera.com/)
- [Authentication](https://na.api.solera.com/)

## Common Properties

- [Website](https://www.audatex.com/)
- [Documentation](https://www.audatex.com/solutions/)
- [PrivacyPolicy](https://www.audatex.com/privacy-policy/)
- [TermsOfService](https://www.audatex.com/terms-and-conditions/)
- [Contact](https://www.audatex.com/contact/)

## Features

| Name | Description |
|------|-------------|
| Claims Assessment API | Search, download, upload, and amend vehicle damage assessments programmatically via the AudaConnect API. |
| Repair Cost Estimation | Access Audatex repair cost estimation data and labor rates for collision repair workflow automation. |
| Photo Management | Upload, retrieve, and manage vehicle damage photos attached to claims via the assessment API. |
| Repair Order Integration | Create, update, and query repair orders from bodyshop management systems via BMS API integration. |
| Vehicle Reference Data | Query vehicle reference data including make, model, trim, and VIN decoding for assessment setup. |
| OAuth 2.0 Security | All AudaConnect APIs are secured with OAuth 2.0 authorization for enterprise-grade access control. |

## Use Cases

| Name | Description |
|------|-------------|
| Insurance Claims Automation | Automate first notice of loss, damage assessment, and claims settlement workflows for auto insurers. |
| Bodyshop Management System Integration | Integrate bodyshop management systems with Audatex for repair order creation, parts pricing, and labor time. |
| Total Loss Determination | Access vehicle valuation and total loss thresholds to automate total loss claims decisions. |
| Digital Claims Submission | Enable digital submission of vehicle damage photos and assessment data from mobile apps to the Audatex platform. |

## Integrations

| Name | Description |
|------|-------------|
| Bodyshop Management Systems | Native integration with major BMS platforms for automated repair order and parts pricing workflows. |
| Insurance Core Systems | Integration with insurance policy and claims management systems for end-to-end claims processing. |
| Vehicle History Providers | Integration with vehicle history and VIN data providers for complete vehicle information at claims initiation. |
| Parts Suppliers | Connection to OEM and aftermarket parts supplier catalogs for parts pricing and availability in repair estimates. |

## Solutions

| Name | Description |
|------|-------------|
| Claims Process Automation | End-to-end automation of auto insurance claims from FNOL through repair authorization and settlement. |
| Repair Shop Workflow | Digital workflow management for collision repair shops integrating estimates, parts, labor, and customer communication. |

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
