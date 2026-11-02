[한국어 (Korean)](./README.ko.md) | **English**

# Open-Source Software Developer (Apache-2.0)

AI/ML Verification & Software Quality Engineering

Python tools for general software engineering, ML hyperparameter-validation, LLM evaluation and static analysis.

## Public tools and verification contracts

| Tool | GitHub Release | Package version / distribution | Verification contract |
|---|---|---|---|
| [antemortem-cli](https://github.com/hibou04-ops/antemortem-cli) | [v0.12.4](https://github.com/hibou04-ops/antemortem-cli/releases/tag/v0.12.4) | `0.12.4` · [PyPI](https://pypi.org/project/antemortem/0.12.4/) | Deterministic code-citation verifier and CI gate with `file:line` grounding and optional SHA-256 binding. Enforces `fabrication_rate` and total `effective_coverage` (`verified / total`); `gate` recomputes decisions from classified evidence and current files, rejecting insufficient coverage and inconsistent saved approvals. |
| [omega-lock](https://github.com/hibou04-ops/omega-lock) | [v0.3.13](https://github.com/hibou04-ops/omega-lock/releases/tag/v0.3.13) | `0.3.13` · [PyPI](https://pypi.org/project/omega-lock/0.3.13/) | Fail-closed holdout auditing for Optuna and ML evaluation. Skipped holdouts retain legacy `passed=True` while enforcing `holdout_evaluated=False` and `deployment_authorized=False`. Pearson linear correlation is combined with absolute and relative score-degradation checks (`max_relative_degradation=0.50`); multiple-testing adjustment is a separate explicit helper. |
| [omegaprompt](https://github.com/hibou04-ops/omegaprompt) | [v2.2.6](https://github.com/hibou04-ops/omegaprompt/releases/tag/v2.2.6) | `2.2.6` · [PyPI](https://pypi.org/project/omegaprompt/2.2.6/) | Provider-neutral LLM evaluation and prompt calibration. Preflight distinguishes unmeasured noise (`PREFLIGHT_UNMEASURED_NOISE`) from a supplied zero value. Only a finite positive noise bound exceeded by the observed improvement, with no blocking traps, permits `PREFLIGHT_VALIDATED`. Every preflight result keeps `holdout_verified=False` and `deployment_authorized=False`. |
| [mini-antemortem-cli](https://github.com/hibou04-ops/mini-antemortem-cli) | [v0.11.4](https://github.com/hibou04-ops/mini-antemortem-cli/releases/tag/v0.11.4) | `0.11.4` · GitHub source release; this version is not on PyPI | Standalone static preflight linter for LLM evaluation configurations, with 11 risk rules including split-ID overlap, few-shot leakage and dead rubric weights. Pydantic is its only direct required runtime dependency; `omegaprompt` is optional through `[bridge]`. Bidirectional model, Enum, JSON and validation-error compatibility is tested explicitly. |
| [mini-omega-lock](https://github.com/hibou04-ops/mini-omega-lock) | [v0.8.6](https://github.com/hibou04-ops/mini-omega-lock/releases/tag/v0.8.6) | `0.8.6` · [PyPI](https://pypi.org/project/mini-omega-lock/0.8.6/) | Standalone measurement-reliability probes. `compute_empirical_noise_ceiling()` computes `mean(abs(delta)) + 1.96 * pstdev(abs(delta)) / sqrt(n)` with population SD; `compute_minimum_detectable_effect()` remains an alias, rather than a power-based MDE claim. Pydantic is the only direct required runtime dependency; `[bridge]` integration has explicit compatibility tests. |
| [antemortem-structural-lab](https://github.com/hibou04-ops/antemortem-structural-lab) | [v0.8.2](https://github.com/hibou04-ops/antemortem-structural-lab/releases/tag/v0.8.2) | `0.8.2a1` · GitHub source release; this version is not on PyPI; research-ledger tag and prerelease package metadata are separate | Contract/oracle-driven counterexample discovery with property-based shrinking and metered C/D exploration. `epsilon_static_floor=0.50` reserves static policy C capacity. After both arms complete actual-call warmup, zero D-only confirmed phenotype gain and strictly lower D yield can trigger `circuit_breaker_triggered` and return remaining D capacity to C when the comparison is resolved; unresolved D evidence stays `UNKNOWN`. Research status remains `M0_HOLD`; utility and independent OOS generalization remain `NOT_ESTABLISHED`. |

Distribution is version-specific: four versions above have published PyPI files; the other two are GitHub source releases. CI and regression success provide evidence for the tested software contracts, rather than utility or generalization on independent external data.

## Short, keyless failure reproducers

Run from each source checkout after installing its declared dependencies. `omega-lock` requires the `[p2]` extra (Optuna). These small offline demonstrations require no API key or model service; dependency installation is separate.

```bash
# antemortem-cli
python examples/reproduce_citation_traps.py
# omega-lock
python examples/reproduce_optuna_traps.py
```

- `antemortem-cli`: blocks coverage 0.10 for `verified=1, fabricated=0, unresolved=9`, inconsistent saved approvals and empty evidence; the fully verified control passes.
- `omega-lock`: denies deployment for No-Holdout `SKIP`, `[1,2,3] -> [-100,-99,-98]` despite Pearson `r=1.0`, and `[0.7,0.8,0.9] -> [0.01,0.02,0.03]` at relative degradation 0.975; the unchanged holdout control passes.

Both scripts exit successfully when the expected blocked/passing outcomes hold. Citation recomputation checks integrity rather than claim truth or source authentication. Preflight does not substitute for independent holdout deployment approval.

## License

The six public tools use Apache-2.0. See each repository's LICENSE and third-party notices.
