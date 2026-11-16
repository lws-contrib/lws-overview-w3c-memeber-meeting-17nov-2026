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

W3C Member meeting · 17 Nov 2026 · 16:00–17:00 EST

- Specs: <https://github.com/w3c/lws-protocol>
- Latest core WD: <https://www.w3.org/TR/2026/WD-lws10-core-20261023/>

---

# High-level goals / rationale

- Let users pick their storage and identity provider independently of the apps they use
  - Apps "bring the code"; users keep the data and the access decisions
- Interoperability: one storage, many client apps — and one app, many storages
- Charter: "secure and permissioned access to externally stored data in an interoperable way"
- Reuse existing Web standards (HTTP, OAuth 2.0, CID, JSON-LD, AS2) rather than invent new ones
- Deployable by mainstream Web/JSON developers — RDF-capable, not RDF-required

---

# Brief high-level tech

- HTTP resources in a containment hierarchy: storage root → containers → data resources
  - CRUD via HTTP; containment via `rel="up"` links + container listing (`items`)
  - URIs are opaque: hierarchy is metadata, not path structure
- Storage description (a CID document) advertises the root and services
  - discovered from any resource via `Link: <…>; rel="https://www.w3.org/ns/lws#storage"`
- Authentication: pluggable suites (OpenID Connect, SAML 2.0, self-signed CID)
- Authorization: OAuth 2.0 profile; `401` + RFC 9728 Protected Resource Metadata → token for the storage
- Metadata: linkset auxiliary resources (RFC 9264)
- Optional services: notifications, access requests/grants, search & type index

---

<!-- _class: dense -->

# Time-frame

- Charter: 2024-09-09 → 2027-01-08
  - chairs: Aaron Coburn, Laurens Debackere, Eric Prud'hommeaux; staff contact: Pierre-Antoine Champin
- Charter plan vs. actual
  - FPWD planned 2025-03 — core FPWD 2026-03-31; auth suites FPWD 2026-04-23
  - CR planned 2026-03, Rec 2026-09 — core now at WD (2026-10-23)
