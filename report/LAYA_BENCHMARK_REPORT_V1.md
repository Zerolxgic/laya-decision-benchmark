# Laya Benchmark Report v1

*From local decision-model experiment to bounded semantic fallback*

## Abstract

This report summarizes a staged evaluation of stock `convaiinnovations/laya` 0.3.20 as a local decision component for ZOMAH. The experiment began with direct semantic judgment, moved through decomposition, unseen holdouts, polarity-based abstention, typed trust policies, and finally live shadow observation.

The central result is architectural: **machine facts that are already known by the system should remain deterministic; probabilistic judgment should be reserved for genuinely unresolved semantic questions.**

## Key conclusion

Laya is useful as a local/offline semantic component and research target, but the tests do not support placing it in front of ordinary tool actions or treating it as a policy authority. ZOMAH should resolve capability, side-effect, provenance, permission, and lifecycle facts deterministically first; judgment models enter only when the deterministic substrate cannot settle the meaning.

## Executive summary

- The first frozen baseline scored 8/12 (66.7%). Laya could recognize broad task semantics, but failures clustered around authorization, persistence, and destructive-risk judgment.
- Breaking broad judgments into narrower semantic primitives improved controlled performance substantially, but a perfect replay on known cases did not generalize to a fresh unseen holdout.
- A dual-polarity agreement gate looked exceptionally strong on synthetic holdouts: v1.2 accepted 23/23 correctly and v1.3 accepted 72/72 correctly while abstaining elsewhere.
- That zero-false-accept pattern broke under integration-shaped distribution shift at v1.4, where selective accuracy fell to 87.9% with four false accepts.
- Typed trust and rolling competence maps reduced exposure to known-weak predicates, but conservative autonomy coverage fell sharply and fresh failures continued to demote previously trusted predicates.
- Live ZOMAH shadow data exposed a deeper lesson: when deterministic READ semantics were supplied, polarity consistency recovered. Those facts were already known to ZOMAH, so asking Laya to rediscover them was unnecessary.

### What this report does not claim

- This is not a direct Jev-vs-Laya head-to-head benchmark. Equivalent Jev runs were not performed across the full experiment sequence.
- The strongest synthetic results are not production proof. Several mechanisms degraded when the input distribution moved closer to integration-shaped or live-observed data.
- v2.0B is a controlled ablation, not evidence that Laya independently understood live tool effects at 98.3%. The experiment explicitly supplied deterministic tool facts already known by ZOMAH.

## 1. Research question and setup

**Question:** Can stock Laya serve as a useful local decision component for ZOMAH before any integration or fine-tuning, and if so, what role should it occupy?

### Setup

- Model: `convaiinnovations/laya` 0.3.20, stock and not fine-tuned.
- Runtime: PyTorch 2.14.0+cpu; Python 3.14.7.
- Checkpoint: approximately 846 MB cold download.
- Warm load: approximately 2.1 seconds.
- Typical CPU inference: approximately 0.18–0.44 seconds per single case.
- Initial lab constraint: CPU-first and isolated from ZOMAH.

### Evidence classes matter

A central discipline in this work was not to treat every percentage as comparable evidence. Known-case replay, fresh frozen holdout, matched ablation, integration-shaped synthetic data, and live-observed shadow data answer different questions. The report therefore tracks both the numeric result and the evidence class that produced it.

See [`../methodology/EVIDENCE_AND_METRICS.md`](../methodology/EVIDENCE_AND_METRICS.md).

## 2. Benchmark trajectory

The useful story is not a monotonic score increase. It is a sequence of confidence updates as the tests became harder and closer to the target environment.

- **v0.1 — Frozen baseline:** 66.7%. Broad semantics were usable, but policy-shaped failures remained.
- **v0.6 → v0.7 — Replay to fresh holdout:** 100% replay became 58.3% final derived-class accuracy on the fresh holdout. Known-case recovery did not generalize cleanly.
- **v0.9 — Semantic invariance:** 75.0% overall, with positives at 100% and negatives at 50%. Polarity and negation sensitivity became visible.
- **v1.2 → v1.3 — Fresh dual-polarity holdouts:** 23/23 and then 72/72 accepted judgments were correct. Agreement gating looked extremely promising inside the tested distribution.
- **v1.4 — Integration-shaped shift:** selective accuracy fell to 87.9% with four false accepts. Contextual contamination broke the zero-false-accept pattern.
- **v1.7 → v1.9 — Typed and rolling trust:** conservative coverage fell to roughly 27–38%. Trust became predicate-specific and evidence-accumulating.
- **v2.0A → v2.0B — Live shadow to controlled ablation:** gate coverage rose from 8.3% to 98.3% when already-known deterministic READ semantics were supplied.

## 3. Findings that changed the architecture

### 3.1 Direct policy judgment was the wrong abstraction

v0.1 established the first boundary: Laya could classify broad semantics, but it was not reliable enough to act as a governance or policy authority. When the four baseline failures were decomposed into descriptive primitives in v0.2, the model recovered 7/7 primitive facts and deterministic policy recovered all four cases.

The architectural lesson was to separate probabilistic semantic description from deterministic policy.

### 3.2 Atomic questions helped, but replay success was misleading

Replacing one broad multiclass operation label with independent predicates materially improved controlled performance. v0.5 reached 91.7% on atomic facts and derived classes. Deterministic normalization in v0.6 then produced 12/12 final derived classes and 6/6 counterfactual pairs on the same known scenarios.

