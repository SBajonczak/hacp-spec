# HACP v0.3 — C2PA Workflow Semantics Experiment

Version: **0.3 Working Draft**  
Status: **Experimental / non-normative**

---

## 1. Scope

HACP v0.3 is not a standalone content-provenance format.

It is an experiment for expressing a small set of creation-workflow semantics using existing C2PA extension mechanisms.

The experiment focuses on:

1. identifying individual action executions,
2. declaring causal dependencies between action executions,
3. describing selected human/AI contribution semantics,
4. testing iterative human/AI edit loops,
5. evaluating whether any of these semantics are appropriate for standardization upstream.

HACP v0.3 does not define cryptographic trust, identity, content binding, AI model disclosure or content-region selection.

Those concerns belong to C2PA and CAWG.

---

## 2. Design constraint

The experiment SHOULD reuse existing C2PA / CAWG mechanisms wherever they already represent the required information.

HACP-specific metadata SHOULD only be introduced where the experiment is testing a semantic that does not map cleanly to an existing interoperable mechanism.

---

## 3. Carrier

The primary experimental carrier is the open custom-parameter map of a C2PA v2 Action.

HACP v0.3 parameters use the entity-specific namespace:

```text
com.bajonczak.hacp
```

Example:

```json
{
  "action": "c2pa.edited",
  "parameters": {
    "com.bajonczak.hacp.actionId": "ai-edit-2",
    "com.bajonczak.hacp.dependsOn": [
      "human-review-1"
    ]
  }
}
```

These names are experimental and do not imply C2PA endorsement.

---

## 4. Action instance identity

### 4.1 `com.bajonczak.hacp.actionId`

A workflow-local identifier for one logical execution of an action.

Example:

```json
{
  "com.bajonczak.hacp.actionId": "ai-draft-1"
}
```

### Requirements

Within one experimental workflow:

- an `actionId` MUST be unique,
- an `actionId` SHOULD be stable for the lifetime of the workflow evidence,
- an `actionId` MUST NOT be interpreted as a globally unique content identifier,
- an `actionId` SHOULD NOT contain personal information.

The initial experiment uses readable string identifiers.

Future experiments may test UUIDs or URIs if cross-assertion or cross-manifest references require them.

---

## 5. Causal dependencies

### 5.1 `com.bajonczak.hacp.dependsOn`

An optional array of `actionId` values whose results or decisions were required inputs to the current action execution.

Example:

```json
{
  "com.bajonczak.hacp.actionId": "ai-draft-1",
  "com.bajonczak.hacp.dependsOn": [
    "ai-research-1",
    "human-research-1"
  ]
}
```

### Semantics

`dependsOn` expresses a causal or process dependency.

It does **not** merely mean:

- happened earlier,
- is visually adjacent,
- occurred at approximately the same time,
- is topically related.

### Graph requirements

The resulting dependency graph:

- MUST NOT contain cycles,
- MAY contain parallel branches,
- MAY contain multiple roots,
- MAY contain multiple leaves.

An iterative process such as repeated AI editing and human review SHOULD create a new action instance for every execution rather than creating a graph cycle.

---

## 6. Display order

HACP v0.3 does not currently define a separate `sequence` field.

A consumer may derive a valid display order from the dependency graph.

If multiple valid topological orders exist, the UI MAY use additional information such as:

- C2PA `when`,
- document order,
- implementation-specific process metadata.

Such display ordering MUST NOT be confused with causal dependency.

This is a deliberate change from HACP v0.2.

---

## 7. Edit loops

A common collaborative workflow is:

```text
AI draft #1
    ↓
Human review #1
    ↓
AI edit #2
    ↓
Human review #2
```

Although this is informally called a loop, the provenance representation remains acyclic because every execution is a distinct action instance.

This representation allows a consumer to answer questions such as:

- Was there a human review after the final AI edit?
- Which human review caused a later rewrite?
- Did another AI action happen after final approval?
- Which branches contributed to a later draft?

---

## 8. Contribution semantics

### 8.1 `com.bajonczak.hacp.contributions`

Optional array describing the semantic contribution made during an action.

Initial experimental vocabulary:

```text
concept
research
research-assistance
expertise
facts
fact-checking
argument
creative-direction
brainstorming
structure
drafting
wording
summarization
review
approval
```

### Existing C2PA action semantics should win

The contribution vocabulary MUST NOT be used to replace a suitable standard C2PA Action.

For example:

- translation SHOULD use the C2PA translation Action,
- editing SHOULD use an appropriate C2PA edit Action,
- content creation SHOULD use the appropriate C2PA creation Action and source type.

A contribution term may provide additional semantic context where useful.

### Open question

