# HACP — Human–AI Contribution Provenance

> Status: **v0.3 Working Draft**
>
> Direction: **C2PA / CAWG interoperability experiment**

HACP started as an experimental standalone model for describing how humans and AI systems contribute to digital content.

That exploration was useful, but the project direction has changed.

After mapping HACP v0.2 against C2PA and CAWG, most of the underlying provenance infrastructure is already available there: Actions, AI Disclosure, Regions of Interest, Ingredients, identity assertions, signatures, trust and process-evidence references.

HACP v0.3 therefore **does not attempt to define a parallel provenance standard**.

Instead, this repository is now an experimental workspace for a much narrower question:

> How can C2PA/CAWG describe the causal structure of multi-actor creation workflows — especially human/AI edit loops — in an interoperable way?

The strongest candidates currently being explored are:

- stable identifiers for individual action executions,
- causal dependencies between action instances,
- explicit representation of iterative edit/review loops,
- shared semantics for intellectual contributions such as `concept`, `fact-checking` and `approval`,
- optional qualitative contribution influence.

The goal is to prototype these ideas using existing C2PA extension mechanisms, validate them against real workflows, and contribute useful missing pieces upstream where appropriate.

---

## Why this changed

The original v0.2 draft modeled systems, actors, workflow steps, contributions, influence, trust boundaries and future attestations.

A detailed comparison showed that much of that functionality should not be reinvented.

Existing C2PA / CAWG mechanisms already substantially cover:

- cryptographic content binding,
- signatures and trust,
- AI model disclosure,
- human oversight disclosure,
- software agents,
- human and organizational identity,
- contributor/editor/publisher roles,
- partial-content attribution,
- input/ingredient relationships,
- custom actions and parameters,
- external audit logs and version histories.

The remaining problem is much smaller — and more interesting.

See:

- [`C2PA-GAP-ANALYSIS.md`](./C2PA-GAP-ANALYSIS.md)
- [`C2PA-MAPPING.md`](./C2PA-MAPPING.md)
- [`MIGRATION.md`](./MIGRATION.md)

---

## The edit-loop problem

Consider a common AI-assisted writing workflow:

```text
Human concept
      ↓
AI draft #1
      ↓
Human review #1
      ↓
AI rewrite #1
      ↓
AI wording pass
      ↓
Human fact-check
      ↓
Human final approval
```

Knowing only that these actions occurred is not enough.

For provenance and governance, the order matters.

For example:

> Did the final human approval happen before or after the last AI modification?

Now consider a branched workflow:

```text
                     ┌── AI research ───────┐
Human concept ───────┤                       ├── AI draft
                     └── Human research ────┘
```

Here simple chronological ordering is not enough either.

The draft depends on two earlier branches.

HACP v0.3 explores whether small, interoperable workflow semantics can describe these relationships without replacing C2PA.

---

## Experimental C2PA encoding

C2PA Actions allow entity-specific custom parameters.

The current experiment uses namespaced parameters such as:

```json
{
  "action": "c2pa.edited",
  "parameters": {
    "com.bajonczak.hacp.actionId": "ai-rewrite-1",
    "com.bajonczak.hacp.dependsOn": [
      "human-review-1"
    ],
    "com.bajonczak.hacp.contributions": [
      "drafting",
      "wording"
    ]
  }
}
```

These fields are **experimental**.

They are not part of the C2PA standard and should not be presented as such.

Their purpose is to test whether the semantics are useful enough to justify an upstream proposal.

---

## Current experimental fields

### `com.bajonczak.hacp.actionId`

Identifies one logical execution of an action inside the workflow experiment.

### `com.bajonczak.hacp.dependsOn`

Declares causal dependencies on other action instances.

### `com.bajonczak.hacp.contributions`

Experimental contribution vocabulary for semantics that may not map cleanly to technical asset transformations.

### `com.bajonczak.hacp.influence`

Optional experimental UX/governance metadata:

```text
minor
supporting
substantial
primary
```

This field is intentionally lower priority because influence is subjective.

It must not be interpreted as a percentage of authorship or ownership.

---

## What HACP no longer tries to define

v0.3 intentionally does **not** define its own:

- AI-provider model,
- human identity model,
- signer identity,
- trust list,
- signature format,
- content binding,
- Region of Interest format,
- ingredient/input graph,
- AI-generated flag,
- human-review boolean.

Where possible, implementations should use C2PA and CAWG for those concerns.

---

## Experiments

The [`experiments`](./experiments/) directory contains small test cases designed to answer concrete interoperability questions.

Current examples:

- [`edit-loop.actions.json`](./experiments/edit-loop.actions.json)
- [`branched-workflow.actions.json`](./experiments/branched-workflow.actions.json)

These are **C2PA Actions fragments for experimentation**, not complete signed C2PA manifests.

---

## Reference viewer

The reference viewer lives in a separate repository:

**HACP Viewer:**  
https://github.com/SBajonczak/hacp-viewer

The viewer currently targets the earlier HACP workflow model.

A next iteration should consume the experimental C2PA Action parameters and visualize causal dependencies and edit loops.

---

## Historical v0.2

HACP v0.2 remains useful as the design exploration that led to this narrower model.

The v0.2 release should be treated as historical and is preserved by the repository release/tag.

Main now tracks the v0.3 working direction.

---

## Questions for the community

The project is currently trying to answer a small number of concrete questions:

1. Is there an existing normative C2PA mechanism for ordering individual Action items that this project has missed?
2. Is `related` intentionally the wrong mechanism for causal dependencies?
3. Should action-instance identity and dependency semantics live directly in Actions?
4. Would a standardized creation-process evidence schema be a better fit?
5. Where should intellectual contribution vocabulary live: Actions, CAWG metadata, or another profile?
6. Is qualitative contribution influence useful enough to standardize at all?

---

## Upstream discussion

The workflow ordering and causal dependency experiment is currently being discussed with the C2PA community:

- [c2pa-org/specifications#127 — How should iterative and causally dependent Actions be represented in C2PA?](https://github.com/c2pa-org/specifications/issues/127)

## Project principle

HACP is not trying to compete with C2PA.

If an idea in this repository is already covered by C2PA or CAWG, it should be removed or mapped to the existing mechanism.

If an experiment identifies a useful missing semantic, the preferred outcome is to contribute that insight upstream.

---

## Repository structure

```text
.
├── README.md
├── SPECIFICATION.md
├── C2PA-MAPPING.md
├── C2PA-GAP-ANALYSIS.md
├── MIGRATION.md
├── ROADMAP.md
├── CONTRIBUTING.md
├── LICENSE
├── schema/
│   └── workflow-parameters.schema.json
└── experiments/
    ├── README.md
    ├── edit-loop.actions.json
    └── branched-workflow.actions.json
```

---

## Reference implementation status

- v0.2 standalone workflow model: released
- C2PA / CAWG gap analysis: available
- v0.3 C2PA extension experiment: in progress
- C2PA-native viewer: planned

Feedback and criticism are welcome.
