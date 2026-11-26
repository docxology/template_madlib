# Quick start

Install uv. This walkthrough selects Python 3.14 to match the pinned
renderer. Run from the repository root.

```bash
uv sync --locked --extra dev --python 3.14
uv run --no-sync --python 3.14 python - <<'PY'
from pathlib import Path
from src.config import load_madlib_config
from src.tokens import generate_token_plan

config = load_madlib_config(Path.cwd())
for choice in generate_token_plan(config).choices[:3]:
    print(choice.variable_name, choice.value, choice.source_key)
PY
```

The token source points to the selected lexicon item in the canonical
configuration. Section titles and render formats do not feed token selection. The seed,
slot identity, category, ordinal, and ordered category vocabulary determine
the choice.

The full test suite includes a subprocess that imports the pinned
template package for manuscript hydration. Use the working-project
registration in the [manuscript guide](manuscript/README.md) and run
`uv run --no-sync --python 3.14 pytest tests/ --cov=src --cov-fail-under=90` from that
project checkout. The shared stages generate and validate the complete
review bundle.