Some contribution concepts describe intellectual input rather than an asset transformation.

Examples:

```text
concept
expertise
creative-direction
approval
```

It remains an open question whether these should ultimately be represented as:

- C2PA Actions,
- CAWG metadata,
- a dedicated profile/assertion,
- standardized process evidence,
- or not standardized at all.

---

## 9. Influence

### 9.1 `com.bajonczak.hacp.influence`

Optional, low-priority experimental metadata.

Allowed values:

```text
minor
supporting
substantial
primary
```

The value represents a declared qualitative assessment of the contribution's significance in the local workflow context.

It MUST NOT be interpreted as:

- percentage of authorship,
- percentage of copyright ownership,
- mathematical contribution weight,
- objective truth.

Because influence is subjective, it is not currently considered a primary candidate for upstream standardization.

Implementations MAY omit it entirely.

---

## 10. Actors and identity

HACP v0.3 does not define an `actor` object.

Human and organizational identity SHOULD use CAWG identity mechanisms where verifiable attribution is required.

A future test should evaluate the recommended pattern for associating a CAWG identity with one specific logical action execution.

---

## 11. AI systems and models

HACP v0.3 does not define `systems`, `provider` or `model` objects.

Implementations SHOULD reuse C2PA mechanisms such as:

- `softwareAgent`,
- `softwareAgents`,
- AI Disclosure,
- `digitalSourceType`,
- related assertions.

A future interoperability test should evaluate how cleanly an Action can be linked to the AI Disclosure that describes the model responsible for that Action.

---

## 12. Human review and approval

HACP v0.3 does not define a `humanReviewed` boolean.

Where applicable, C2PA AI Disclosure should represent coarse human oversight.

More detailed activities may be tested as contribution semantics or entity-specific actions:

```text
fact-checking
review
approval
```

The edit-loop experiment is specifically interested in whether final human approval happened **after the last AI transformation**.

---

## 13. Regions and partial content

HACP v0.3 does not define its own partial-content selector.

Implementations SHOULD reuse C2PA Regions of Interest and the Action `changes` mechanism.

---

## 14. Inputs and ingredients

HACP v0.3 does not define its own input graph.

C2PA Ingredients SHOULD be used for asset/data inputs and existing ingredient relationships.

`dependsOn` is intended only for the experimental relationship between **action executions**.

---

## 15. Trust and signatures

HACP v0.3 defines no cryptographic primitives.

The experiment does not define:

- signatures,
- certificates,
- trust lists,
- revocation,
- timestamps,
- content hashes,
- identity credentials.

These must be handled by established provenance and identity mechanisms.

---

## 16. Process evidence alternative

The experiment does not assume that workflow dependencies must eventually become fields in C2PA Actions.

C2PA can bind external creation-process evidence such as audit logs and version histories.

Therefore two architectures should be compared:

### Option A — Action-native workflow semantics

```text
C2PA Action
  ├── action instance ID
  └── dependencies
```

### Option B — Standardized process-evidence schema

```text
C2PA Manifest
    ↓
hashed external process evidence
    ↓
workflow graph
```

The experiments should determine which approach produces better interoperability, privacy, implementation simplicity, scalability and user experience.

---

## 17. Validation rules for the experiment

A validator for HACP v0.3 experimental parameters SHOULD check:

1. every `actionId` is unique,
2. every referenced `dependsOn` target exists,
3. an action does not depend on itself,
4. the dependency graph is acyclic,
5. contribution values are known experimental values when strict validation is enabled,
6. influence values are valid when present.

These checks are semantic validation for the experiment.

They are not C2PA conformance rules.

---

## 18. Compatibility

Consumers that do not understand HACP experimental parameters should still be able to process the surrounding C2PA data.

The experiment MUST NOT require reinterpretation of standard C2PA fields.

---

## 19. Candidate upstream contribution

No C2PA change is proposed by this document yet.

The first upstream discussion should focus on whether the following primitives are useful and missing:

1. stable identity for individual Action executions,
2. causal Action-to-Action dependency.

Only after community agreement should a concrete C2PA specification change be drafted.

Contribution vocabulary and influence should remain secondary discussions.

---

## 20. Non-goals

HACP v0.3 does not attempt to:

- create a competing provenance ecosystem,
- replace C2PA,
- replace CAWG,
- determine whether content is true,
- detect AI-generated content,
- measure authorship percentages,
- define copyright ownership,
- define search ranking,
- establish a private trust authority.

---

## 21. Experimental status

Everything in this document is subject to change.

The `com.bajonczak.hacp.*` namespace is used only to make interoperability experiments concrete.

It should not be treated as a stable public standard namespace until the experiment has been reviewed and its final home is known.
