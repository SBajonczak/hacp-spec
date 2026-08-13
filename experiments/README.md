# Experiments

This directory contains test cases for the HACP v0.3 C2PA workflow-semantics experiment.

The JSON files are **illustrative C2PA Actions fragments**.

They are not complete C2PA manifests and are not signed Content Credentials.

The purpose is to test:

- stable action-instance identity,
- causal `dependsOn` relationships,
- edit/review loops,
- branched workflows,
- contribution semantics.

## Files

### `edit-loop.actions.json`

Tests repeated AI editing followed by human review and final approval.

The key question is whether a consumer can determine that the final human approval happened after the final AI transformation.

### `branched-workflow.actions.json`

Tests two parallel research branches feeding one later draft.

The key question is whether causal dependency can be represented independently from timestamps or array order.

## Namespace

Experimental parameters use:

```text
com.bajonczak.hacp.*
```

These are not C2PA-standard fields.
