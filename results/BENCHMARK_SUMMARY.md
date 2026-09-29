# Benchmark summary

This table is an aggregate index of the staged Laya experiment. Scores from different rows are **not automatically comparable** because the evidence class, task formulation, sample size, or measured metric may change between versions.

See [`../methodology/EVIDENCE_AND_METRICS.md`](../methodology/EVIDENCE_AND_METRICS.md) before interpreting cross-version changes.

| Version | Evidence / purpose | Primary result | Main finding |
|---|---|---:|---|
| v0.1 | Frozen direct-judgment baseline, 12 cases | 8/12 (66.7%) | Broad task semantics were usable; failures clustered around authorization, persistence, and destructive-risk judgment. |
| v0.2 | Targeted decomposition of four known failures | 7/7 primitive facts; 4/4 derived outcomes | Separating probabilistic semantic description from deterministic policy recovered the known failures. This was replay, not unseen generalization. |
| v0.3 | Counterfactual test, 20 cases / 10 pairs | 17/20 (85%); 7/10 complete pairs | Multiclass `operation_type` remained fragile, especially for read-only semantics. |
| v0.4-pre | Invalidated diagnostic run | **INVALIDATED** | Runner loaded the v0.3 benchmark instead of the intended diagnostic set. Preserved for provenance and excluded from conclusions. |
| v0.4 | Corrected diagnostic, 18 cases | 13/18 (72.2%); 4/9 pairs | Read-only recognition was a systematic weakness under the broad multiclass abstraction. |
| v0.5 | Controlled atomic-predicate scenarios | 44/48 atomic facts (91.7%); 11/12 derived classes | Narrow independent predicates worked better than a broad synthesized operation label. |
| v0.6 | Exact v0.5 replay + deterministic normalization | 34/36 stage-1 facts; 12/12 derived classes | Controlled replay reached 100% derived-class accuracy. This demonstrated known-case recovery, not generalization. |
| v0.7 | Fresh frozen unseen holdout, 24 cases | 14/24 derived classes (58.3%); 4/12 complete pairs | The v0.6 recovery mechanism did not generalize cleanly. |
| v0.8 | Balanced predicate probe, 36 cases | 27/36 (75%) | Strong positive/negative asymmetry emerged; negative and negated formulations were substantially weaker. |
| v0.9 | Matched paraphrase probe, 48 cases | 36/48 (75%); positives 24/24, negatives 12/24 | Polarity and wording sensitivity were strongly supported, though not a complete explanation of errors. |
| v1.0 | v0.9 replay using binary choice | 35/48 (72.9%) | Replacing the response interface did not remove the negative-class weakness. |
| v1.1 | v0.9 replay with semantic complements | Complement 44/48; polarity-consistent 32/48 | Original/complement disagreement tracked errors and suggested agreement as an abstention signal. |
| v1.2 | Fresh frozen dual-polarity holdout, 32 cases | Gate 23/32 (71.9% coverage); accepted 23/23 correct | First fresh evidence that agreement gating could trade coverage for reliability inside the tested distribution. |
| v1.3 | Fresh frozen robustness holdout, 96 cases | Gate 72/96 (75%); accepted 72/72 correct | Strong synthetic selective-abstention evidence, but still not production proof. |
| v1.4 | Fresh integration-shaped holdout, 64 records | Gate 33/64 (51.6%); 29/33 correct (87.9%) | Distribution shift broke the previous zero-false-accept pattern. |
| v1.5 | Matched representation comparison, 96 items | 61/96 coverage; 57/61 correct | Context quantity alone was not the problem; bare commands can remove semantics while bounded structured context can help. |
| v1.6 | Fresh structured-relevant holdout, 96 cases | 53/96 coverage; 49/53 correct (92.5%) | Reliability remained predicate-dependent; four false accepts remained. |
| v1.7 | Typed trust counterfactual, 96 cases | Typed router 36/96 coverage; 34/36 correct | Predicate-specific trust prevented some errors but sacrificed many correct accepts. |
| v1.8 | More conservative typed trust, 96 cases | 26/96 coverage; 25/26 correct | Safer autonomy cost substantial coverage; fresh evidence demoted path identity. |
| v1.9 | Rolling trust / competence map, 96 cases | 28/96 accepted; 26/28 correct | Promotion remained too permissive; synthetic trust tuning was stopped in favor of live shadow observation. |
| v2.0A | Live-observed read-only tool proposals, 20 events × 6 predicates | Raw 120/120; complement 10/120; gate coverage 8.3% | Strong synthetic-to-live representation shift. Polarity agreement largely collapsed. |
| v2.0B | Matched live ablation with deterministic READ facts supplied | Complement 118/120; gate 118/120; 118/118 gated correct | Supplying facts already known by the runtime restored consistency, supporting a deterministic-substrate-first architecture. |

## Interpretation

The trajectory should not be read as a model leaderboard. The experiment repeatedly changed the question being asked:

1. Can Laya classify broad operational semantics at all?
2. Does decomposition improve the abstraction?
3. Does the improvement survive unseen cases?
4. Can model disagreement be turned into abstention?
5. Does the abstention mechanism survive integration-shaped inputs?
6. Can trust be scoped by predicate rather than assigned globally?
7. What changes when the inputs come from genuine tool proposals?
8. Which information should never have been delegated to a probabilistic model in the first place?

The final architectural conclusion came from that sequence, not from choosing the highest percentage in the table.
