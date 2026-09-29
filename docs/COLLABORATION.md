# Collaboration model

This benchmark was developed as a human-directed, AI-collaborative research project.

## Roles

### Zerrius

- Set the research direction and decided which questions were worth testing.
- Ran the experiments locally and controlled the execution environment.
- Reviewed outputs and decided when results were convincing, suspicious, or worth challenging with a stronger test.
- Made the final architecture decisions for ZOMAH.
- Controlled what material could be published.

### ChatGPT

- Collaborated on experiment design and progression between benchmark stages.
- Helped identify confounds, alternative explanations, and stronger follow-up tests.
- Analyzed result patterns and helped distinguish result, interpretation, and architectural decision.
- Helped maintain evidence-class distinctions between replay, fresh holdout, ablation, synthetic integration-shaped data, and live-observed shadow data.
- Assisted with documentation, public synthesis, benchmark packaging, and release preparation.

### Other AI tools

Additional coding agents and language models were used as implementation tools where appropriate. Their use does not replace the provenance of the actual benchmark artifacts, runners, inputs, outputs, and hashes.

## Responsibility and evidence

AI assistance is disclosed because it materially contributed to the research process. It is not presented as independent experimental evidence.

The benchmark's claims are grounded in recorded experiment artifacts and observed results. Zerrius remained responsible for executing experiments, choosing what entered the system, evaluating whether further tests were required, and deciding what conclusions were adopted.

## Working pattern

The recurring loop was:

1. Form a hypothesis or identify a failure.
2. Design a narrower test.
3. Run the test locally.
4. Inspect the evidence together.
5. Challenge the current interpretation with a stronger evidence class when warranted.
6. Update the architecture only when the evidence justified it.

Several of the most important results came from this process refusing to stop at an apparently successful benchmark. A 100% controlled replay led to a fresh holdout; a zero-false-accept synthetic gate led to integration-shaped testing; synthetic robustness led to live shadow observation.

That iterative pressure-testing process is part of the methodology of this project, not something the public release intends to hide.