- Other publications
  - Vocabulary: Group Note Draft 2026-07-14
  - did:key auth suite: Discontinued Draft 2026-09-29 — subsumed by SSI-CID (a did:key *is* a CID; [#141](https://github.com/w3c/lws-protocol/issues/141), [PR #229](https://github.com/w3c/lws-protocol/pull/229))
- ~150 commits / ~100 merged PRs in 2026
- Next: rechartering (TAG charter review: [w3c/strategy#566](https://github.com/w3c/strategy/issues/566)), CR target Q2 2027
- Implementations: 4+ servers (lws-server, Halcyon, sparq, Community Solid Server), clients (lws-client, sai-js)
  - test suite: [lws-contrib/lws-test-suite](https://github.com/lws-contrib/lws-test-suite)

---

# Relationship to Solid

- Solid is incubation — needs standardization
  - Solid Protocol 0.11 (CG report) is a charter *input*, not the output
  - Normative deps not Rec-track ready (WebID, Solid-OIDC, WAC/ACP, Solid Notifications)
- The WG re-specifies the needed parts on Rec-track foundations
  - WebID + Solid-OIDC → pluggable authn suites + OAuth 2.0 profile
  - WAC/ACP → resource manager + access requests/grants (access policy language left open)
  - Turtle-first → **dual-access JSON-LD**: one document read as plain JSON *or* as RDF
    - other RDF syntaxes (Turtle, N-Triples…) via profile conneg, RDF-isomorphic
- Continuity: spec acknowledges Solid authors; Solid compatibility/migration Note planned ([#66](https://github.com/w3c/lws-protocol/issues/66))
- Community Solid Server is also an LWS implementation

---

<!-- _class: dense -->

# Ruled in / out / possible

- **In**
  - Core storage protocol: containers, CRUD, discovery, metadata, pagination
  - Authentication suites + OAuth 2.0 authorization profile
  - Notifications (data model + delivery suites)
  - Access requests & grants; search/type index services
  - Threat model; test suite + implementation report
- **Out** (charter)
  - "a unique and exclusive solution for Personal Data Stores"
  - New identity mechanisms; new data formats
  - Authorization-server ↔ storage-server interaction; a mandated access-policy language
- **Possible / under discussion**
  - HTTP Streaming notification suite (draft [PR #284](https://github.com/w3c/lws-protocol/pull/284))
  - Web-CID agent identification profile ([#96](https://github.com/w3c/lws-protocol/pull/96)); FedCM integration ([#279](https://github.com/w3c/lws-protocol/issues/279))
  - Auxiliary resources & manifest ([#113](https://github.com/w3c/lws-protocol/pull/113)); schema integration ([#294](https://github.com/w3c/lws-protocol/issues/294))
  - Protocol version discovery ([#291](https://github.com/w3c/lws-protocol/issues/291)); Solid migration Note ([#66](https://github.com/w3c/lws-protocol/issues/66))

---

<!-- _class: dense -->

# Modularization

- Core (`lws10-core`) + suites as separate documents
  - Orthogonality concern raised ([#178](https://github.com/w3c/lws-protocol/issues/178), citing WebArch §5.1)
  - → OAuth profile being extracted from core into its own spec ([PR #299](https://github.com/w3c/lws-protocol/pull/299))
- Authentication suites: OpenID Connect, SAML 2.0, SSI-CID (did:key folded in)
- **Access**
  - Requests: agent POSTs an access request to an `AccessRequestService`
  - Grants: storage controller issues an access grant via `AccessGrantService`
    - a grant is a *record*, not a token — server adjusts its own policy
    - access profile drawn from ODRL (actions, constraints, assignees) ([#303](https://github.com/w3c/lws-protocol/issues/303))
- **Notifications**
  - Core: `NotificationService` discovery, subscription model, notification data model
  - Delivery suites: Webhooks (`lws10-notifications-webhook`); HTTP Streaming (draft)
- Index: search & type index services (`lws10-index`)
- Same-origin services may reuse the storage's access token ([PR #305](https://github.com/w3c/lws-protocol/pull/305))

---

<!-- _class: dense -->

# Structures used

n.b. new data formats are [out of scope](https://www.w3.org/2024/09/linked-web-storage-wg-charter.html#out-of-scope)

- **CID** (Controlled Identifiers 1.0)
  - Storage description = CID document + LWS vocabulary
  - [PR #306](https://github.com/w3c/lws-protocol/pull/306): drop `application/lws+cid` → plain `application/cid`
  - SSI-CID auth suite (covers did:key); webhook signing keys as CID verification methods
- **Activity Streams 2.0**: notification envelope wraps an AS2 `Create` / `Update` / `Delete` / `Move`
- **JSON-LD with a profile**
  - [PR #307](https://github.com/w3c/lws-protocol/pull/307): `application/lws+json` → `application/ld+json; profile="https://www.w3.org/ns/lws/v1"`
  - Any RDF syntax (Turtle, TriG, N-Triples…) if RDF-isomorphic to the profile
- **IETF building blocks**: OAuth 2.0 (RFC 6749), Protected Resource Metadata (RFC 9728), Web Linking (RFC 8288), Linksets (RFC 9264), Problem Details (RFC 9457)
- Vocabularies reused: ODRL, Dublin Core, schema.org, `ldp:inbox`
- Trend: removing the WG's own media types ([#267](https://github.com/w3c/lws-protocol/issues/267), [#276](https://github.com/w3c/lws-protocol/issues/276))

---

<!-- _class: dense -->

# Relationship to LDP

- LWS CRUD is semantically LDP CRUD ([#309](https://github.com/w3c/lws-protocol/issues/309), prompted by TAG charter review)
  - POST-to-container, server-minted URIs, atomic containment
  - GET/HEAD with ETag + Link; server-managed containment; opaque URIs
- **Our container subtype**
  - Proposal: `lws:Container` ⊑ `ldp:BasicContainer`, `lws:items` → `ldp:contains` in the JSON-LD context
  - Zero cost to JSON developers; LDP/Solid/Fedora tools can read LWS containers
  - `lws:DataResource` ⊑ `ldp:Resource` (not `ldp:NonRDFSource` — it may be RDF)
- **Other gaps** @@feedback welcome@@ (from #309 discussion, still being enumerated)
  - LDP requires `text/turtle` — LWS is JSON-first (Turtle via profile)
  - `ldp:constrainedBy` link on errors — compatible, could be added (raised on #309, 16 Nov)
  - Paging: LWS uses `first`/`next`/`prev`/`last` link rels (vs. LDP Paging Note)
  - `describedby` / linkset metadata vs. LDP's optional `describedby`
- **Problem details**
  - Errors may use RFC 9457 `application/problem+json` (400 SHOULD; 401/403/404 MAY)
  - Open question: does `ldp:constrainedBy` complement or duplicate problem+json?

---

# Obligatory "Get involved" slide

- Repo & issues: <https://github.com/w3c/lws-protocol>
- Implementations: [Implementations.md](https://github.com/w3c/lws-protocol/blob/main/Implementations.md) — signal interest via PR
- Test suite: <https://github.com/lws-contrib/lws-test-suite>
- Hot right now: [#309](https://github.com/w3c/lws-protocol/issues/309) LDP alignment · [#303](https://github.com/w3c/lws-protocol/issues/303) ODRL access profile · [#291](https://github.com/w3c/lws-protocol/issues/291) version discovery · [#308](https://github.com/w3c/lws-protocol/issues/308) OIDC `aud`
