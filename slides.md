---
marp: true
theme: w3c
paginate: true
title: Linked Web Storage (LWS) WG — status & direction
description: W3C Member meeting, 17 Nov 2026
footer: 'W3C LWS WG · Member meeting · 17 Nov 2026'
---

<!-- _class: title -->
<!-- _paginate: false -->
<!-- _footer: '' -->

# Linked Web Storage (LWS) WG

## status & direction

W3C Member meeting · 17 Nov 2026

- Specs: <https://github.com/w3c/lws-protocol>
- Latest core WD: <https://www.w3.org/TR/2026/WD-lws10-core-20261109/>

---

# High-level goals / rationale

- Privacy
- User control of personal data
- Data interoperability across applications
- Ease of adoption


<!--- Let users pick their storage and identity provider independently of the apps they use-->
  <!--- Apps "bring the code"; users keep the data and the access decisions-->
<!--- Interoperability: one storage, many client apps — and one app, many storages-->
<!--- Charter: "secure and permissioned access to externally stored data in an interoperable way"-->
<!--- Reuse existing Web standards (HTTP, OAuth 2.0, CID, JSON-LD, AS2) rather than invent new ones-->
<!--- Deployable by mainstream Web/JSON developers — RDF-capable, not RDF-required-->

---

# Brief high-level tech

- HTTP resources in a containment hierarchy
- Data and service discovery
- Authentication: integration with OpenID Connect, SAML, self-asserted identity
- Authorization: OAuth 2.0 profile


