# HACP and C2PA

Status: **Experimental mapping note for HACP v0.2**

---

## 1. Purpose

HACP is designed to complement existing provenance standards rather than replace them.

The responsibilities are intentionally separated.

### HACP focuses on contribution semantics

HACP describes:

- which humans or AI systems participated,
- what each actor contributed,
- the order of contributions,
- dependencies between workflow stages,
- qualitative influence of each actor.

### Provenance frameworks focus on trust infrastructure

An established provenance framework such as C2PA can provide mechanisms for:

- binding provenance to content,
- digital signatures,
- signer identity,
- trust chains,
- validation,
- provenance history.

HACP SHOULD reuse such mechanisms instead of defining competing cryptographic infrastructure.

---

## 2. Conceptual relationship

```text
HACP
  │
  │ describes
  ▼
Human / AI contribution workflow
  │
  │ carried by or referenced from
  ▼
Provenance framework
  │
  │ binds and signs
  ▼
Digital content
```

HACP therefore acts as a semantic layer.

---

## 3. Why HACP is still useful

Existing provenance systems can describe that AI was used or that a human validated content.

HACP aims to describe a finer-grained workflow.

Example:

```text
Step 1  Human     concept + expertise          primary
Step 2  AI A      research assistance          supporting
Step 3  AI B      structure + drafting         primary
Step 4  AI C      translation                  substantial
Step 5  Human     fact-check + approval         primary
```

This information is useful for:

- transparency,
- enterprise governance,
- publishing workflows,
- provider analytics,
- compliance rules,
- content labeling.

---

## 4. Future assertion mapping

A future HACP version may define a dedicated assertion payload that can be embedded into or referenced by a C2PA manifest.

Conceptual example:

```json
{
  "specVersion": "0.2",
  "systems": [
    {
      "id": "sys-ai-1",
      "provider": {
        "name": "Example AI Provider"
      }
    }
  ],
  "workflow": [
    {
      "id": "step-1",
      "sequence": 1,
      "actor": {
        "type": "ai",
        "systemId": "sys-ai-1"
      },
      "contributions": [
        "drafting"
      ],
      "influence": "primary"
    }
  ]
}
```

The exact assertion label and namespace are intentionally not fixed in v0.2.

A stable name SHOULD only be defined after:

- the project name is finalized,
- a stable namespace is available,
- interoperability requirements have been tested.

---

## 5. Mapping principle

HACP SHOULD NOT duplicate information already represented authoritatively by the surrounding provenance framework.

For example, HACP does not need to invent its own:

- certificate format,
- signature algorithm profile,
- revocation model,
- timestamp format,
- trust store.

Instead, HACP should contribute the information that is unique to its purpose:

> Who contributed what, when, and with what qualitative influence?

---

## 6. Step attestations

A future integration may allow individual workflow steps to carry attestations from the systems that actually performed them.

Concept:

```text
Human concept
    │
    ▼
AI research
    │  ✓ provider attestation
    ▼
AI draft
    │  ✓ provider attestation
    ▼
Human review
    │  ✓ publisher attestation
    ▼
Published asset
```

This would make provenance stronger than a single self-declared manifest.

---

## 7. Non-goal

HACP does not attempt to become a replacement for C2PA.

The preferred direction is interoperability.
