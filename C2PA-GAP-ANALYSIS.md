# C2PA / CAWG Gap Analysis for HACP

Status: **Working analysis**  
HACP baseline: **v0.2 Experimental Draft**  
C2PA baseline: **Content Credentials Technical Specification 2.4**  
CAWG baseline: **Identity Assertion 1.3 draft and Metadata Assertion 1.2 draft**

---

## 1. Purpose

HACP started as an experimental machine-readable model for describing how humans and AI systems contributed to digital content.

After feedback from the C2PA community, the right question is no longer:

> How can HACP become a parallel provenance standard?

The more useful question is:

> Which HACP concepts are already expressible with C2PA/CAWG, and which semantics are not cleanly interoperable today?

This document maps HACP v0.2 against existing C2PA and CAWG mechanisms and identifies the smallest remaining gaps worth discussing with the community.

The desired outcome is **not** to duplicate functionality already present in C2PA.

If HACP contains semantics that are genuinely useful and missing, those semantics should preferably be contributed to the existing provenance ecosystem.

---

## 2. Current conclusion

Most infrastructure originally described by HACP is already available in C2PA and CAWG.

In particular, existing specifications already cover or substantially cover:

- cryptographic content binding,
- signatures and manifest trust,
- AI model disclosure,
- human oversight disclosure,
- software agents,
- multiple software/AI tools,
- standard and entity-specific actions,
- translation,
- content-region attribution,
- ingredients and input relationships,
- human and organizational identity,
- creator/editor/publisher/contributor roles,
- externally stored audit logs and version history,
- custom assertions and custom action parameters.

The strongest remaining HACP ideas appear to be:

1. **interoperable ordering and causal dependencies between action instances,**
2. **explicit modeling of iterative/edit-loop workflows,**
3. **semantic contribution vocabulary beyond technical asset transformations,**
4. **possibly per-step contribution significance/influence,**
5. **a consistent pattern for binding a human or provider attestation to a specific workflow contribution.**

Items 4 and 5 are less clearly gaps than items 1–3 and require community feedback.

---

## 3. High-level mapping