<!--- HTTP resources in a [containment hierarchy](https://www.w3.org/TR/lws10-core/#logical-resource-organization): storage root → containers → data resources-->
  <!--- CRUD via HTTP; containment via `rel="up"` links + [container listing](https://www.w3.org/TR/lws10-core/#container-representation) (`items`)-->
  <!--- URIs are opaque: hierarchy is metadata, not path structure ([§](https://www.w3.org/TR/lws10-core/#resource-identification-0))-->
<!--- [Storage description](https://www.w3.org/TR/lws10-core/#storage-description) (a CID document) advertises the root and services-->
  <!--- discovered from any resource via `Link: <…>; rel="https://www.w3.org/ns/lws#storage"` ([§](https://www.w3.org/TR/lws10-core/#storage-description-binding))-->
<!--- [Authentication](https://www.w3.org/TR/lws10-core/#authentication): pluggable suites ([OpenID Connect](https://www.w3.org/TR/lws10-authn-openid/), [SAML 2.0](https://www.w3.org/TR/lws10-authn-saml/), [self-signed CID](https://www.w3.org/TR/lws10-authn-ssi-cid/))-->
<!--- [Authorization](https://www.w3.org/TR/lws10-core/#authorization): OAuth 2.0 profile; `401` + RFC 9728 [Protected Resource Metadata](https://www.w3.org/TR/lws10-core/#protected-resource-metadata) → token for the storage-->
<!--- [Metadata](https://www.w3.org/TR/lws10-core/#metadata): linkset auxiliary resources (RFC 9264)-->
<!--- Optional services: [notifications](https://www.w3.org/TR/lws10-core/#notifications), [access requests/grants](https://www.w3.org/TR/lws10-core/#access-requests-and-grants), [search & type index](https://w3c.github.io/lws-protocol/lws10-index/)-->

---

<!-- _class: dense -->

<!--# Time-frame-->

<!--- Charter: 2024-09-09 → 2027-01-08-->
  <!--- chairs: Aaron Coburn, Laurens Debackere, Eric Prud'hommeaux; staff contact: Pierre-Antoine Champin-->
<!--- Charter plan vs. actual-->
  <!--- FPWD planned 2025-03 — core FPWD 2026-03-31; auth suites FPWD 2026-04-23-->
  <!--- CR planned 2026-03, Rec 2026-09 — core now at WD (2026-11-09)-->
<!--- Other publications-->
  <!--- Vocabulary: Group Note Draft 2026-07-14-->
  <!--- did:key auth suite: Discontinued Draft 2026-09-29 — subsumed by SSI-CID (a did:key *is* a CID; [#141](https://github.com/w3c/lws-protocol/issues/141), [PR #229](https://github.com/w3c/lws-protocol/pull/229))-->
<!--- ~150 commits / ~100 merged PRs in 2026-->
<!--- Next: rechartering (TAG charter review: [w3c/strategy#566](https://github.com/w3c/strategy/issues/566)), CR target Q2 2027-->
<!--- Implementations: 4+ servers (lws-server, Halcyon, sparq, Community Solid Server), clients (lws-client, sai-js)-->
  <!--- test suite: [lws-contrib/lws-test-suite](https://github.com/lws-contrib/lws-test-suite)-->

<!------->

<!-- _class: dense -->

# Relationship to Solid

- Solid is incubation — needs standardization
  - [Solid Protocol 0.11](https://solidproject.org/TR/2024/protocol-20240512) (CG report) is a charter *input*, not the output
  - Normative deps not Rec-track ready (WebID, Solid-OIDC, WAC/ACP, Solid Notifications)
- The WG puts the necessary components on Rec-track foundations
  - Identity and authentication → CID + pluggable authn suites
  - Authorization and sharing → [ODRL Access Profile](https://www.w3.org/TR/lws10-core/#access-profile)
  - Developer-friendly interoperability via JSON-LD based data model
- Community Solid Server is also an LWS implementation

---

<!-- _class: dense -->

# Relationship to LDP

- LWS operations are semantically the same as LDP CRUD
  - Proposal: identify a required subset of LDP while keeping developer friendly focus
  - Content negotiation for containers
  - Include paging support for containers

---

<!-- _class: dense -->

# Out of scope

  - From charter: "a unique and exclusive solution for Personal Data Stores"
  - New identity mechanisms
  - New data formats

<!--
- **Possible / under discussion**
  - HTTP Streaming notification suite (draft [PR #284](https://github.com/w3c/lws-protocol/pull/284))
  - Web-CID agent identification profile ([#96](https://github.com/w3c/lws-protocol/pull/96)); FedCM integration ([#279](https://github.com/w3c/lws-protocol/issues/279))
  - Auxiliary resources & manifest ([#113](https://github.com/w3c/lws-protocol/pull/113)); schema integration ([#294](https://github.com/w3c/lws-protocol/issues/294))
  - Protocol version discovery ([#291](https://github.com/w3c/lws-protocol/issues/291)); Solid migration Note ([#66](https://github.com/w3c/lws-protocol/issues/66))
-->

<!------->

<!-- _class: dense -->

<!--# Modularization-->

<!--- Core (`lws10-core`) + suites as separate documents-->
  <!--- Orthogonality concern raised ([#178](https://github.com/w3c/lws-protocol/issues/178), citing WebArch §5.1)-->
  <!--- → OAuth profile being extracted from core into its own spec ([PR #299](https://github.com/w3c/lws-protocol/pull/299))-->
<!--- Authentication suites: [OpenID Connect](https://www.w3.org/TR/lws10-authn-openid/), [SAML 2.0](https://www.w3.org/TR/lws10-authn-saml/), [SSI-CID](https://www.w3.org/TR/lws10-authn-ssi-cid/) (did:key folded in)-->
<!--- **[Access](https://www.w3.org/TR/lws10-core/#access-requests-and-grants)**-->
  <!--- Requests: agent POSTs an access request to an [`AccessRequestService`](https://www.w3.org/TR/lws10-core/#access-request-endpoint)-->
  <!--- Grants: storage controller issues an access grant via [`AccessGrantService`](https://www.w3.org/TR/lws10-core/#access-grant-endpoint)-->
    <!--- a grant is a *record*, not a token — server adjusts its own policy-->
    <!--- [access profile](https://www.w3.org/TR/lws10-core/#access-profile) drawn from ODRL (actions, constraints, assignees) ([#303](https://github.com/w3c/lws-protocol/issues/303))-->
<!--- **[Notifications](https://www.w3.org/TR/lws10-core/#notifications)**-->
  <!--- Core: `NotificationService` [discovery](https://www.w3.org/TR/lws10-core/#notification-discovery), [subscription model](https://www.w3.org/TR/lws10-core/#subscriptions), [notification data model](https://www.w3.org/TR/lws10-core/#notification-data-model)-->
  <!--- Delivery suites: [Webhooks](https://w3c.github.io/lws-protocol/lws10-notifications-webhook/); HTTP Streaming (draft)-->
<!--- Index: search & type index services ([`lws10-index`](https://w3c.github.io/lws-protocol/lws10-index/))-->
<!--- Same-origin services may reuse the storage's access token ([PR #305](https://github.com/w3c/lws-protocol/pull/305))-->

---

<!-- _class: dense -->

# Data structures used

<!--n.b. new data formats are [out of scope](https://www.w3.org/2024/09/linked-web-storage-wg-charter.html#out-of-scope)-->

- **CID** (Controlled Identifiers 1.0)
  - [Storage description](https://www.w3.org/TR/lws10-core/#storage-description-data-model) = CID document + LWS vocabulary
  <!--- [PR #306](https://github.com/w3c/lws-protocol/pull/306): drop `application/lws+cid` → plain `application/cid`-->
  - Verification mechanism for authentication suites
- **Activity Streams 2.0**: [notification envelope](https://www.w3.org/TR/lws10-core/#notification-data-model) wraps an AS2 `Create` / `Update` / `Delete` / `Move`
- **JSON-LD with a [profile](https://www.w3.org/TR/lws10-core/#lws-profile)**
  - Container data model

  <!--- [PR #307](https://github.com/w3c/lws-protocol/pull/307): `application/lws+json` → `application/ld+json; profile="https://www.w3.org/ns/lws/v1"`-->
  <!--- Any RDF syntax (Turtle, TriG, N-Triples…) via [conneg](https://www.w3.org/TR/lws10-core/#content-negotiation) if RDF-isomorphic to the profile-->
- **IETF building blocks**
  - OAuth 2.0 (RFC 6749)
  - Protected Resource Metadata (RFC 9728)
  - Web Linking (RFC 8288)
  - Linksets (RFC 9264)
  - Problem Details (RFC 9457)
- Vocabularies reused: ODRL, Dublin Core (`dcterms:format`, `modified`), schema.org (`schema:size`, `schema:expires`), `ldp:inbox`
<!--- Trend: removing the WG's own media types ([#267](https://github.com/w3c/lws-protocol/issues/267), [#276](https://github.com/w3c/lws-protocol/issues/276))-->

---

# Obligatory "Get involved" slide

- Repo & issues: <https://github.com/w3c/lws-protocol>
- Implementations: [Implementations.md](https://github.com/w3c/lws-protocol/blob/main/Implementations.md) — signal interest via PR
- Test suite: <https://github.com/lws-contrib/lws-test-suite>
- Hot right now: [#309](https://github.com/w3c/lws-protocol/issues/309) LDP alignment · [#303](https://github.com/w3c/lws-protocol/issues/303) ODRL access profile · [#291](https://github.com/w3c/lws-protocol/issues/291) version discovery
