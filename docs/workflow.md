# Madlib authoring and review workflow

## Project Shape

| Surface | Responsibility |
| --- | --- |
| `docs/manuscript/config.yaml` | Declares the Madlib schema, manuscript titles, narrative moves, method protocol, design principles, pipeline phases, evaluation criteria, QA probes, failure modes, authoring obligations, visualization controls, audit rules, lexicon categories, and slots. |
| `src/config.py` | Validates `madlib:` and rejects malformed sections, empty required lexicons, bad protocol/design/phase/evaluation/probe/failure/obligation rows, malformed visualization controls, and invalid composition depth; records explicit and defaulted config paths. |
| `src/tokens.py` | Uses seeded digest selection so a fixed config produces a stable token plan and config edits produce reviewable changes. |
| `src/composition.py` | Builds multi-paragraph IMRAD, evaluation, reproducibility, limitation, scope, and authoring-contract bodies plus Markdown evidence tables and figure groups from config plus token plan. |
| `src/analysis.py` | Thin orchestrator: `generate_artifacts` loads config, builds the token plan, and delegates to the sibling `analysis_*` modules, returning the artifact-path map. |
| `src/analysis_fields.py` | Builds the configured-field inventory and explicit/default origin counts. |
| `src/analysis_figures.py` | Writes the nine figure PNGs (cover overview, token density, injection flow, section allocation, provenance map, quality-gate matrix, configured-field matrix, section heatmap, origin summary) and the figure registry. |
| `src/analysis_reports.py` | Writes the JSON data artifacts (token inventory, section plan, configured-field inventory, injection trace) and the Markdown summary/configured-field reports. |
| `src/manuscript_variables.py` | Emits the flat `{{TOKEN}}` replacement map consumed by manuscript injection. |
| `src/output_validator.py` | Deterministic project-local output validation: hydrated placeholders, token provenance, figure registry, configured-field origins, declared artifact inventory, and authoring-contract binding; writes `output/reports/output_validation.json`. |
| `docs/manuscript/*.md` | Keeps author-readable section shells; generated prose belongs only under `output/manuscript/`. |

## Method Protocol Contract

Treat `madlib.method_protocol` as the source of the generated Methods section, not as decorative metadata. A fork that changes the generation method should update the protocol rows, `pipeline_phases`, `quality_probes`, `failure_modes`, `audit_rules`, `contribution_claims`, and the project claim ledger in the same change. Each method row should name the action, the evidence artifact, and the output it creates; each pipeline phase should name its input, transformation, output, and guard.

The exemplar's digest method is deterministic by construction: the seed, slot name, category, ordinal, and ordered category inventory are hashed before selecting a token. The configured-field inventory then separates explicit YAML paths from loader defaults, and the slot-to-section allocation records where each token enters the manuscript. Methods prose, evidence tables, cover image, pipeline figures, registry rows, and validation gates all point back to those same source-owned facts.

The method now treats review operations as first-class outputs. A reviewer packet is not just the combined PDF or HTML page; it includes hydrated Markdown, web output, slides, figures, JSON data, reports, validation results, and copy statistics. In path terms, the handoff includes `output/manuscript`, `output/pdf`, `output/web`, `output/slides`, `output/figures`, `output/data`, `output/reports`, and `output/reports/output_statistics.json`. Token-selection invariants are also explicit: only seed, slot name, category, ordinal, and ordered category inventory should change token choices. Renderer state, file-copy order, and hand-edited output are outside the token-selection method.

Forks that add domain claims need additional domain validators and claim-ledger entries before they can claim domain evidence. Changing vocabulary alone does not make the generated manuscript empirically validated, reader-ready, or publication-ready.

## Fork Migration Checklist

Before a fork turns the exemplar into a domain report, update these surfaces together:

