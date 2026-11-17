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

# Goals

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

# Technology stack

- HTTP resources in a containment hierarchy
- Data and service discovery
- Authentication: integration with OpenID Connect, SAML, self-asserted identity
- Authorization: OAuth 2.0 profile


---


# Relationship to Solid

- Solid is incubation — needs standardization
  - [Solid Protocol 0.11](https://solidproject.org/TR/2024/protocol-20240512) (CG report) is a charter *input*, not the output
  - Normative deps not Rec-track ready (WebID, Solid-OIDC, WAC/ACP, Solid Notifications)
- Placing the necessary components on Rec-track foundations
  - Identity and authentication → CID + pluggable authn suites
  - Authorization and sharing → [ODRL Access Profile](https://www.w3.org/TR/lws10-core/#access-profile)
  - Developer-friendly interoperability via JSON-LD based data model
- Community Solid Server is also an LWS implementation

---


# Relationship to LDP

- LWS operations are semantically the same as LDP CRUD
  - Proposal: identify a required subset of LDP while keeping developer friendly focus
  - Content negotiation required for containers
  - Include paging support for containers

---


# Out of scope

  - From charter: "a unique and exclusive solution for Personal Data Stores"
  - New identity mechanisms
  - New data formats

---

<!-- _class: dense -->

# Data structures

- **CID** (Controlled Identifiers 1.0)
  - [Storage description](https://www.w3.org/TR/lws10-core/#storage-description-data-model) = CID document + LWS vocabulary
  - Verification mechanism for authentication suites
- **Activity Streams 2.0**: [notification envelope](https://www.w3.org/TR/lws10-core/#notification-data-model) wraps an AS2 `Create` / `Update` / `Delete` / `Move`
- **JSON-LD with a [profile](https://www.w3.org/TR/lws10-core/#lws-profile)**
  - Container data model
- **IETF building blocks**: OAuth 2.0 (RFC 6749), Protected Resource Metadata (RFC 9728), Web Linking (RFC 8288), Linksets (RFC 9264), Problem Details (RFC 9457)
- **Vocabularies**: ODRL, Dublin Core, LDP, schema.org

---

<!-- _class: dense -->

# Charter vote

- AC review runs through Dec 9
- Draft charter: https://w3c.github.io/lws-wg-charter/
- Charter vote: https://www.w3.org/wbs/33280/lws-charter-2026/

