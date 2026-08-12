# Roadmap

This roadmap is intentionally short and experimental.

---

## v0.2 — Workflow model

Status: **Current draft**

Goals:

- [x] support multiple AI systems
- [x] introduce workflow steps
- [x] introduce `dependsOn`
- [x] define qualitative influence levels
- [x] unify human and AI contribution vocabulary
- [x] represent human review as workflow activity
- [x] separate contribution semantics from cryptographic trust
- [x] document intended C2PA relationship

---

## v0.3 — Validation and interoperability

Potential goals:

- [ ] reference validator
- [ ] semantic validation beyond JSON Schema
- [ ] reject dependency cycles
- [ ] validate `systemId` references
- [ ] define stable extension rules
- [ ] refine contribution vocabulary
- [ ] add partial-document examples
- [ ] add translation and summarization inheritance examples
- [ ] define a draft C2PA assertion mapping
- [ ] create interoperability test vectors

---

## v0.4 — Attestations

Potential goals:

- [ ] define abstract step attestation model
- [ ] distinguish self-declared and externally attested steps
- [ ] support AI-provider attestations
- [ ] support publisher attestations
- [ ] support tool/platform attestations
- [ ] define attestation status and verification result semantics

The specification SHOULD avoid inventing custom cryptographic primitives.

---

## Reference tooling

Potential open-source tools:

- [ ] JSON Schema validator
- [ ] workflow graph visualizer
- [ ] command-line validator
- [ ] JavaScript / TypeScript SDK
- [ ] .NET SDK
- [ ] reference C2PA mapping demo

---

## Commercial ecosystem opportunities

These are not part of the open specification.

Potential services include:

- verified issuer identity,
- signing and key-management APIs,
- provider attestation gateways,
- audit logs,
- enterprise governance policies,
- CMS integrations,
- compliance reporting,
- provider participation analytics,
- high-volume verification APIs,
- private enterprise deployment,
- SLA-backed verification infrastructure.

The open specification and commercial infrastructure SHOULD remain separable.

---

## Open design questions

Important unresolved topics:

1. How should influence be aggregated without creating fake precision?
2. How should manual editing affect inherited provenance?
3. How should partial-document provenance be represented?
4. Can providers attest to generation events without disclosing prompts?
5. How should privacy-preserving provider analytics work?
6. How should provenance survive exports and format conversions?
7. How should trustworthy publisher identity be represented?
8. How should HACP interoperate with C2PA without duplicating it?
