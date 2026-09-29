# Evidence classes and metrics

The benchmark deliberately separates result magnitude from evidence strength. A percentage only means something in the context of how the cases were produced and whether they were previously seen.

## Evidence classes

### Targeted replay

Known cases are rerun after a mechanism, prompt, decomposition, or normalization change.

Use: mechanism diagnosis and regression checking.

Do not use as: evidence of unseen generalization.

### Controlled synthetic benchmark

Cases are constructed to probe a specific semantic behavior under controlled conditions.

Use: isolating failure modes such as negation, polarity, abstraction level, or wording sensitivity.

Do not use as: production-reliability evidence.

### Fresh frozen holdout

Cases are frozen before inference and have not been used to tune the mechanism being tested.

Use: stronger evidence of generalization within the tested distribution.

### Matched ablation

The same frozen cases are rerun with one intentional change to the representation or mechanism.

Use: estimating the effect of that specific change.

Do not use as: an independent sample.

### Integration-shaped synthetic data

Cases approximate the structure and context of the intended agent system rather than a narrow benchmark formulation.

Use: testing whether a mechanism survives more realistic representation and contextual noise.

### Live-observed shadow data

Inputs originate from genuine model-proposed actions observed in the running system, while the judgment model remains non-authoritative and cannot affect execution.

Use: examining synthetic-to-live distribution shift without introducing execution risk.

## Core metrics

### Raw / primary accuracy

The fraction of evaluated labels or cases whose primary model answer matches the benchmark label.

### Complement-derived accuracy

Accuracy when the target proposition is expressed through a semantic complement and converted back to the original label space.

### Gate coverage

The fraction of cases for which the selective mechanism accepts rather than abstains or escalates.

`gate coverage = accepted cases / total cases`

### Selective accuracy

Accuracy only among cases accepted by the gate.

`selective accuracy = correct accepted cases / accepted cases`

Selective accuracy must always be reported together with coverage. A high selective accuracy with very low coverage may represent a highly conservative mechanism rather than broad competence.

### Pair accuracy

For paired counterfactual or semantic-invariance tests, the fraction of pairs for which all required members are correct.

## Reporting rules

1. Replay success and unseen generalization are separate claims.
2. Percentages from different evidence classes are not treated as directly interchangeable.
3. Zero observed false accepts on a modest holdout is not evidence of zero true failure probability.
4. Matched ablations are used to diagnose mechanisms, not counted as independent replications.
5. Invalidated runs are preserved with the reason for invalidation and excluded from headline metrics.
6. When a deterministic fact is supplied to the model in an ablation, the result must not be described as the model independently discovering that fact.
7. Model-level trust is avoided when performance is predicate- or domain-dependent; competence should be scoped to the evidence actually observed.

## Benchmark-specific example

The v0.6 controlled replay reached 12/12 final derived classes. The immediately following fresh v0.7 holdout reached 14/24 (58.3%). The correct interpretation is not that performance simply fell from 100% to 58.3%; the two runs answer different questions. v0.6 showed that the mechanism could recover known cases. v0.7 tested whether that mechanism generalized to unseen cases.

Likewise, v2.0B raised agreement-gate coverage from 8.3% to 98.3% by supplying deterministic READ semantics already known by the runtime. This is evidence about representation and system architecture, not evidence that Laya independently inferred live tool side effects at 98.3% accuracy.