| HACP concept | C2PA / CAWG mechanism | Assessment | Remaining question |
|---|---|---|---|
| `summary.aiInvolvement` | `digitalSourceType` + `c2pa.ai-disclosure.contentProfile.humanOversightLevel` | Largely covered | HACP summary may be derived for UX rather than standardized |
| AI model name | `c2pa.ai-disclosure.modelName` | Covered | None |
| AI model identifier | `c2pa.ai-disclosure.modelIdentifier` | Covered | None |
| AI provider | Model identifier + `softwareAgent` + custom generator fields | Partially covered | Provider organization is not a clear first-class AI Disclosure field |
| User-facing AI product/tool | `softwareAgent` / generator info | Covered | None |
| Multiple AI systems | `softwareAgents`, multiple actions, AI Disclosure assertions | Covered | Need clear linkage between action and model disclosure |
| AI action → model disclosure | `parameters.relatedAssertions` can reference non-action assertions | Covered pattern | Worth testing as reference implementation |
| Human identity | CAWG Identity Assertion | Covered | None |
| Human role | CAWG roles (`creator`, `contributor`, `editor`, `publisher`, `translator`, etc.) | Covered | Contribution-specific roles may still need vocabulary |
| Human actor attests to Actions assertion | CAWG Identity Assertion may reference `c2pa.actions` | Covered | Granularity is assertion-level, not an individual action item |
| Issuer / publisher identity | C2PA signer + CAWG Identity Assertion | Covered | None |
| Content binding | C2PA hard/soft binding | Covered | HACP should not duplicate |
| Cryptographic signatures | C2PA / CAWG | Covered | HACP should not duplicate |
| Partial content attribution | C2PA Regions of Interest | Covered | Includes textual regions |
| Translation | `c2pa.translated` + source/target language parameters | Covered | None |
| Editing | `c2pa.edited` and more specific actions | Covered | Semantic intent may need extra metadata |
| AI generation | `c2pa.created` + trained algorithmic `digitalSourceType` + AI Disclosure | Covered | None |
| Inputs to AI process | Ingredient `inputTo` / action ingredients | Covered | None |
| Generic content contributions | CAWG Metadata Assertion + custom metadata | Technically possible | No standardized contribution workflow vocabulary |
| `workflow.sequence` | No normative ordering of Actions array | **Potential gap** | Should action instances have explicit order? |
| `workflow.dependsOn` | No direct causal dependency relation between action items | **Potential gap** | Strong candidate |
| Edit loops / iterations | `multipleInstances` + optional timestamps + external process evidence | **Partial only** | No normalized per-instance loop/dependency model |
| Workflow step ID | No obvious standard identifier for each v2 action item | **Potential enabling gap** | Needed for reliable action-to-action references |
| `related` | C2PA `related` actions | Not equivalent | `related` normally groups associated actions, not causal dependencies |
| `when` | C2PA action timestamp | Partial | Timestamp is optional and explicitly non-trusted |
| Audit log | External Reference `c2pa.types.audit-log` | Covered infrastructure | Log format is implementation dependent |
| Version history | External Reference `c2pa.types.version-history` | Covered infrastructure | History format is implementation dependent |
| `contributions.concept` | Custom action or metadata | Partial | Is intellectual contribution an Action at all? |
| `contributions.expertise` | Metadata/custom assertion | Partial | No standard semantic term |
| `contributions.argument` | Metadata/custom assertion | Partial | No standard semantic term |
| `contributions.creative-direction` | Custom action/metadata | Partial | No standard semantic term |
| `contributions.brainstorming` | Custom action/metadata | Partial | No standard semantic term |
| `contributions.research` | Ingredients / metadata / custom action | Partial | Input provenance exists; contribution semantics do not |
| `contributions.research-assistance` | Custom action | Partial | No standard semantic term |
| `contributions.facts` | Ingredients / metadata | Partial | "supplied facts" is not equivalent to content transformation |
| `contributions.fact-checking` | Custom action / metadata | Partial | No standard semantic term |
| `contributions.structure` | `c2pa.edited` / custom action | Partial | Technical edit can be expressed; semantic role is not standardized |
| `contributions.drafting` | `c2pa.addedText`, `created`, `edited` | Partial | Drafting intent is not standardized |
| `contributions.wording` | `c2pa.edited` / `addedText` | Partial | Semantic role is not standardized |
| `contributions.summarization` | Custom action | Partial | No standard action |
| `contributions.review` | AI Disclosure `human_validated` + custom action | Partial | Human validation is coarse, not a detailed review activity |
| `contributions.approval` | `human_validated`, CAWG role, endorsement concepts | Partial | Approval of a specific workflow step needs clearer pattern |
| `influence` | CAWG creator/contributor role + AI human oversight are adjacent concepts | **Potential gap** | Is qualitative influence useful/defensible enough to standardize? |
| Step-level provider attestation | C2PA assertions + CAWG organizational identity can form a pattern | Partial | No obvious standardized "provider attests this generation step" profile |

---

## 4. Actions already provide much of the workflow foundation

C2PA `c2pa.actions.v2` already describes actions that affect an asset's content.

An action may include:

- an action type,
- a human-readable description,
- an optional timestamp,
- a `softwareAgent`,
- a `digitalSourceType`,
- affected Regions of Interest,
- action-specific parameters,
- ingredient references,
- references to other assertions,
- related actions.

C2PA also allows entity-specific action names and open-ended custom parameters.

This means a large part of the HACP workflow can already be represented without changing C2PA.

For example, a writing tool could already record an entity-specific action:

```text
com.bajonczak.hacp.drafting
```

or add custom parameters to `c2pa.edited`.

The question is therefore not whether C2PA is extensible enough.

It is.