The next fresh holdout, v0.7, cut final derived-class accuracy to 14/24 (58.3%) and complete counterfactual pairs to 4/12 (33.3%). The perfect replay demonstrated orchestration recovery on known cases, not generalization.

**Reporting rule learned from v0.6 → v0.7:** replay success and unseen generalization must be reported as separate claims.

### 3.3 Polarity became a diagnostic signal

v0.8 and v0.9 exposed a pronounced asymmetry: positive examples were much easier than negative ones, and negated target verbs frequently triggered false positives. Replacing `noul` with binary choice in v1.0 did not fix the issue.

But v1.1 revealed that the original proposition and a semantic complement often disagreed in a way that tracked mistakes. That turned polarity consistency into a candidate abstention signal.

### 3.4 The agreement gate worked — until the distribution shifted

On fresh frozen synthetic holdouts, the dual-polarity gate looked unusually strong. v1.2 accepted 23/23 cases correctly at 71.9% coverage, and v1.3 accepted 72/72 correctly at 75.0% coverage.

The mechanism did not survive richer ZOMAH-shaped inputs: v1.4 fell to 87.9% selective accuracy with four false accepts. The failure pattern pointed to contextual contamination rather than simple text length.

### 3.5 Trust became a competence map, not a model-level label

Once reliability proved predicate-dependent, the useful question changed. Instead of asking whether Laya was globally trustworthy, v1.7–v1.9 tracked which predicates had accumulated enough clean evidence to be provisionally accepted and which should always escalate.

Fresh false accepts immediately demoted previously trusted predicates. This reduced unsafe accepts, but conservative coverage fell to roughly 27–38%, showing the cost of autonomy when evidence is thin.

## 4. Live ZOMAH shadow integration

Laya was never given execution authority. ZOMAH captured model-proposed tool calls before execution, sanitized obvious credentials and payload fields, and logged them for shadow evaluation. Existing behavior remained intact, and the observer was designed to fail open so an observer failure could not block the normal tool path.

### v2.0A — representation shift on genuine tool proposals

The first frozen live-observed slice contained 20 genuine Qwen-generated read-only tool proposals. Across six deterministically labelable predicates, raw original propositions were 120/120 correct, but complement-derived accuracy was only 10/120 (8.3%). The agreement gate therefore covered only 8.3% of the labeled predicate-event pairs.

This was a sharp synthetic-to-live representation shift.

### v2.0B — deterministic tool semantics ablation

The exact same 20 events were replayed with deterministic READ semantics explicitly supplied. Raw original accuracy stayed at 100%, complement-derived accuracy rose to 118/120 (98.3%), and gate coverage rose to 98.3% with 118/118 gated judgments correct.

This does **not** show that Laya independently learned live tool effects. It shows that a normalized semantic contract can restore consistency when the contract contains facts the system already knows.

### Architectural implication

Do not spend probabilistic inference rediscovering capability facts, side effects, filesystem boundaries, permissions, provenance, or lifecycle state that the runtime can know exactly. Encode those facts in the deterministic substrate and reserve semantic judgment for questions that remain unresolved after that layer.

## 5. Final disposition

**Research status:** complete for the current phase.

- ZOMAH keeps machine truth and policy boundaries in its deterministic substrate.
- Jev remains the intended primary lightweight judgment layer for rare unresolved semantic cases.
- Laya remains a local/offline open-weight fallback, comparison target, and candidate for future specialization or fine-tuning if a narrow decision problem justifies it.
- Neither judgment model should be called routinely in front of ordinary tool actions; model calls should be rare, bounded, and triggered by unresolved meaning rather than by facts the system already possesses.

### Why Laya still matters

The experiment did not end with “Laya failed.” It produced a more useful placement. The model is inexpensive to run locally, exposes semantic behaviors worth studying, and can remain available when local/offline operation or open-weight specialization matters.

The failure analysis also materially improved ZOMAH by clarifying where probabilistic judgment should stop and deterministic infrastructure should begin.

## 6. Limitations

- Most benchmark cases were synthetic and intentionally narrow. Synthetic robustness is informative but does not substitute for natural workload evidence.
- Several runs were replays or controlled ablations. These are useful for mechanism diagnosis but must not be reported as independent generalization evidence.
- The predicate family was designed around ZOMAH-shaped operational semantics; results should not be generalized to unrelated decision domains.
- Sample sizes were modest relative to any claim of production reliability. Zero observed false accepts on a small holdout is not evidence of zero true failure probability.
- The experiment did not conduct an equivalent full-series Jev comparison, so the final placement of Jev and Laya is an architectural disposition rather than a benchmark ranking.

## 7. Reproducibility and provenance

The canonical experiment record tracks the benchmark sequence, frozen-run hashes, invalidated runs, corrections, result files, and major interpretation changes. For frozen holdouts, both benchmark and runner SHA-256 values were captured before inference.

The experiment also preserved the invalidated v0.4 attempt instead of deleting it after discovering that the wrong benchmark file had been loaded.

Public raw artifacts are being staged separately from the canonical lab and will be added only after privacy and reproducibility review.

## Collaboration

This research was developed collaboratively by **Zerrius and ChatGPT**. Zerrius directed the research, executed the local experiments, selected what to test next, and made the architecture decisions. ChatGPT collaborated on experiment design, analysis, failure interpretation, documentation, and synthesis. Additional coding agents were used as implementation tools where appropriate.

The benchmark is presented as an evidence trail rather than as a claim that any individual tool or model authored the work alone.
