# MiroFish CLI

Social simulation engine. Feed it documents and a plain-English requirement; it builds a knowledge graph, spins up AI agents that post and argue on simulated Twitter and Reddit, then writes a report, a machine-readable `verdict.json`, and SVG snapshots.

Fork of [666ghj/MiroFish](https://github.com/666ghj/MiroFish), translated to English and reduced to a CLI that runs on the `claude` or `codex` CLI.

## Install

One-command bootstrap (installs uv if missing, creates `.env`, syncs
dependencies, checks your LLM provider CLI and runs `mirofish doctor`):

```bash
# macOS / Linux
./scripts/setup.sh                      # or: --provider codex-cli, --yes

# Windows (PowerShell)
.\scripts\setup.ps1                     # or: -Provider codex-cli, -Yes
# If script execution is disabled (stock Windows default):
powershell -ExecutionPolicy Bypass -File scripts\setup.ps1
```

Or manually:

```bash
uv tool install git+https://github.com/SCTY-Inc/mirofish-cli
mirofish doctor   # check provider CLI (claude/codex) and config
```

Requires Python 3.11-3.12 and [uv](https://docs.astral.sh/uv/). For development: `uv sync`, then `uv run pytest`.

## Usage

```bash
mirofish run \
  --files policy.pdf context.md \
  --requirement "Predict public reaction over 30 days" \
  --json

mirofish runs list --json
mirofish runs status <run_id> --json
mirofish runs export <run_id> --json
```

`mirofish run` options: `--files` (pdf/md/txt), `--requirement`, `--platform parallel|twitter|reddit` (default `parallel`), `--max-rounds N`, `--output-dir PATH`, `--json`.

- Without `--json`: live progress display on stderr. With `--json`: JSON on stdout.
- Exit code 0 on success, 1 on error (including config errors).

Each run writes an immutable directory under `uploads/runs/<run_id>/` containing `manifest.json`, `report/verdict.json` (prediction, confidence, key dynamics, signals), `report/summary.json`, `report/report.md`, and `visuals/*.svg`.

## LLM provider

Set `LLM_PROVIDER` in `.env` (see `.env.example`). Accepted values: `claude-cli` (default) and `codex-cli`. Anything else exits 1 at startup.

## Acknowledgments

- [MiroFish](https://github.com/666ghj/MiroFish) by 666ghj: original project
- [OASIS](https://github.com/camel-ai/oasis) by CAMEL-AI: multi-agent social simulation framework

## License

AGPL-3.0
