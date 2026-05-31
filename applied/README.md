# Applied research

Versioned research tied to specific Bessemer projects. Each project tracked here has a paired development repo under `usebessemer/`; this track captures what each release is meaningfully advancing, in a form independent of the code itself.

See [../README.md](../README.md) for the top-level research framing (applied and theory tracks).

## Active tracks

| Project | Dev repo | Folder |
|---|---|---|
| agent-dev-team | [usebessemer/agent-dev-team](https://github.com/usebessemer/agent-dev-team) | [agent-dev-team/](agent-dev-team/) |

The most recent `cycles/<vN.M>/` folder is the current cycle for each project. A cycle is closed when its `findings.md` is written.

## Cadence

| Layer | Maps to | Artifact |
|---|---|---|
| Epoch | Major version (`v1`, `v2`) | A whitepaper synthesizing the work of a major version. Lives in `<project>/epochs/<vN>.md`. |
| Cycle | Minor version (`v1.6`, `v1.7`) | A four-file folder (scope, experiment-log, findings, next-questions). Lives in `<project>/cycles/<vN.M>/`. |
| Sprint | A batch of GitHub issues within a cycle | Reflected as patch versions in the dev repo, and as `experiment-log` entries during the cycle. |

## Cycle folder conventions

Each `cycles/<vN.M>/` folder contains four files. Their purpose and lifecycle:

- **`scope.md`**: locks before the corresponding dev cycle begins. Defines the questions the cycle is investigating and what counts as moving them forward.
- **`experiment-log.md`**: accumulates references to dev-track issues and pull requests throughout the cycle. Cross-references use full GitHub URLs in both directions.
- **`findings.md`**: written at dev cycle close. Captures what was learned, not just what shipped.
- **`next-questions.md`**: feeds the subsequent cycle's `scope.md`.

## Adding a new applied track

A new project gets its own `applied/<project-name>/` folder when its dev repo has an active release cadence worth tracking per cycle. Create the folder, add `epochs/` and `cycles/` subfolders, open the first cycle's `scope.md` before that dev cycle begins, and add a row to the **Active tracks** table above.

## Specs and applied research

Specs themselves live in their development repos, not here. A spec is part of the codebase's contract; it ships and versions with the code. Applied research about how a spec evolves (what each cycle is advancing, which experiments tested it) belongs here, alongside the corresponding code-side cycle work.
