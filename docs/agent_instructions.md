# Agent Instructions — Template Madlib

## Operational Constraints

1. **Config owns the vocabulary.** All lexicon, section titles, method
   steps, and token values live in `manuscript/config.yaml` under
   `madlib:`. Never hard-code token values in source code.
2. **Keep `src/` pure.** Token expansion, lexicon parsing, and composition
   logic belong in `src/` — scripts only orchestrate.
3. **No hand-edited output.** Generated manuscript under `output/manuscript/`
   is disposable. Regenerate from source and config.
4. **QA probes come from config.** Every quality probe in the manuscript
   must correspond to a `madlib.quality_probes` entry.

## Workflow

Run commands from this repository root. Stage commands use the monorepo form
(`scripts/pipeline/stage_02_analysis.py --project templates/template_madlib`):
those scripts import `infrastructure.*`, which this standalone mirror does not
ship, so they run from a `docxology/template` checkout.

1. Edit lexicon or section structure in `manuscript/config.yaml`.
2. Regenerate artifacts and the hydrated manuscript through Stage 02 in the monorepo checkout.
3. Verify here: `uv run --extra dev pytest tests/ --cov=src --cov-fail-under=90`.
