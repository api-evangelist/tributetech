# Tribute Technology (tributetech)

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
