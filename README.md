# Laya Decision Benchmark v1

*Testing an open-weight semantic decision model under decomposition, polarity changes, distribution shift, and live agent-system inputs.*

## Overview

This repository documents a staged evaluation of stock `convaiinnovations/laya` 0.3.20 as a local semantic decision component inside an agent-system architecture. The work began with direct judgment, moved through decomposition and frozen holdouts, explored polarity-based abstention and typed trust, and ended with live shadow evaluation against genuine tool proposals.

The main result is architectural rather than leaderboard-oriented: **facts the runtime already knows exactly should stay deterministic. Probabilistic judgment should be reserved for genuinely unresolved semantic questions.**

## Collaboration

This benchmark series was developed collaboratively by **Zerrius and ChatGPT**. Zerrius directed the research, ran the local experiments, and made the architecture decisions. ChatGPT assisted with experiment design, analysis, failure interpretation, documentation, and synthesis. Other coding agents were used during implementation where appropriate; the benchmark record preserves the evidence rather than attributing correctness to any one model.

## Research question

Can stock Laya serve as a useful local decision component before fine-tuning, and if so, what role should it occupy in a larger agent system?

## Tested configuration

- Model: `convaiinnovations/laya` 0.3.20, stock and not fine-tuned.
- Runtime: PyTorch 2.14.0+cpu; Python 3.14.7.
- Checkpoint: approximately 846 MB cold download.
- Warm load: approximately 2.1 seconds.
- Typical CPU inference: approximately 0.18–0.44 seconds per single case.

## What the benchmark explores

- Direct semantic judgment versus deterministic policy.
- Broad multiclass labels versus narrower atomic predicates.
- Known-case replay versus fresh unseen holdouts.
- Positive/negative asymmetry, negation sensitivity, and polarity inversion.
- Dual-polarity agreement as a selective-abstention mechanism.
- Distribution shift from synthetic cases toward integration-shaped inputs.
- Predicate-specific trust, escalation, promotion, and demotion.
- Live shadow evaluation on genuine read-only tool proposals.

## Key results

- **v0.1 baseline:** 8/12 correct (66.7%). Broad semantics were usable, but policy-shaped failures remained.
- **v0.6 → v0.7:** 100% derived-class accuracy on a controlled replay fell to 14/24 (58.3%) on a fresh unseen holdout. Replay success did not generalize cleanly.
- **v1.3 dual-polarity robustness:** 72/96 cases were accepted by the agreement gate (75.0% coverage), and all 72 accepted cases were correct in that frozen synthetic benchmark.
- **v1.4 integration-shaped holdout:** selective accuracy fell to 87.9% with four false accepts, breaking the zero-false-accept pattern under distribution shift.
- **v2.0A live shadow slice:** the agreement gate covered only 8.3% of labeled predicate-event pairs on genuine read-only tool proposals, revealing a strong synthetic-to-live representation shift.
- **v2.0B controlled ablation:** supplying deterministic READ semantics already known by the runtime raised gate coverage to 98.3%, with 118/118 gated judgments correct. This is an ablation result, not proof that Laya independently understood tool effects at 98.3%.

## Evidence discipline

Percentages from different evidence classes are not treated as interchangeable. The release distinguishes targeted replay, controlled synthetic benchmarks, fresh frozen holdouts, matched ablations, integration-shaped synthetic inputs, and live-observed shadow data. Invalidated runs are preserved rather than silently removed.

See [`methodology/EVIDENCE_AND_METRICS.md`](methodology/EVIDENCE_AND_METRICS.md) for definitions.

## What this benchmark does not claim

- It is not a direct Jev-vs-Laya benchmark.
- It does not establish production reliability.
- It does not show that zero observed false accepts imply zero true failure probability.
- It should not be generalized beyond the tested operational-semantic predicate family.
- v2.0B should not be read as independent semantic understanding because known deterministic facts were supplied to the model.

## Repository structure

```text
laya-decision-benchmark/
├── README.md
├── LICENSES.md
├── report/
├── benchmarks/        # added after public-data audit
├── runners/           # added after code/privacy audit
├── results/           # added after result/privacy audit
├── hashes/
├── methodology/
├── invalidated/
└── docs/
```

## Report

The public research synthesis is available at [`report/LAYA_BENCHMARK_REPORT_V1.md`](report/LAYA_BENCHMARK_REPORT_V1.md).

## Reproducibility

For frozen holdouts, benchmark and runner SHA-256 values were captured before inference. The canonical experiment log records benchmark versions, corrections, invalidated runs, major interpretation changes, and the transition from synthetic testing to live shadow observation.

Raw cases, runners, result files, and live-shadow inputs are being copied into a separate public staging area and audited before they are added here. The canonical local experiment artifacts are not being rewritten in place.

## Licensing

This repository uses split licensing by artifact type. See [`LICENSES.md`](LICENSES.md).

- Code and experiment runners: **Apache-2.0**.
- Benchmark data, documentation, reports, and figures: **CC BY 4.0**, unless otherwise noted.

## Current status

The current Laya research phase is complete. Laya remains useful as a local/offline open-weight fallback, comparison target, and future specialization candidate. In the tested architecture, deterministic machine facts and policy boundaries remain outside the model; semantic judgment is invoked only when deterministic layers cannot resolve the question.