The important question is whether certain semantics should become **interoperable and standardized** rather than remaining vendor-specific custom fields.

---

## 5. Candidate gap #1: action ordering

This currently appears to be one of the strongest gaps.

C2PA explicitly states that, except for a few mandatory first-action rules, the order of entries in the Actions array is unspecified and **does not imply the order in which actions were performed**.

An Action may contain `when`, but C2PA describes it as a simple, non-trusted timestamp.

Therefore this:

```text
Action A
Action B
Action C
```

does not normatively mean:

```text
A → B → C
```

Even if timestamps are included, a consumer has only advisory chronological data.

This matters for human/AI workflows because the order often changes the meaning of the provenance.

Compare:

```text
Human concept
    ↓
AI drafting
    ↓
Human review
```

with:

```text
AI drafting
    ↓
Human review
    ↓
AI rewrite
```

Both workflows may contain the same actors and similar actions.

They do **not** describe the same human involvement.

A final human review before publication is materially different from a human review followed by another AI rewrite.

### Why timestamps alone are insufficient

Timestamps have several limitations:

- they are optional,
- they are explicitly non-trusted,
- clocks may differ between providers,
- two activities may overlap,
- a workflow dependency does not necessarily imply immediate chronological succession,
- parallel work cannot be represented clearly by simple ordering,
- privacy-preserving implementations may intentionally avoid precise timestamps.

A causal relationship is different from a timestamp.

---

## 6. Candidate gap #2: causal dependencies

HACP introduced `dependsOn` to distinguish display ordering from provenance dependency.

Example:

```text
               ┌── AI research ──────┐
Human concept ─┤                      ├── Draft
               └── Human research ───┘
```

The Draft depends on both research branches.

A linear timestamp list loses that relationship.

### C2PA `related` is not the same thing

C2PA v2 Actions have a `related` field.

However, the specification describes it as a mechanism for a series of related actions, usually actions occurring together, with related action objects represented as subsets of the primary action.

That is useful grouping semantics.

It does not appear to express:

> Action C used the outputs of Action A and Action B and therefore causally depends on both.

### Ingredients are also different

Ingredients provide powerful asset-level relationships such as:

- `parentOf`,
- `componentOf`,
- `inputTo`.

They can therefore represent that one **asset or input** contributed to another asset or process.

This is not identical to action-instance dependency inside a creation workflow.

Example:

```text
Human provides concept
AI performs research
Human corrects research
AI drafts based on corrected research
```

Not every conceptual workflow step necessarily produces a separate ingredient asset.

### Candidate requirement

A machine-readable workflow may need a way to express:

```text
ActionInstance B dependsOn ActionInstance A
```

or:

```text
ActionInstance D dependsOn [B, C]
```

without relying on timestamps.

---

## 7. Candidate gap #3: action instance identity

A dependency graph requires stable action-instance references.

C2PA Actions v2 contains action type, timestamp, regions, software agent and parameters, but there is no obvious general-purpose standard ID on each individual v2 action item that can be referenced from another action.

Without an action-instance ID it becomes difficult to express:

```text
draft-2 dependsOn review-1
```

in a standard interoperable way.

An entity-specific parameter can solve this experimentally today, but other implementations will not understand the semantics unless they implement the same extension.

This suggests that **action instance identity may be the enabling primitive** required before causal workflow relationships can be standardized.

---

## 8. Edit loops expose the ordering problem clearly

Iterative writing and AI-assisted editing are a useful test case.

Consider:

```text
1. Human concept
2. AI draft
3. Human review
4. AI rewrite
5. Human fact-check
6. AI wording pass
7. Human approval
```

This is a normal collaborative workflow.

C2PA can represent many of the individual activities.

The difficulty is preserving the semantics of the loop.

### `multipleInstances` is not sufficient

C2PA provides `multipleInstances` for an action that was performed repeatedly with the same parameters/settings.

That can state:

```text
editing happened multiple times
```

but not:

