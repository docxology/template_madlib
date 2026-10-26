---
name: template-madlib
description: Deterministic token-injection manuscript generator — config-owned lexicon, conditional IMRAD, QA probes, authoring contract, and token provenance.
version: 0.1.1
author: docxology
license: MIT
tags: [exemplar, madlib, token-injection, imrad]
---

# template-madlib

Project-scoped skill for this exemplar. In the `docxology/template` monorepo it
lives at `projects/templates/template_madlib/`; this repository is the standalone
publication mirror, so the project root is the repository root.

## When to Use

- Working inside the `template_madlib` exemplar — running scripts, editing source,
  or regenerating outputs.
- Forking this exemplar as the starting scaffold for a new research project.
- Validating that the exemplar's contracts (thin-orchestrator, layer boundaries,
  no-mocks testing) still hold after changes.

## Quick Reference

```bash
# From this repository root (standalone mirror)
uv run --extra dev pytest tests/ --cov=src --cov-fail-under=90
```

Stage commands (`scripts/pipeline/stage_02_analysis.py` through
`stage_05_copy.py --project templates/template_madlib`) require the
`infrastructure` package from the `docxology/template` monorepo checkout; the
`scripts/*` wrappers in this mirror import it and do not run standalone.

## Pitfalls

- **Keep scripts thin.** Business logic belongs in `src/` modules (`run.py`, `composition_*`, `figure_specs`, `analysis_*`), not in `scripts/`.
- **Import surface.** Prefer `composition.py` re-exports in tests; edit split modules for body/table/figure changes.
- **No mocks.** All tests must use real data, real files, and real computation.
- **Outputs are disposable.** Never hand-edit `output/` — regenerate from
  source and config.
- **Run from the repo root.** Commands assume the template monorepo root
  as working directory unless the child `AGENTS.md` states otherwise.

## Cross-refs

- Project contract: [`AGENTS.md`](../../../AGENTS.md)
- README: [`README.md`](../../../README.md)
- TODO: [`TODO.md`](../../../TODO.md)
- Standalone notes: [`STANDALONE.md`](../../../STANDALONE.md)
