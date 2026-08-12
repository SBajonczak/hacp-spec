# Human–AI Contribution Provenance Specification

Version: **0.2**  
Status: **Experimental Draft**

---

## 1. Introduction

Human–AI Contribution Provenance (HACP) defines a machine-readable vocabulary for describing how humans and AI systems contributed to digital content.

HACP describes declared contribution history rather than attempting to infer provenance from the finished content.

The central concept is a **workflow** composed of ordered or dependent contribution steps.

---

## 2. Design principles

HACP SHOULD:

- represent human and AI participation without treating AI usage as inherently positive or negative,
- avoid unsupported numerical claims about authorship,
- support multiple AI providers and models,
- support human review and approval as explicit workflow steps,
- support both linear and branching workflows,
- remain independent from any single AI vendor,
- avoid defining a new cryptographic trust system where established provenance standards can be used.

---

## 3. Top-level document

A HACP document MAY contain:

- `specVersion`
- `summary`
- `systems`
- `workflow`
- `issuer`
- `extensions`

Example:

```json
{
  "specVersion": "0.2",
  "summary": {
    "aiInvolvement": "collaborative"
  },
  "systems": [],
  "workflow": [],
  "issuer": {
    "type": "author",
    "name": "Example Author"
  }
}
```

---

## 4. `specVersion`

Required.

The version of the HACP specification used by the document.

For this draft:

```json
{
  "specVersion": "0.2"
}
```

---

## 5. `summary`

Optional.

`summary` provides a simplified representation for user interfaces and consumers that do not require the detailed workflow.

Example:

```json
{
  "summary": {
    "aiInvolvement": "collaborative"
  }
}
```

Allowed values for `aiInvolvement`:

### `none`

No generative AI contribution is declared.

### `assisted`

AI primarily assisted a human-led process.

Examples:

- grammar correction,
- wording improvements,
- formatting,
- translation,
- restructuring,
- limited research assistance.

### `collaborative`

Human and AI contributions were both substantial to the resulting content.

### `generated`

AI performed the primary generation of the resulting content.

The summary is intentionally coarse.

Consumers SHOULD prefer the detailed `workflow` when available.

---

## 6. `systems`

Optional.

`systems` describes AI systems referenced by workflow steps.

Example:

```json
{
  "systems": [
    {
      "id": "sys-openai-1",
      "provider": {
        "name": "OpenAI"
      },
      "model": {
        "name": "Example Model"
      },
      "product": "Example Product"
    }
  ]
}
```

### 6.1 `id`

Required for each system.

The value MUST be unique inside the HACP document.

### 6.2 `provider`

Required.

Describes the organization providing the AI system.

Example:

```json
{
  "provider": {
    "name": "Example AI Provider",
    "identifier": "https://example.com"
  }
}
```

`identifier` is optional and SHOULD contain a stable provider identifier when known.

### 6.3 `model`

Optional.

Example:

```json
{
  "model": {
    "name": "Example Model",
    "identifier": "provider:model-id"
  }
}
```

### 6.4 `product`

Optional.

May describe the user-facing product through which the model was used.

Examples include:

- a chat product,
- an API,
- an IDE integration,
- a CMS assistant.

The product name MUST NOT be used as a substitute for model identity when the model is known.

---

## 7. `workflow`

Required.

`workflow` describes how the content was created or transformed.

Each entry represents one contribution step.

Example:

```json
{
  "workflow": [
    {
      "id": "step-1",
      "sequence": 1,
      "actor": {
        "type": "human",
        "role": "author"
      },
      "contributions": [
        "concept",
        "expertise"
      ],
      "influence": "primary"
    }
  ]
}
```

---

## 8. Workflow step

A workflow step contains:

- `id`
- `sequence`
- `dependsOn`
- `actor`
- `contributions`
- `influence`
- `description`
- `extensions`

### 8.1 `id`

Required.

A workflow-local unique identifier.

Example:

```json
{
  "id": "step-3"
}
```

### 8.2 `sequence`

Required.

Positive integer representing the preferred display order.

`sequence` is not sufficient to express all causal relationships.

Consumers SHOULD use `dependsOn` when reconstructing provenance relationships.

### 8.3 `dependsOn`

Optional.

Array containing workflow step IDs on which this step depends.

Example:

```json
{
  "dependsOn": [
    "step-1",
    "step-2"
  ]
}
```

A referenced step MUST exist in the same HACP document.

Implementations SHOULD reject circular dependency graphs.

### 8.4 `actor`

Required.

Describes the participant responsible for the contribution step.

Human actor example:

```json
{
  "actor": {
    "type": "human",
    "id": "author-1",
    "role": "author"
  }
}
```

AI actor example:

```json
{
  "actor": {
    "type": "ai",
    "systemId": "sys-openai-1"
  }
}
```

Allowed `type` values in v0.2:

- `human`
- `ai`

Future versions MAY introduce additional actor types.

For `type: ai`, `systemId` MUST reference an entry in `systems`.

### 8.5 `contributions`

Required.

An array describing what the actor contributed during the step.

Initial contribution vocabulary:

- `concept`
- `research`
- `research-assistance`
- `expertise`
- `facts`
- `fact-checking`
- `argument`
- `creative-direction`
- `brainstorming`
- `structure`
- `drafting`
- `wording`
- `translation`
- `summarization`
- `editing`
- `review`
- `approval`
- `generation`