```text
AI draft #1
    ↓
Human review #1
    ↓
AI edit #2
    ↓
Human review #2
```

The distinction matters.

The workflow can determine whether the final asset:

- was reviewed after the last AI transformation,
- contains an AI modification after human approval,
- passed through repeated human validation cycles,
- branched into alternative drafts,
- combined multiple research sources before drafting.

### Edit loops do not require cyclic provenance graphs

A useful distinction:

The *process* may be described informally as a loop:

```text
AI edit ↔ Human review
```

but the provenance graph should still remain acyclic because every execution is a new action instance:

```text
AI edit #1
    ↓
Human review #1
    ↓
AI edit #2
    ↓
Human review #2
```

Therefore the missing primitive is probably **not support for graph cycles**.

It is support for:

- unique action instances,
- causal dependencies,
- optionally iteration/phase grouping.

That keeps provenance deterministic and traversable.

---

## 9. C2PA Process Evidence partially covers this

C2PA 2.4 provides external process-evidence types through External Reference assertions:

- `c2pa.types.audit-log`
- `c2pa.types.version-history`

A hashed external reference can therefore cryptographically bind an external creation-process log or version history to the manifest.

This is important and means C2PA already has a mechanism for preserving richer process history.

However, the specification explicitly leaves the structure of these logs and version-history records **implementation dependent**.

This leads to an important distinction:

### C2PA already provides

> A standard way to cryptographically reference a process log.

### It does not currently appear to provide

> A standard interoperable semantic model for action ordering, dependencies and edit iterations inside that process log.

Therefore the gap may not require putting an entire workflow graph directly into `c2pa.actions`.

Another possible direction is to standardize a creation-process evidence schema that C2PA references.

This needs community discussion.

---

## 10. Candidate gap #4: contribution semantics are not always Actions

This is the other important conceptual difference between HACP and C2PA Actions.

C2PA describes Actions as edits and other actions that affect an asset's content.

Many HACP contribution types map cleanly or approximately:

| HACP contribution | C2PA representation |
|---|---|
| translation | `c2pa.translated` |
| editing | `c2pa.edited` or more specific Action |
| generation | `c2pa.created` + digital source type |
| wording | `c2pa.edited` / `c2pa.addedText` |
| drafting | `c2pa.created`, `c2pa.addedText`, or custom Action |
| structure | `c2pa.edited` or custom Action |

Other HACP concepts are not simply transformations:

```text
concept
expertise
argument
creative-direction
brainstorming
research
research-assistance
facts
fact-checking
review
approval
```

For example:

> A domain expert supplied the technical concept and facts, but did not type any of the final sentences.

That is a meaningful human contribution to the content.

It may not naturally be described as an edit operation on the asset.

### Existing ways to encode this

C2PA and CAWG are flexible enough that such information could already be represented using:

- entity-specific actions,
- custom action parameters,
- custom assertions,
- CAWG Metadata Assertions,
- Dublin Core `dc:contributor`,
- CAWG Identity Assertions,
- external process evidence.

So this is **not a missing extensibility mechanism**.

The possible gap is a missing **shared contribution vocabulary and workflow semantics**.

Without shared semantics, one vendor might write:

```text
com.vendorA.concept
```

another:

```text
org.vendorB.ideation
```

and another might store the same claim as arbitrary metadata.

All are technically valid.

They are not interoperable.

---

## 11. CAWG already covers contributor identity better than HACP

HACP's `human` actor model should not compete with CAWG.

CAWG Identity Assertion already provides:

- verifiable named actors,
- multiple actors,
- cryptographic identity binding,
- creator/contributor/editor/producer/publisher/sponsor/translator roles,
- the ability for an identity assertion to reference other C2PA assertions.

The CAWG specification explicitly notes that an actor can reference a `c2pa.actions` assertion to attest that they performed those actions.

That is significantly stronger than HACP's self-declared human actor object.

### Conclusion

HACP should not invent human identity.

