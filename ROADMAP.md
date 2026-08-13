# Roadmap

HACP is an experimental interoperability project.

The roadmap changed after mapping the v0.2 model against C2PA and CAWG.

## v0.2 — Standalone workflow exploration

Status: **Released / historical**

The v0.2 draft explored multiple AI systems, workflow steps, `dependsOn`, contribution vocabulary, qualitative influence, human review and future attestations.

The release remains available as design history.

The project does not currently plan to continue the standalone provenance format.

## v0.3 — C2PA workflow experiment

Status: **Current working draft**

- [x] complete C2PA / CAWG gap analysis
- [x] pivot away from duplicate provenance infrastructure
- [x] define experimental Action instance ID
- [x] define experimental causal `dependsOn`
- [x] retain contribution vocabulary only as an experiment
- [x] demote `influence` to optional/low-priority metadata
- [x] define edit-loop test case
- [x] define branched workflow test case
- [ ] build semantic validator for experimental parameters
- [ ] update reference viewer to consume C2PA Action fragments
- [ ] test AI Disclosure linkage
- [ ] test CAWG actor linkage
- [ ] test C2PA Regions of Interest with text edits
- [ ] create a complete signed C2PA proof of concept
- [ ] compare Action-native graph vs external process-evidence graph
- [ ] ask C2PA community to validate the identified gaps

## First upstream candidate

The first standards discussion should stay intentionally small.

Candidate primitives:

1. stable identity for an individual Action execution,
2. causal dependency between Action executions.

No PR should be prepared until the intended abstraction level is confirmed by the C2PA community.

## Secondary research

- [ ] evaluate contribution vocabulary
- [ ] evaluate provider attestation patterns
- [ ] evaluate detailed human review semantics
- [ ] evaluate privacy implications
- [ ] evaluate cross-manifest workflow references

## Low-priority / controversial research

- [ ] qualitative contribution influence
- [ ] governance interpretation of influence
- [ ] UX representation of contribution importance

## Reference tooling

- [ ] workflow parameter validator
- [ ] graph visualizer for C2PA Actions
- [ ] edit-loop visualization
- [ ] C2PA/CAWG interoperability test vectors
- [ ] CLI inspection tool
- [ ] signed proof-of-concept asset

## Success criteria

### Outcome A

The experiments reveal that C2PA/CAWG already provide a clean solution.

Result: remove the redundant HACP concept and document the correct mapping.

### Outcome B

The experiments identify a small useful interoperability gap.

Result: propose that gap upstream and help implement it.

### Outcome C

The workflow belongs in external process evidence rather than C2PA Actions.

Result: prototype an interoperable evidence schema and discuss it with the community.

A standalone HACP standard is no longer the primary success criterion.
