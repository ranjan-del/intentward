# Experiment protocol

> Status: binding for every experiment in this repository.

## Every experiment states

1. Research question
2. Hypothesis (with the result that would disprove it)
3. Threat model
4. Baseline
5. Proposed mechanism
6. Experimental environment
7. Attack scenarios
8. Metrics
9. Results
10. Ablation study
11. Limitations
12. Conclusion

Items 1 to 8 are written **before** the run. A metric or task suite chosen after seeing results is
not evidence.

## Honesty rules

- No manufactured results. No improvement is claimed without a recorded run.
- Every number in a README, doc or paper links to the run directory it came from.
- Negative and surprising results are kept, in `research/failed_experiments/` when they contradict
  a hypothesis.
- No guarantee is claimed beyond what the implementation and tests show, with assumptions stated.
- More security layers are not assumed to be better. Each layer is ablated.

## Reproducibility

| Item | Rule |
|---|---|
| Configuration | Stored in `configs/`, referenced by hash in the run record |
| Randomness | Seeds recorded; real-LLM runs repeated N times with confidence intervals |
| Models | Exact model id, provider, date and parameters recorded |
| Traces | Full traces stored per run in `research/experiments/<run-id>/traces/` |
| Worst-case runs | Use the scripted adversarial model so results are exact |
| Offline first | The test suite and worst-case benchmark run with no API key |

## Storage

```
research/experiments/<run-id>/   config, seed, model, traces, raw output (immutable)
research/results/                summaries linking to runs
research/failed_experiments/     expected, happened, why, what changed, next test
configs/                         baseline, defenses, full, ablations
intentward/benchmark/scenarios/  benchmark cases
```

## The research gap process (per major component)

```
Existing approach -> Limitation -> Failure case -> Research question -> Hypothesis
  -> Proposed architecture -> Experiment -> Results -> Limitation -> Next hypothesis
```

This process matters more than the amount of code.