Any future contribution-workflow model should reuse CAWG identity.

---

## 12. Actor-to-specific-step attribution is possible, but awkward

CAWG identity binds a named actor to one or more **assertions**.

An Actions assertion may contain multiple action items.

Therefore:

```text
Identity → Actions Assertion
```

may bind the actor to all relevant actions in that assertion.

The CAWG specification explicitly says an Actions assertion may be referenced to attest that the actor performed those specific actions.

This means action-level actor attribution can potentially be achieved by organizing actions into actor-specific assertions, possibly even one Actions assertion per logical actor contribution.

That suggests actor-to-step attribution is **not necessarily a fundamental missing capability**.

However, it creates practical questions:

- What if an Actions assertion contains actions from multiple humans?
- What if one human wants to attest only one action item?
- Should tools split Actions assertions by actor?
- Can a provider attest only to a single AI action without implicitly attesting the rest?
- How should consumers display those relationships?

This is best treated as an interoperability/profile question before proposing a new primitive.

---

## 13. AI systems and providers are mostly covered

HACP's `systems` array is largely redundant with existing mechanisms.

C2PA provides:

- `softwareAgent`,
- lists of `softwareAgents`,
- `c2pa.ai-disclosure.modelName`,
- `c2pa.ai-disclosure.modelIdentifier`,
- `digitalSourceType`.

Actions can also reference other assertions using `parameters.relatedAssertions`.

This creates a promising pattern:

```text
Action
   │
   ├── softwareAgent → user-facing tool
   │
   └── relatedAssertions → AI Disclosure
                              │
                              └── model identity
```

### Small remaining question: provider organization

AI Disclosure has model name and model identifier, but no obvious standardized first-class field specifically named `provider`.

A provider may be inferable from:

- the model identifier,
- software agent information,
- entity-specific generator fields,
- organizational identity assertions.

This is probably a small interoperability issue rather than a major missing feature.

---

## 14. AI Disclosure covers human oversight, but not contribution significance

C2PA AI Disclosure currently defines:

- `fully_autonomous`
- `prompt_guided`
- `human_validated`

This solves an important part of HACP's original problem.

It tells a consumer whether an AI generation pipeline had final human validation.

However, HACP's `influence` field describes something different:

```text
minor
supporting
substantial
primary
```

Example:

```text
Human concept        primary
AI research          supporting
AI drafting          primary
Human proofreading   minor
Human approval       primary
```

`humanOversightLevel` cannot express this because it applies to human involvement in the AI generation pipeline, not the significance of each actor's contribution.

CAWG roles such as `creator` and `contributor` provide some nearby semantics but remain asset-level roles rather than per-step influence.

### Is this actually worth standardizing?

This is uncertain.

"Influence" is inherently subjective.

Two actors may disagree about whether a contribution was `supporting` or `substantial`.

Therefore influence may be better treated as:

- declared metadata,
- UX information,
- policy input,
- or an optional extension,

rather than a trusted objective provenance fact.

This should **not** be the first proposed C2PA change.

---

## 15. Human review is already partly represented

HACP explicitly records:

```text
fact-checking
review
approval
```

C2PA AI Disclosure's:

```text
human_validated
```

already means that a human reviewed/approved the final AI output before release.

That covers the coarse use case.

HACP remains more detailed because it can distinguish:

```text
proofreading
fact-checking
editorial review
legal review
final approval
```

Those details could already be represented with:

- custom Actions,
- custom parameters,
- CAWG identity,
- metadata assertions.

Again, the possible missing piece is standardized semantic vocabulary rather than infrastructure.

---

## 16. Regions of Interest already solve partial-content attribution

HACP should not invent its own mechanism for identifying portions of a document.

C2PA Regions of Interest support:

- spatial ranges,
- temporal ranges,
- frame ranges,
- textual ranges,
- identified items.

Textual selectors can identify content in formats such as tagged PDF and Office documents.

