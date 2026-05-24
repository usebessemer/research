# Context as architecture

> Draft outline, revised 2026-05-24. Status: outline; prose to follow.

## TL;DR

Where information lives in a project determines when and how an agent uses it, making folder structure the load-bearing element of agentic workflow design rather than mere file organization.

## The architecture in five stages

A small toy example, progressively complicated, surfaces the two axes that make folder structure architectural: the **routing hierarchy** (vertical: global → workspace → task) and the **load pattern** (horizontal: always loaded / on-demand / per-work-item / appendable).

### Stage 0 — Identity only

Before the workspace, there's the chatbot. A user opens a chatbot connected to an LLM, has a conversation, closes the tab. The next session starts from zero. No memory of what was discussed, no continuity of preferences, no identity.

A workspace with a single `CLAUDE.md` file changes that. `CLAUDE.md` carries who the user is (identity), how the agent should behave (conventions, voice rules, mode declarations) and what context lives where, once the workspace grows beyond a single file (routing instructions). What this delivers, that a chatbot connected to an LLM cannot, is persistent state across instances of a chat. Each new session begins by reading `CLAUDE.md`, so the agent picks up where the last session left off. A chatbot, by contrast, begins stateless every time. 

What this example doesn't offer is a place for work to live. The product of each session has only two places to go: forced into `CLAUDE.md`, which pollutes what should remain identity-focused, or accumulated as ad-hoc documents with no convention. What's missing is folder structure: a place for work that isn't identity.

### Stage 1 — Work files

[Add drafts/. First architectural split: always-loaded vs per-task.]

### Stage 2 — References

[Add references/. Third load pattern: on-demand by task type.]

### Stage 3 — Background context

[Add context/. Split between identity and background truth.]

### Stage 4 — Workspace

[Add a nested workspace with its own CLAUDE.md. Routing hierarchy enters: L0 to L1.]

### Stage 5 — Task-level routing

[Workspace CLAUDE.md gets a load/skip table. L2 task routing.]

### The two axes, and why they recur

[Vertical: routing hierarchy. Horizontal: load pattern. The same load-pattern logic repeats at every routing level. That recursion is what makes the architecture scale without learning a new system at each level.]

## AIOS as a worked example at scale

[Map the AIOS root onto Stages 0 through 3. Map one engagement workspace onto Stages 4 and 5. Concrete evidence that the toy example is the real architecture, just at more nodes.]

## Failure modes

- Monolithic context
- Hidden context
- Stale context
- Over-routing

## Adjacent concepts

- ICM (Interpretable Context Methodology), Van Clief and McDermott. The architectural lineage.
- Progressive disclosure
- AGENTS.md / CLAUDE.md conventions
- Memory systems
- Prompt engineering

## References

- Van Clief, A. and McDermott, J. *Interpretable Context Methodology: Folder Structure as Agentic Architecture.* arXiv:2603.16021.
- Matt Pocock, open-source agent-skills repository.
- Anthropic, Claude Code documentation on `CLAUDE.md`.

---

*Outline revised 2026-05-24. Next: fill each stage section into prose, then fill the AIOS worked-example section, then failure modes and TL;DR.*