# Contributing

HACP is currently an experimental C2PA / CAWG interoperability project.

Critical feedback is welcome, especially feedback that shows an experimental HACP concept is already solved by an existing standard.

## Upstream-first principle

Before proposing a new HACP field or structure, ask:

1. Can C2PA already represent this?
2. Can CAWG already represent this?
3. Can an existing Action, Region, Ingredient, identity or process-evidence mechanism represent it?
4. Is this only a UX concern rather than a provenance semantic?
5. Does a custom entity-specific extension already provide enough room to experiment?

If the answer is yes, prefer the existing mechanism.

## Useful contributions

- corrections to the C2PA/CAWG mapping,
- examples that break the current dependency model,
- realistic human/AI edit loops,
- privacy concerns,
- interoperability concerns,
- validator implementations,
- C2PA test assets,
- CAWG identity experiments,
- alternative process-evidence designs.

## Proposed workflow

For semantic changes:

1. open an issue,
2. describe the concrete workflow,
3. show how existing C2PA/CAWG mechanisms behave,
4. identify the precise missing semantic,
5. prototype it using an entity-specific extension when possible,
6. seek upstream feedback before treating it as stable.

## Experimental namespace

The current prototype uses:

```text
com.bajonczak.hacp.*
```

This namespace is experimental and must not be presented as an official C2PA namespace.