Actions can use the `changes` field to associate an action with the affected Region of Interest.

This can represent scenarios such as:

```text
paragraphs 1–3 → human edits
paragraph 4    → AI rewrite
paragraphs 5–7 → human review
```

where the underlying document format allows stable textual selection.

### Conclusion

Partial-document provenance is already substantially covered.

---

## 17. Ingredients already provide input provenance

C2PA Ingredient v3 supports relationships including:

- `parentOf`
- `componentOf`
- `inputTo`

The specification explicitly mentions AI/ML input data as an Ingredient use case.

Therefore research documents, datasets, source articles or previous drafts can potentially be represented as inputs to a content-generation process.

This means HACP does not need to invent a generic input graph.

The remaining distinction is between:

```text
asset/data dependency
```

and:

```text
workflow action dependency
```

Those are related but not identical concepts.

---

## 18. Step/provider attestations: mostly an integration pattern

HACP v0.2 proposes future provider attestations such as:

> An AI provider attests that this AI step was actually performed by its model.

C2PA + CAWG already provide many primitives needed to build such a system:

- signed C2PA assertions,
- gathered vs. created assertions,
- CAWG organizational identity,
- identity assertions referencing specific assertions,
- AI model disclosure,
- actions,
- related assertions,
- external references.

A possible provider-attestation architecture could therefore be:

```text
AI Provider
   │
   │ organizational identity
   ▼
provider-specific generation assertion
   │
   ├── model information
   ├── output/content binding
   └── generation event reference
          │
          ▼
C2PA Action
```

or an AI provider could attest to a narrowly scoped assertion that is then referenced from an Action.

### Remaining question

There does not appear to be a standardized profile saying:

> This AI provider cryptographically attests that model X performed workflow action Y on output Z.

That may be a useful future interoperability profile, but it does not necessarily require a new C2PA core primitive.

---

## 19. Proposed prioritization of gaps

### Priority A — strong candidates

#### A1. Action instance identity

A stable identifier for an individual action execution.

Why it matters:

- required for action-to-action references,
- required for deterministic edit-loop representation,
- useful for visualizers,
- useful for attestations,
- useful for process evidence correlation.

#### A2. Causal action dependencies

A normative way to state:

```text
B depends on A
```

or:

```text
D depends on [B, C]
```

without relying on array order or wall-clock timestamps.

#### A3. Standardized iteration/edit-loop representation

Probably built from A1 + A2 rather than a separate graph primitive.

Potential optional concepts:

- phase,
- iteration,
- supersedes/revises,
- review-of.

The first proposal should remain minimal.

### Priority B — useful, but possibly better outside C2PA Actions

#### B1. Human/AI contribution vocabulary

Terms such as:

```text
concept
expertise
research
brainstorming
creative-direction
fact-checking
review
approval
```

Potential homes:

- CAWG metadata profile,
- C2PA custom assertion,
- entity-specific Action vocabulary,
- standardized creation-process evidence schema.

#### B2. Provider attestation profile

A standard recipe for an AI provider to attest to a generation/contribution step without exposing prompts.

### Priority C — interesting but controversial

#### C1. Qualitative influence

```text
minor
supporting
substantial
primary
```

Useful for UX and governance, but subjective.

This should probably not be the first contribution proposal.

---

## 20. A concrete edit-loop test case

This use case should be used when discussing the gap because it is difficult to reduce to a simple "AI generated" label.

### Scenario

1. Human defines the topic, facts and intended argument.
2. AI A produces a first draft.
3. Human reviews the draft and identifies factual problems.
4. AI A rewrites the relevant sections.
5. AI B performs a wording pass.
6. Human performs final fact-checking.
7. Human approves publication.

Conceptually:

```text
human-concept
      │
      ▼
ai-draft-1
      │
      ▼
human-review-1
      │
      ▼
ai-rewrite-1
      │
      ▼
ai-wording-1
      │
      ▼
human-factcheck-1
      │
      ▼
human-approval-1
```

