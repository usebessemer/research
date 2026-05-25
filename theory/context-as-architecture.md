# Context as architecture

> Draft outline, revised 2026-05-24. Status: outline; prose to follow.

## TL;DR

Where information lives in a project determines when and how an agent uses it, making folder structure the load-bearing element of agentic workflow design rather than mere file organization.

## The architecture in five stages

A small toy example, progressively complicated, surfaces the two axes that make folder structure architectural: the **routing hierarchy** (vertical: global → workspace → task) and the **load pattern** (horizontal: always loaded / on-demand / per-work-item / appendable).

### Stage 0 — Identity only
```
my-workspace/
├── CLAUDE.md
```

Before the workspace, there's the chatbot. A user opens a chatbot connected to an LLM, has a conversation, closes the tab. The next session starts from zero. No memory of what was discussed, no continuity of preferences, no identity.

A workspace with a single `CLAUDE.md` file changes that. `CLAUDE.md` carries who the user is (identity), how the agent should behave (conventions, voice rules, mode declarations) and what context lives where, once the workspace grows beyond a single file (routing instructions). What this delivers, that a chatbot connected to an LLM cannot, is persistent state across instances of a chat. Each new session begins by reading `CLAUDE.md`, so the agent picks up where the last session left off. A chatbot, by contrast, begins stateless every time. 

What this example doesn't offer is a place for work to live. The product of each session has only two places to go: forced into `CLAUDE.md`, which pollutes what should remain identity-focused, or accumulated as ad-hoc documents with no convention. What's missing is folder structure: a place for work that isn't identity.

### Stage 1 — Work files
```
my-workspace/
├── CLAUDE.md
└── projects/
    ├── project-1.md
    ├── project-2.md
    └── project-3.md
```

Now the folder has content. There are three projects currently underway. In this example there are now two distinct load patterns: `CLAUDE.md` is always loaded (identity, global context) and `projects/*` files are loaded per task. This creates separation between identity (always loaded) and per-task work (loaded only when relevant). Identity remains clean; work has a stable home; agent has a target location for output. 

Identity plus per-task work is most of what an agent needs, but not all. There is still durable knowledge that applies across projects, such as style guides, methodologies, and conventions. None of it is identity or work-in-progress, and each needs a home of its own.

### Stage 2 — References
```
my-workspace/
├── CLAUDE.md
├── references/
│   └── style-guide.md
└── projects/
    └── project-1.md
```

Now the workspace includes a style guide. It captures conventions the user wants applied to writing across all projects, such as formatting standards, citation discipline, and argument structure. The style guide is neither identity nor work-in-progress. `references/style-guide.md` is loaded on demand when the task type calls for it (writing). Three load patterns now distinguishable:

- Always loaded → `CLAUDE.md`
- On-demand by task type → `references/`
- Per specific work item → `projects/`

Identity, per-task work, and on-demand reference handle most context needs. What's still missing is always-relevant facts about the user that aren't identity statements. Things like "currently moving cities" or "managing a chronic injury that affects desk time" shape how the agent should respond, but they aren't durable identity (too situational) and they aren't reference (relevant everywhere, not just for some tasks). They need their own always-loaded home.



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