The same contribution vocabulary is used for human and AI actors.

This is intentional.

For example, both a human and an AI system may perform:

- translation,
- drafting,
- editing,
- research.

The actor type identifies who performed the contribution.

### 8.6 `influence`

Required.

Qualitative description of the actor's influence on the respective workflow step.

Allowed values:

#### `minor`

A small contribution that did not materially determine the result.

#### `supporting`

A contribution that supported an already established direction or content base.

#### `substantial`

A contribution that materially shaped the result.

#### `primary`

The determining contribution for the respective workflow stage.

`influence` refers to influence on the respective workflow step.

It MUST NOT be interpreted as a mathematically measured percentage of total authorship or intellectual ownership.

Implementations SHOULD NOT convert these values into purportedly exact authorship percentages without clearly describing the methodology used.

### 8.7 `description`

Optional.

Short human-readable description of the step.

Example:

```json
{
  "description": "AI system restructured the human-provided technical notes into an article outline."
}
```

Descriptions SHOULD NOT expose confidential prompts or private source material.

---

## 9. Human review and approval

HACP v0.2 does not define a separate `humanReviewed` boolean.

Human review SHOULD be represented as a workflow step.

Example:

```json
{
  "id": "step-5",
  "sequence": 5,
  "actor": {
    "type": "human",
    "role": "editor"
  },
  "contributions": [
    "fact-checking",
    "review",
    "approval"
  ],
  "influence": "primary"
}
```

This representation is more expressive than a boolean because it records:

- who reviewed,
- when review happened,
- what kind of review occurred,
- and what influence the review had.

---

## 10. Multiple providers

A HACP workflow MAY reference any number of AI systems.

Example workflow:

```text
Human concept
    ↓
Provider A research assistance
    ↓
Provider B drafting
    ↓
Provider C translation
    ↓
Human review
```

Each AI workflow step SHOULD reference the corresponding entry in `systems`.

---

## 11. Branched workflows

HACP supports non-linear provenance through `dependsOn`.

Example:

```text
               ┌─ AI research ─────┐
Human concept ─┤                    ├─ Draft
               └─ Human research ──┘
```

The draft step may declare:

```json
{
  "dependsOn": [
    "step-ai-research",
    "step-human-research"
  ]
}
```

This creates a directed provenance graph.

Workflow dependency graphs MUST NOT contain cycles.

---

## 12. `issuer`

Optional.

Describes the party issuing the HACP declaration.

Example:

```json
{
  "issuer": {
    "type": "publisher",
    "name": "Example Publishing"
  }
}
```

Suggested issuer types:

- `author`
- `publisher`
- `platform`
- `editor`
- `tool`
- `ai-provider`

The issuer is the party making the declaration.

The issuer is not necessarily the actor responsible for every workflow step.

---

## 13. Trust and content binding

HACP defines contribution semantics and does not define its own cryptographic trust infrastructure.

Implementations requiring:

- content binding,
- digital signatures,
- signer identity,
- certificate chains,
- revocation,
- trusted timestamps,
- trust lists,

SHOULD use an established provenance or cryptographic framework.

A standalone HACP document without an external trust mechanism MUST be treated as a self-declared provenance statement.

HACP is designed so that workflow information can later be bound to trusted provenance records or attestations.

---

## 14. Step attestations

Step-level attestations are **not normative in v0.2**.

A future version may allow independent parties to attest to individual workflow steps.

Example concept:

```text
Human
  │ concept
  ▼
AI Provider
  │ drafting
  │ provider attestation
  ▼
Human Editor
  │ approval
  │ publisher attestation
  ▼
Published
```

A provider attestation SHOULD only assert facts the provider can reasonably verify.

For example, an AI provider may be able to attest that:

- a specific system processed a request,
- a specific model family was used,
- a particular generation event occurred.

The provider SHOULD NOT be expected to attest that a human-origin claim is true unless the provider independently verified that claim.

---

## 15. Extensions

Optional.

Implementations MAY include experimental metadata under `extensions`.

Example:

```json
{
  "extensions": {
    "example.org/custom-field": {
      "value": "example"
    }
  }
}
```

Extension keys SHOULD use a namespace controlled by the defining entity.

Consumers MUST ignore unknown extensions they do not understand.

---

## 16. Privacy

HACP documents SHOULD disclose the minimum amount of information necessary to express provenance.

Implementations SHOULD NOT expose:

- private prompts,
- hidden system instructions,
- personal conversations,
- confidential source material,
- unnecessary personal identifiers,
- API secrets,
- private request payloads.

Stable identifiers SHOULD be preferred over unnecessary personal data.

---

## 17. Interpretation

HACP describes declared provenance.

HACP does not determine:

- truthfulness,
- quality,
- copyright ownership,
- plagiarism,
- legal authorship,
- search ranking,
- academic acceptability.

Those decisions belong to consuming systems and applicable policies.

---

## 18. Open questions for v0.3+

Open questions include:

- step-level provider attestations,
- standardized identity references,
- partial-document provenance,
- inheritance during translation,
- inheritance during summarization,
- provenance after manual editing,
- provenance across document fragments,
- mapping to C2PA assertions,
- aggregation of provider participation,
- policy evaluation,
- privacy-preserving analytics,
- robust fallback signals when metadata is removed.