Now add a branch:

```text
                      ┌── ai-research-1 ───────┐
human-concept ────────┤                         ├── ai-draft-1
                      └── human-research-1 ────┘
```

The important provenance questions are:

- Did the final human approval occur before or after the last AI edit?
- Which research inputs were used by the draft?
- Did the rewrite depend on the human review?
- Was a second AI model used after human validation?
- Which action did each actor attest to?

These questions cannot be reliably answered from an unordered set of actions alone.

---

## 21. Possible experimental encoding today

Before requesting a C2PA specification change, the idea can be prototyped using C2PA's existing entity-specific custom parameters.

For example:

```json
{
  "action": "c2pa.edited",
  "softwareAgent": {
    "name": "Example AI Writing Tool"
  },
  "parameters": {
    "com.bajonczak.hacp.actionId": "ai-rewrite-1",
    "com.bajonczak.hacp.dependsOn": [
      "human-review-1"
    ],
    "com.bajonczak.hacp.contributions": [
      "drafting",
      "wording"
    ],
    "com.bajonczak.hacp.influence": "substantial"
  }
}
```

A subsequent human review could use:

```json
{
  "action": "com.bajonczak.hacp.review",
  "parameters": {
    "com.bajonczak.hacp.actionId": "human-review-2",
    "com.bajonczak.hacp.dependsOn": [
      "ai-rewrite-1"
    ],
    "com.bajonczak.hacp.contributions": [
      "fact-checking",
      "review",
      "approval"
    ]
  }
}
```

This would be valid as an experimental entity-specific extension pattern because C2PA allows custom action names and custom namespaced action parameters.

It would also let the HACP Viewer evolve into a **C2PA workflow experiment** rather than a competing provenance format.

### Important

This encoding is only an experiment.

The field names above are not proposed as C2PA standard fields yet.

The purpose is to test whether:

- the graph semantics are useful,
- consumers can implement them,
- real edit loops need them,
- the model survives realistic authoring workflows.

---

## 22. Alternative: standardized process evidence

Another architecture may be better:

```text
C2PA Manifest
    │
    └── hashed External Reference
            │
            ▼
    standardized workflow log
```

C2PA already supports cryptographically bound audit logs and version histories.

Instead of modifying Actions, a future proposal could define a common interoperable schema for a creation-process workflow log.

That schema could contain:

```text
action instance IDs
dependencies
actors
iterations
contribution semantics
provider attestations
```

Advantages:

- keeps C2PA Actions simple,
- handles very large workflows,
- supports complex edit loops,
- process data may remain external,
- potentially better privacy controls.

Disadvantages:

- external data is less immediately available,
- consumers need another schema,
- workflow semantics are no longer native to Actions,
- may reduce interoperability if optional retrieval is common.

This is a key architectural question for the C2PA community.

---

## 23. Questions to take back to the C2PA / CAWG community

The next discussion should be narrow and concrete.

Suggested questions:

1. **Is there an existing normative mechanism for expressing execution order between individual `c2pa.actions.v2` action items that this analysis has missed?**
2. **Is `related` intentionally unsuitable for causal dependency semantics?**
3. **For iterative workflows, is the intended approach to rely on external `audit-log` / `version-history` process evidence rather than modeling action instances directly?**
4. **Would stable action-instance IDs and a causal relation such as `dependsOn` be useful additions, or would they violate the intended abstraction level of Actions?**
5. **Should intellectual contribution semantics such as `concept`, `expertise`, `fact-checking` and `approval` be modeled as Actions, CAWG metadata, or a separate assertion/profile?**
6. **Is the recommended way to attribute a named actor to a single action to place that action into its own Actions assertion and bind a CAWG Identity Assertion to it?**
7. **Is there already a recommended pattern for an AI provider to attest to one specific generation/edit action using organizational identity plus AI Disclosure?**
8. **Would a standardized creation-process evidence schema be preferable to adding workflow graph semantics to `c2pa.actions.v2`?**

