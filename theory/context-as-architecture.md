# Context as architecture

> Draft outline, 2026-05-23. Status: outline; prose to follow in subsequent commits.

## TL;DR

Where information lives in a project determines when and how an agent uses it, making folder structure the load-bearing element of agentic workflow design rather than mere file organization.

## The concept

Three core ideas to develop into prose:

1. **Context is routing, not just storage.** A file's location encodes its load pattern. A file in `00-context/` is read every time the workspace is entered; a file in `02-scope/` is read only when working on scope. The path is the routing rule.

2. **Progressive disclosure.** Agents do not need everything at once. They need exactly enough for the current step. Context windows are finite, and so is relevance.

3. **The folder is the methodology.** Structure encodes process. A staged folder with per-stage `CONTEXT.md` contracts is not documentation *about* the methodology; it *is* the methodology.

## Where it shows up in practice

Three real examples to anchor the concept:

- **AIOS root structure.** `CLAUDE.md` provides L0 identity, always loaded and defining who the agent is. `references/` holds reference material loaded as needed. `context/` is always-loaded background. `decisions/log.md` is the appendable record. Each lives where its load pattern requires.

- **The consulting SOP.** Per-stage `CONTEXT.md` files with Input, Process, Output, and Completion contracts. Each stage's contract tells the agent exactly what to load and what to skip. The contract format itself is the routing protocol.

- **Engagement workspace instantiation.** Each consulting engagement instantiates the SOP. The L1 `CLAUDE.md` router contains a load/skip table: "for discovery prep, load 00-context/* and 01-discovery/call-prep.md; skip 03-build/ through 06-writeup/." The structure walks the agent through the work without overload.

## Failure modes

- **Monolithic context.** Everything placed in one large file. No routing, everything loads, context window exhausted. The opposite of progressive disclosure.

- **Hidden context.** Information that should be loaded for a task but is not surfaced in any routing layer. The agent never finds it.

- **Stale context.** Loaded but no longer accurate. Worse than missing context because it overrides truth.

- **Over-routing.** Too many layers, the agent cannot navigate. In practice three layers are usually sufficient: global (L0), workspace (L1), and task (L2).

## Adjacent concepts

- **ICM (Interpretable Context Methodology):** Van Clief and McDermott. The architectural lineage; their five-layer model is the formal expression of what is described here in three layers.

- **Progressive disclosure:** the underlying principle. Borrowed from UI design and newly load-bearing in agentic contexts.

- **AGENTS.md / CLAUDE.md:** the convention for declaring an agent's L0 entry point. Different vendors, same idea.

- **Memory systems:** adjacent but distinct. Memory is *agent-side state* that survives the conversation; context-as-architecture is *project-side structure* that the agent reads each time.

- **Prompt engineering:** related but distinct. A prompt operates per-call. Context-as-architecture is structural and persistent.

## References

- Van Clief, A. and McDermott, J. *Interpretable Context Methodology: Folder Structure as Agentic Architecture.* arXiv:2603.16021.
- Matt Pocock, open-source agent-skills repository.
- Anthropic, Claude Code documentation on `CLAUDE.md`.

---

*Outline drafted 2026-05-23. Next: fill each section into prose, targeting 1 to 3 pages total.*
