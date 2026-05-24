# Research

Bessemer's research arm. Comprises two parallel tracks:

- **Applied.** Versioned research tied to specific Bessemer projects. Tracks what each release is meaningfully advancing.
- **Theory.** Free-standing investigations into programming patterns, emerging technology, methodologies, and adjacent ideas. May inform applied work; may stand on its own.

## Structure

```
<root>/
├── applied/
│   └── <project-name>/
│       ├── epochs/           one whitepaper per major version
│       └── cycles/<v{N.M}>/  one folder per minor version, containing
│                             scope, experiment-log, findings, next-questions
└── theory/
    ├── README.md             index of theory pieces
    └── <piece-slug>.md       one file per piece (folder if it grows)
```

## Active applied tracks

| Project | Dev repo | Folder |
|---|---|---|
| agent-dev-team | [usebessemer/agent-dev-team](https://github.com/usebessemer/agent-dev-team) | [applied/agent-dev-team/](applied/agent-dev-team/) |

## How applied research works

A cycle's `scope.md` locks before the corresponding dev cycle begins. The `experiment-log.md` accumulates references to dev-track issues and pull requests throughout the cycle. `findings.md` is written at dev cycle close. `next-questions.md` feeds the subsequent cycle's scope.

Cross-references use full GitHub URLs in both directions. At scale, applied research and the corresponding dev project may be owned by separate contributors; for now they are paired by one operator.

## How theory pieces work

Each piece begins as a feature branch. Drafts iterate on the branch. When ready, merging to main constitutes publication. There is no separate `drafts/` subfolder; main reflects the published state, while feature branches hold in-progress work.

See [theory/README.md](theory/README.md) for the current index.

## Three-layer cadence (applied)

| Layer | Maps to | Artifact |
|---|---|---|
| Epoch | Major version (`v1`, `v2`) | Whitepaper |
| Cycle | Minor version (`v1.6`, `v1.7`) | The four-file cycle folder |
| Sprint | Batch of GitHub issues | Patch versions and experiment-log entries |
