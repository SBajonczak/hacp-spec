# HACP v0.2 → v0.3

## Why the project changed direction

HACP v0.2 was intentionally broad.

It modeled actors, AI systems, contribution types, workflow ordering, dependencies, influence, human review, issuer information and future attestations.

That model helped make the problem concrete.

After comparing it in detail with C2PA and CAWG, it became clear that much of the v0.2 structure duplicates capabilities that already exist in the content-authenticity ecosystem.

Continuing to build a parallel standard would make interoperability worse, not better.

Therefore v0.3 changes the project direction.

## v0.2

```text
Standalone HACP Manifest
    │
    ├── actors
    ├── systems
    ├── workflow
    ├── contributions
    ├── influence
    └── issuer
```

## v0.3

```text
C2PA / CAWG
    │
    ├── Actions
    ├── AI Disclosure
    ├── Identity
    ├── Regions
    ├── Ingredients
    └── Trust
          │
          └── HACP experiment
                ├── actionId
                ├── dependsOn
                ├── contributions
                └── influence (optional)
```

## Removed from the HACP core experiment

- `systems` → use C2PA software-agent and AI Disclosure mechanisms.
- `actor` → use CAWG identity where named/verifiable attribution is required.
- `issuer` → use surrounding C2PA/CAWG trust and identity.
- `summary.aiInvolvement` → prefer existing C2PA source and human-oversight semantics.
- custom trust/signature model → removed.
- content-region model → use C2PA Regions of Interest.
- input graph → use C2PA Ingredients.

## Retained as experimental semantics

- action instance identity,
- causal dependency,
- contribution vocabulary,
- optional influence research metadata.

## Compatibility

v0.3 is a conceptual breaking change.

A v0.2 HACP manifest is not a v0.3 document.

The released v0.2 tag remains the reference for the original standalone format.

New work on `main` should target the v0.3 interoperability experiment.

## Why this is progress

The goal of HACP is not to own a format.

The goal is to improve how human/AI contribution provenance can be represented.

If an existing standard already solves part of the problem, using that standard is the correct outcome.
