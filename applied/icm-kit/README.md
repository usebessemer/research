# Applied research: icm-kit

Tracks what each [icm-kit](https://github.com/usebessemer/icm-kit) release is meaningfully advancing, independent of the code. icm-kit operationalizes [*Context as Architecture*](../../theory/context-as-architecture.md) and the [agent-role topology](../../theory/agent-role-topology.md): a spec-driven scaffolder (`init`) and linter (`audit`).

## Current

The v0.1 to v0.5 line shipped the rule model, the §2.5 classifier, and `audit`, hardened by a dry run against a real production workspace. The question that run answered: a synthetic fixture validates rule *mechanics* but not real-tree *calibration and coverage*; the unmodeled-homes, binary-exclusion, nested-stage, and within-cell-pointer-resolution fixes all came from that gap. Next: `init`, then the role-layer SPEC extension.

Cycle folders (`cycles/<vN.M>/`) and an epoch whitepaper are added as the cadence formalizes; see [../README.md](../README.md) for the conventions.