---

## 24. Recommended direction for the HACP project

Based on this analysis, HACP should not currently continue toward a standalone general-purpose provenance standard.

A better direction is:

```text
HACP v0.2
    │
    ▼
C2PA / CAWG mapping
    │
    ▼
prototype missing semantics using entity-specific extensions
    │
    ▼
real-world tests
    │
    ▼
community feedback
    │
    ├── no real gap → retire redundant HACP feature
    │
    └── useful gap → contribute focused proposal upstream
```

The HACP repository can remain useful as:

- historical design exploration,
- gap-analysis workspace,
- interoperability examples,
- experimental extension vocabulary,
- reference viewer,
- proof-of-concept implementation.

The goal should be interoperability, not ownership of a parallel standard.

---

## 25. Suggested next implementation experiment

The most useful next technical experiment is an **edit-loop workflow encoded using standard C2PA Actions plus namespaced custom parameters**.

The experiment should include:

```text
Human concept
      ↓
AI research
      ↓
AI draft
      ↓
Human review
      ↓
AI rewrite
      ↓
Human final review
      ↓
Publish
```

The prototype should test:

- action IDs,
- `dependsOn`,
- multiple software agents,
- AI Disclosure linkage,
- CAWG identity linkage for human steps,
- Regions of Interest for partial text edits,
- final graph rendering in `hacp-viewer`.

If this model works cleanly, it provides evidence for a focused standards discussion.

---

## 26. What should NOT be proposed yet

Avoid proposing these as new C2PA features until the mapping is validated:

- a second signature model,
- a second identity model,
- a second Region of Interest model,
- a generic `AI-generated` flag,
- another AI model metadata structure,
- a parallel content-binding mechanism,
- a new percentage-based authorship metric.

C2PA/CAWG already provide stronger foundations for those areas.

---

## 27. Working hypothesis

The current working hypothesis is:

> The main missing piece is not AI disclosure itself. It is an interoperable way to describe the **causal structure of multi-actor creation workflows**, particularly iterative human/AI edit loops, while reusing C2PA for actions, content binding and AI disclosure and CAWG for actor identity.

That hypothesis still needs validation by the C2PA and CAWG communities.

---

## 28. Primary sources

Only primary specification sources were used for this analysis.

- C2PA Content Credentials Technical Specification 2.4  
  https://spec.c2pa.org/specifications/specifications/2.4/specs/C2PA_Specification.html

- CAWG Identity Assertion  
  https://cawg.io/identity/

- CAWG Identity Assertion 1.3 draft  
  https://cawg.io/identity/1.3-draft%2Bvlei/

- CAWG Metadata Assertion  
  https://cawg.io/metadata/

- CAWG Metadata Assertion 1.2 draft  
  https://cawg.io/metadata/1.2-draft/

---

## 29. Relevant specification facts used in this analysis

The following points are especially important when reviewing the conclusions above:

- C2PA states that the order of Actions array entries generally does not imply execution order.
- `when` is a simple, non-trusted timestamp.
- `related` groups related actions, usually actions occurring together; it is not defined as a causal dependency relation.
- `multipleInstances` indicates that an action occurred multiple times but does not describe each execution instance.
- v2 Actions do not expose an obvious general-purpose standard action-instance identifier.
- custom entity-specific action names are allowed.
- custom namespaced action parameters are allowed.
- Actions can identify `softwareAgent`.
- Actions can refer to non-action assertions using `relatedAssertions`.
- Regions of Interest include textual ranges.
- Ingredient v3 supports `inputTo` for computational processes including AI/ML.
- AI Disclosure provides model provenance and `humanOversightLevel`.
- CAWG Identity Assertions can bind a named actor to referenced C2PA assertions.
- CAWG explicitly describes referencing an Actions assertion to attest that an actor performed those actions.
- External Reference process evidence supports audit logs and version history, while leaving their internal formats implementation dependent.
