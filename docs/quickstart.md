# Quickstart — Template Madlib

```bash
# Install dependencies
uv sync --extra dev

# Run tests
uv run --extra dev pytest tests/ --cov=src --cov-fail-under=90
```

Stage 02-05 pipeline commands (`scripts/pipeline/*.py --project templates/template_madlib`) run from the [`docxology/template`](https://github.com/docxology/template) monorepo checkout: the `scripts/*` wrappers import `infrastructure.*`, which this standalone mirror does not ship.
