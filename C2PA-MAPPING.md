# HACP v0.3 and C2PA / CAWG

Status: **Working interoperability map**

## Direction

HACP v0.3 is no longer designed as a parallel provenance layer.

The project now starts from the assumption that C2PA and CAWG should provide the foundation.

HACP only experiments with workflow semantics that may not currently be interoperable.

## Reuse first

| HACP v0.2 concept | Preferred mechanism in v0.3 |
|---|---|
| `systems` | C2PA `softwareAgent` / AI Disclosure |
| AI provider/model | AI Disclosure + existing generator/software metadata |
| human actor | CAWG Identity Assertion |
| issuer | C2PA signer / CAWG identity where applicable |
| human review summary | AI Disclosure human oversight |
| content binding | C2PA |
| signatures/trust | C2PA |
| content regions | C2PA Regions of Interest |
| input assets | C2PA Ingredients |
| translation | standard C2PA translation Action |
| generic edit | standard C2PA edit Actions |
| process log | C2PA external process evidence |

## Experimental additions

The current experiment tests only:

```text
action instance identity
causal dependencies
contribution semantics
optional influence
```

using namespaced custom Action parameters.

## Why ordering is being tested

A set of Actions can say what happened.

Collaborative provenance may also need to answer:

```text
What happened after what?
What depended on which review?
Was there an AI modification after human approval?
Which parallel research branches fed the final draft?
```

The experiment intentionally separates chronology from causal dependency.

## `related` is not assumed to mean `dependsOn`

HACP v0.3 does not reinterpret C2PA `related`.

The project treats it as an existing C2PA concept with its own semantics.

Any causal dependency proposal should use a separate experimental field until the community confirms the correct model.

## Identity

HACP v0.3 intentionally removes its own human actor structure.

CAWG should be used for named/verifiable actors.

An interoperability experiment is still needed to determine the cleanest way to associate a CAWG actor with one specific logical action instance when an Actions assertion contains multiple Action items.

## AI model linkage

HACP v0.3 intentionally removes its own AI system registry.

A future experiment should test:

```text
Action
   │
   ├── softwareAgent
   │
   └── related assertion
          ↓
      AI Disclosure
```

## Process evidence

C2PA supports hashed external creation-process evidence such as audit logs and version histories.

This creates a legitimate alternative architecture to putting workflow-graph fields directly into Actions.

## Preferred upstream path

```text
existing C2PA/CAWG mapping
        ↓
entity-specific prototype
        ↓
real edit-loop test
        ↓
community review
        ↓
small gap proposal
        ↓
upstream PR only after direction is agreed
```

The goal is to contribute the smallest useful missing semantics rather than preserve HACP as a separate standard.
