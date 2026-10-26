# Tribute Technology (tributetech)

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Tribute Technology is a funeral-home technology company (Middleton, Wisconsin) serving over 9,000 funeral homes across the US and Canada. Its platform spans obituary publishing, memorial websites, funeral-home management software, online payments and funeral funding (Tribute Pay), and e-commerce (flowers and personalized products) through the Tribute Store.

For partners, Tribute Technology exposes the **Tribute Store API** - a partner-gated, REST-style JSON API that lets funeral-home case-management systems authenticate a funeral home, push its serving locations (rooftops), and push obituary cases. Each successfully posted obituary automatically provisions a personalized Tribute Store page for the deceased, reachable at the store base URL with a `?oId={OBITUARY_ID}` query string.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/tributetech/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/tributetech/refs/heads/main/apis.yml)

## Access Model

This is **not** a public, self-service API. There is no open developer portal, signup, or API-key issuance on tributetech.com. Access is provisioned by Tribute Technology through a partner/integration agreement:

- An integrator receives a `{Provider}` credential string plus an **IP allowlist** - requests must originate from an allowlisted server.
- Each funeral home receives a `{HostName, UserName, Password}` triple.
- The triple is exchanged at `POST /token/` for a short-lived **HTML bearer token** that scopes all subsequent requests to that one funeral home.

The endpoint surface documented here is drawn verbatim from the publicly circulated **"Tribute Store API Documentation 1.1"** (updated March 25, 2019), a PDF distributed to integration partners. The API is not callable without provisioned credentials and an allowlisted IP, so the endpoints in the OpenAPI definition are marked `endpointsModeled: true`. Response object schemas beyond the documented sample fields are omitted rather than fabricated.

**Base URLs** (per the 2019 documentation): production `http://api.tributecenteronline.com/`, development `http://api.demo.tributecenteronline.com/`. The definitions here use `https://` for the base; confirm the current scheme during partner onboarding.

## Tags

- Funeral Technology
- Obituaries
- Memorials
- Funeral Homes
- E-commerce
- Death Care
- Case Management

## Timestamps

- **Created:** 2026-07-03
- **Modified:** 2026-07-03

## APIs

### Tribute Store Authentication API

Exchange a funeral home's HostName/UserName/Password triple (scoped to your integrator Provider credential) for a short-lived HTML bearer token via `POST /token/`. The token identifies the funeral home on all other endpoints and requires an allowlisted source IP.

- **Base URL:** `https://api.tributecenteronline.com`
- Endpoints: `POST /token/`

### Tribute Store Serving Locations API

Create or update funeral-home serving locations (rooftops) that obituaries attach to, retrieve a location, list all locations, and list the predefined location types.

- **Base URL:** `https://api.tributecenteronline.com`
- Endpoints: `POST /api/external-location/post`, `GET /api/locations/{id}`, `GET /api/locations/`, `GET /api/locations/getlocationtypes`

### Tribute Store Obituaries API

Push obituary cases - the deceased, obituary text, service events (visitation, service), and a base64 photo thumbnail - using your `CaseId` as an add/update key. Each success returns a Tribute Store `OBITUARY_ID` and auto-provisions a personalized store page. Retrieve a single obituary or list obituary summaries with OData paging/sorting (`$orderby`, `$skip`, `$top`).

- **Base URL:** `https://api.tributecenteronline.com`
- Endpoints: `POST /api/external-case/post`, `GET /api/obituaries/GetObituary/{id}`, `GET /api/obituaries`

## Artifacts

- [OpenAPI](openapi/tributetech-openapi.yml)
- [Postman Collection](collections/tributetech.postman_collection.json)
- [Open Collection](collections/tributetech.opencollection.json)
- [Plans / Pricing](plans/tributetech-plans-pricing.yml)
- [Rate Limits](rate-limits/tributetech-rate-limits.yml)
- [FinOps](finops/tributetech-finops.yml)
- [Review](review.yml)

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/tributetechnology)
- [Website](https://www.tributetech.com/)
- [Documentation (Tribute Store API 1.1 PDF)](https://awheeler.funeraltechweb2.com/additional-service-info/file/3/Tribute%20Store%20API%20Documentation%201.1.pdf)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
