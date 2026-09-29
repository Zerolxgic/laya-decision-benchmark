# Public Release Manifest — Laya Decision Benchmark v1

This document defines what can be published immediately, what must be audited before release, and what should remain private while turning the Laya research into a reproducible public benchmark package.

## Publish now

- Public README and high-level benchmark narrative.
- Benchmark report after replacing private Drive source links with public repository links or plain source labels.
- Benchmark matrix with aggregate metrics, evidence class, caveats, and architecture impact.
- Model/runtime metadata for stock `convaiinnovations/laya` 0.3.20.
- Benchmark version history from v0.1 through v2.0B.
- Frozen-run SHA-256 hashes for benchmark and runner files.
- Invalidated-run records, including the v0.4 attempt that loaded the wrong benchmark and was excluded.
- Methodology covering scoring, selective accuracy, gate coverage, abstention, evidence classes, and the rule that replay success is not generalization evidence.
- Collaboration statement describing Zerrius's role in directing/running the research and ChatGPT's role in experiment design, analysis, documentation, and synthesis.

## Publish after audit

- Synthetic benchmark cases: check for personal names, private file names, local paths, internal project references, or examples copied from private material.
- Runner scripts: remove hard-coded absolute paths, credentials, API tokens, private endpoints, usernames, and machine-specific assumptions; pin dependencies where practical.
- Raw result files: check prompts, task text, labels, metadata, trace fields, filenames, and exception output for private or machine-specific information.
- Live-shadow ZOMAH slices: apply the strictest review because genuine tool proposals may contain document names, retrieval queries, paths, target arguments, or neighboring context that should not be public.
- Shadow labels and enriched v2.0B inputs: confirm deterministic tool semantics do not expose private registry names, paths, or implementation details unrelated to the benchmark.
- Screenshots, terminal captures, or logs: remove usernames, home-directory paths, account identifiers, Drive IDs, tokens, and unrelated desktop information.

## Keep private

- API keys, credentials, cookies, authentication material, environment secrets, and provider tokens.
- Raw personal documents, Drive content, private knowledge-base excerpts, or retrieval results used only as internal system context.
- Unredacted live tool payloads that expose private file names, paths, arguments, or user data.
- Machine-specific infrastructure details that do not contribute to benchmark reproducibility.
- Private Google Drive URLs/IDs used for the canonical internal log, matrix, or working report once public repository equivalents exist.
- Unrelated ZOMAH implementation/state that is not necessary to understand or reproduce the Laya experiments.

## Local artifact harvest

The raw benchmark package is not currently stored in Google Drive. Harvest it from the local Laya-Lab workspace into a clean staging directory before publishing. Copy files; do not move or rewrite the canonical originals during the first extraction pass.

Expected material includes:

- `cases/` — frozen baseline, diagnostics, holdouts, ablations, trust-policy sets, and live-observed slices.
- `run_*.py` or equivalent runners — exact experiment runners used for each frozen version.
- `results/` — raw valid-run outputs, kept versioned and immutable.
- `labels/` — deterministic labels used for live-shadow evaluation where applicable.
- `smoke.py` and runtime setup notes — useful for reproducing Gate 1.
- Lab README/dependency notes — audit before carrying forward.

## Release gates

- Every public result maps to the exact benchmark version and runner used.
- Fresh holdouts are clearly distinguished from replays and ablations.
- Invalidated runs are retained but excluded from headline metrics.
- No private data, credentials, absolute personal paths, or private source links remain.
- The public report and README use the same metric definitions as the raw artifacts.
- The public package can reproduce at least one representative frozen run from documented commands and dependencies.
- A license is selected before calling the repository an open benchmark.

## Current release state

**Ready now:** narrative, aggregate results, methodology, caveats, provenance approach, collaboration statement, and release structure.

**Not yet extracted:** canonical raw local cases, runners, results, and labels. Those files are the next blocking step before the repository is fully reproducible.