- `madlib.method_protocol`, `pipeline_phases`, `quality_probes`, `failure_modes`, `audit_rules`, and `contribution_claims`.
- `src/config.py` when the fork adds, removes, or changes schema fields, defaults, or malformed-value behavior.
- `src/composition.py` when the generated Methods prose or evidence tables change.
- `src/analysis.py` when new method artifacts, reports, figures, or registry rows are needed.
- Stage 04 validators or project-local validators when the fork introduces evidence that generic render checks cannot inspect.
- `tests/` so method rows, generated prose, artifacts, validators, registry coverage, review-packet surfaces, and unresolved-token scans can fail.
- `data/claim_ledger.yaml` so each local claim, non-claim, or domain claim has evidence.
- README/manuscript docs so reviewers know how to inspect generated Markdown, PDF, HTML, slides, figures, data, reports, validation results, and `output/reports/output_statistics.json`.

## Generated Evidence

Stage 02 writes:

- `output/data/token_inventory.json`
- `output/data/section_plan.json`
- `output/data/configured_field_inventory.json`
- `output/data/manuscript_variables.json`
- `output/reports/injection_trace.json`
- `output/reports/madlib_summary.md`
- `output/reports/configured_field_summary.md`
- `output/figures/madlib_cover_overview.png`
- `output/figures/token_density.png`
- `output/figures/token_injection_flow.png`
- `output/figures/section_token_allocation.png`
- `output/figures/provenance_trace_map.png`
- `output/figures/quality_gate_matrix.png`
- `output/figures/configured_field_matrix.png`
- `output/figures/section_configuration_heatmap.png`
- `output/figures/field_origin_summary.png`
- `output/figures/figure_registry.json`
- `output/reports/output_validation.json` (project-local validator report)

The hydrated manuscript includes Abstract, Introduction, Methods, Results, Discussion, Configuration, Evaluation, Reproducibility, Limitations, Scope, Authoring Contract, and References. Methods is deliberately long because it is the exemplar's main contribution: it binds schema validation, review-scenario declaration, explicit/default field tracking, deterministic token planning, invariant review, slot-to-section allocation, conditional section hydration, visual audit figures, claim-ledger alignment, operational phases, review-packet assembly, evidence artifacts, validation gates, and failure-mode checks into one source-owned protocol. Evaluation adds explicit QA probes, while the Authoring Contract states what a human reviewer or downstream fork must still do before treating generated prose as reader-ready.

Stage 03 renders PDF, HTML, and slides from the hydrated manuscript. Stage 04 verifies PDF validity, Markdown, figure registry, evidence registry, design overlays, and artifact manifest. Stage 05 copies ignored generated deliverables into `output/templates/template_madlib/`.

## Boundaries

This exemplar demonstrates configuration-driven token injection, not
natural-language quality or domain scholarship: random word substitution
against a seeded lexicon does not create scholarship, and the generated
manuscript is not a claim of empirically validated or reader-ready
content on its own. Forks that add domain claims need additional domain
validators and claim-ledger entries before they can claim domain
evidence (see the Method Protocol Contract above). The scope stops at
deterministic token planning, section allocation, and evidence-table
generation from `docs/manuscript/config.yaml`; it does not evaluate or assert
the truth of the substituted content itself.

## Variants

[`template_gold_refinement`](https://github.com/docxology/template_gold_refinement) is a worked
**domain-specialization** of this exemplar. It reuses the same deterministic
token-injection composition core (`src/composition.py`, `src/config.py`,
`src/manuscript_variables.py`, and a near-identical `docs/manuscript/config.yaml`
schema) and adds a metallurgy domain skin (`src/assay.py`, `src/purity.py`,
`src/refinery.py`, `src/domain_adapter.py`) that maps the abstract composition
pipeline onto a concrete refinement sequence: ore → smelting → assaying →
cupellation → certification. Use it as a reference for how to take this generic
madlib engine and specialize it to a real domain with its own validators and
claim ledger.

## Template integrity

- Forward backlog: [`TODO.md`](../TODO.md).
- Copy-and-customize config: [`docs/manuscript/config.yaml.example`](manuscript/config.yaml.example).
- Project validation: `uv run --extra dev pytest tests/ --cov=src --cov-fail-under=90`.
- Monorepo-only checks (Stage 02-05 pipeline, `scripts/audit/check_template_drift.py --strict`) run from a [`docxology/template`](https://github.com/docxology/template) checkout.

Output Markdown, PDF, HTML, slides, figures, reports, and copied deliverables are disposable. Regenerate them through Stages 02-05 after source edits; do not hand-edit `output/` artifacts to make a method or documentation claim pass review.
