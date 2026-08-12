# Contributing

Thank you for taking an interest in Human–AI Contribution Provenance.

HACP is an experimental specification and critical feedback is welcome.

---

## What contributions are useful?

Useful contributions include:

- specification issues,
- terminology criticism,
- edge cases,
- interoperability concerns,
- privacy concerns,
- security concerns,
- example workflows,
- JSON Schema improvements,
- C2PA integration proposals,
- implementation experiments.

---

## Design principles

When proposing changes, please prefer solutions that are:

- vendor neutral,
- understandable without proprietary infrastructure,
- privacy preserving,
- interoperable,
- explicit about trust assumptions,
- resistant to fake numerical precision,
- useful for both human-readable and machine-readable representations.

---

## Issues before pull requests

For significant changes to the data model, opening an issue first is encouraged.

Please describe:

1. the problem,
2. a real-world example,
3. the proposed change,
4. compatibility impact,
5. possible alternatives.

---

## Versioning

Until a stable release exists, breaking changes are expected.

Draft versions use:

```text
0.x
```

A future `1.0` release should only happen after:

- the naming is stable,
- interoperability has been tested,
- the vocabulary is sufficiently mature,
- trust and C2PA integration have been reviewed.

---

## Security and privacy

Please do not include:

- API keys,
- private prompts,
- personal conversations,
- confidential documents,
- production credentials,

in examples or issue reports.

---

## Commercial implementations

The specification is intended to remain openly implementable.

Contributors are free to build commercial products and services around it, subject to the repository license and applicable third-party rights.

Commercial implementations do not receive special influence over the open specification solely because they are commercial.
