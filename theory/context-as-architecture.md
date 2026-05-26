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

Identity alone is a name tag, not a workspace. The next decision is where actual work lives, because it can't live inside CLAUDE.md. There are three projects currently underway. In this example there are now two distinct load patterns: `CLAUDE.md` is always loaded (identity, global context) and `projects/*` files are loaded per task. This creates separation between identity (always loaded) and per-task work (loaded only when relevant). Identity remains clean; work has a stable home; agent has a target location for output. 

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

Identity says who. Projects say what. But there is a third category, neither identity nor work-in-progress: durable rules that apply across projects. So far, they have nowhere to go. The style guide captures conventions the user wants applied to writing across all projects, such as formatting standards, citation discipline, and argument structure. The style guide is neither identity nor work-in-progress. `references/style-guide.md` is loaded on demand when the task type calls for it (writing). Three load patterns now distinguishable:

- Always loaded → `CLAUDE.md`
- On-demand by task type → `references/`
- Per specific work item → `projects/`

Identity, per-task work, and on-demand reference handle most context needs. What's still missing is always-relevant facts about the user that aren't identity statements. Things like "currently moving cities" or "managing a chronic injury that affects desk time" shape how the agent should respond, but they aren't durable identity (too situational) and they aren't reference (relevant everywhere, not just for some tasks). They need their own always-loaded home.

### Stage 3 — Background context
```
my-workspace/
├── CLAUDE.md
├── context/
│   └── about-me.md
├── references/
│   └── style-guide.md
└── projects/
    └── project-1.md
```

Identity is durable. Projects come and go. Reference material is timeless. But what about the user's life right now: the recent move or a standing appointment? Facts that are always relevant but never identity. This is the first time two locations share a load pattern but hold different content types. Load pattern (when something gets loaded) and content type (what kind of information it is) are independent axes; the architecture distinguishes both. `context/about-me.md` holds facts about the user that always inform decisions but aren't identity statements. These shape every interaction but aren't durable identity. Identity should be stable; these change.

Everything so far lives at the same routing level. There is no notion yet of a subproject with its own internal structure, or a workspace inside a workspace. Real workflows have nesting. A "novel" project has chapters, references specific to that novel, and a `CLAUDE.md` that governs how the novel work specifically gets done. The flat architecture can't accommodate that.

### Stage 4 — Nested Workspaces

```
my-workspace/
├── CLAUDE.md
├── context/
│   └── about-me.md
├── references/
│   └── style-guide.md
└── projects/
    └── novel/
        ├── CLAUDE.md
        ├── context/
        │   └── plot-bible.md
        └── chapters/
            └── ch-1.md
```
Some projects outgrow being a file. A novel is not a single markdown document; it has chapters, plot notes, character sheets, and conventions specific to that novel only. Once a project's internal structure starts mattering, the flat architecture breaks down. The novel becomes a workspace: a folder with its own internal organization, its own `CLAUDE.md`, and its own context. A workspace's internal architecture is the global architecture, locally instantiated. The novel can have its own `context/` (a plot-bible), its own `references/`, its own work files (`chapters/`). Each workspace takes what it needs from the global pattern without polluting the global context with workspace-specific concerns.

When an agent works inside the novel, it does not replace its global context with workspace context. It adds. The loading sequence:

1. At the global level (L0):
- Read root `CLAUDE.md` (identity and routing)
- Read global `context/*` (situational facts)
- Load global `references/*` if the task type calls for them
2. At the workspace level (L1):
- Read `novel/CLAUDE.md` (workspace identity and routing)
- Read `novel/context/*` (workspace situational facts, like the plot bible)
- Load `novel/references/*` if applicable
3. At the task level:
- Read the work file (the specific chapter)
- Execute the task with the full stacked context

Each layer assumes the layer above is already present. The novel's `CLAUDE.md` does not restate global voice rules; those are already loaded. It contains only what is specific to the novel: voice rules for the protagonist, target reader, structural conventions for chapters.

This is the first time the architecture has two visible axes. The horizontal axis (load pattern combined with content type: identity, situational context, references, working files) operates at each routing level. The vertical axis (routing hierarchy: L0, L1, and eventually L2) determines where the agent operates. The two axes are dimensionally independent: knowing the routing level does not fix the content type, and vice versa. They are also structurally invariant: the horizontal pattern repeats at each routing level. This is the fractal property of the architecture. The same categories appear at L0, at L1, and at any nested workspace, with no special cases at any level.

What L1 does not provide is task-specific routing. The novel's `CLAUDE.md` establishes who the agent is in this workspace, but not which files to load when drafting chapter 3 versus editing chapter 1 versus checking continuity across the novel. The workspace knows its identity; it does not yet specify operations. That is the gap Stage 5 fills.

### Stage 5 — Task-level routing

```
my-workspace/
├── CLAUDE.md
├── context/
│   └── about-me.md
├── references/
│   └── style-guide.md
└── projects/
    └── novel/
        ├── CLAUDE.md       (now includes a load/skip table)
        ├── context/
        │   └── plot-bible.md
        └── chapters/
            └── ch-1.md
```
The workspace knows who it is. It does not know what to do. The novel's `CLAUDE.md` establishes voice rules, target reader, and structural conventions, but says nothing about which files to load when drafting chapter 3 versus editing chapter 1 versus checking continuity across the novel. That gap is the last architectural problem to solve.

The fix is mechanically simple. The workspace's `CLAUDE.md` grows a section that explicitly maps tasks to file loads:


## What to load per task

| Task | Load | Skip |
|---|---|---|
| Drafting chapter X | `chapters/X.md`, `plot-bible.md`, `style-guide.md` | other chapters |
| Editing chapter X | `chapters/X.md`, `feedback/X.md` | `plot-bible.md` |
| Continuity check | `plot-bible.md`, all chapters | `style-guide.md` |

That is L2: task-level routing. Notice what is not added: no new folder, no new file, not even a new structural pattern. The addition is content inside an existing file. The workspace's `CLAUDE.md` was already L1 (workspace identity); now it grows a section that handles L2 routing too. The table is a living document, populated when the workspace is first set up and refined as new task types emerge.

With L2 in place, the agent's loading sequence gains a branching step:

At the global level (L0):
Read root `CLAUDE.md`, global `context/*`, and any global `references/*` the task calls for
At the workspace level (L1):
Read `novel/CLAUDE.md` (workspace identity, routing, and the load/skip table)
Read `novel/context/*`
Match the requested task against the load/skip table. This is L2.
At the task level (now informed by L2):
Load exactly the files the matched row specifies; skip the rest
Execute the task
The new step is the table consultation. The agent reads the workspace's `CLAUDE.md`, finds the row that matches the user's request, and now knows precisely which files to load. The table replaces "load everything in the workspace" or "guess what's relevant" with a deterministic rule.

The architecture is now complete. Both axes are fully populated. The horizontal axis (load pattern combined with content type) operates at each routing level; the vertical axis (L0, L1, L2) determines where the agent operates. Every kind of extension uses the same pattern. Add new workspaces, new task types, nested workspaces, new content types. The architecture absorbs all of them by applying its existing patterns at new scopes. No level of complexity is unreachable while still maintaining strict order. The fractal property does the work.

With operations now part of the architecture alongside identity, a dichotomy emerges that is worth articulating directly before the synthesis. That is the next section.

### Indentity vs Operations

Throughout the build-up, `CLAUDE.md` files have been described as "identity." That is incomplete. `CLAUDE.md` actually carries two distinct kinds of content, and the distinction matters.

Identity content is who the agent is at this scope. Conventions, voice rules, behavioral patterns, mode declarations. It is declarative and durable. It answers: what kind of agent am I in this context?

Operations content is what the agent does given the scope. Load/skip tables (introduced in Stage 5), task-specific routing, action sequences. It is imperative and situational. It answers: given a task, what should I do?

Both kinds are present in `CLAUDE.md`, especially at L1. The root L0 leans more toward identity (the user's identity is the user's identity, full stop). The workspace L1 balances both: identity for the workspace plus operations for tasks within it.

The split matters for three reasons. Identity is stable, operations change. Identity rules rarely shift; operations grow with new task types. Recognizing them as distinct lets you extend operations without touching identity. Operations are derived from identity. You can write operations clearly only when identity is clear first. The architecture's robustness comes from this separation. Adding a new task type means adding a row to the load/skip table. Identity stays untouched. If they were entangled, every new task would require revisiting identity.

In Stage 5's toy example, identity and operations are co-located in the workspace's `CLAUDE.md`. The same file carries the identity section and the load/skip table. That is the simplest form. More mature workflows separate them further: each stage gets its own `CONTEXT.md` file with explicit Input → Process → Output → Completion contracts, and the workspace's `CLAUDE.md` stays identity-only. This is the form Van Clief's methodology specifies for multi-stage pipelines, and the form used in practice in production workspaces. Co-location is fine for simple workspaces with few task types; separation pays off when task types multiply and per-task instructions grow beyond a row in a table.

Operations are a living layer of the architecture. The load/skip table is populated when the workspace is first set up and refined over time as new task types emerge, existing rules turn out to be incomplete, or task types fall out of use. This refinement happens deliberately, with human review. The agent can propose changes when it notices recurring patterns or repeated insufficiencies, but the table itself stays under human control. Identity, by contrast, stabilizes early and changes rarely.

### The two axes, and why they recur

Step back from the stages and look at the architecture as a whole.

What started as a single file (`CLAUDE.md`) has grown into a structure with two organizing dimensions.

Vertical: routing hierarchy. L0 (root), L1 (workspace), L2 (task-specific operations). An agent's scope deepens as it moves down. Each level scopes what is relevant at that depth.

Horizontal: load pattern combined with content type. At each routing level, the architecture distinguishes always-loaded identity, always-loaded situational context, on-demand reference material, and per-task working files. The categories repeat at every depth.

These two axes are dimensionally independent: knowing the routing level does not fix the content type, and knowing the content type does not fix the routing level. Both axis values are needed to locate the agent's current context. They are also structurally invariant: the horizontal pattern repeats at each routing level. The same categories appear at L0, at L1, and at any nested workspace. This is the fractal property. The agent does not learn a new system at L1; it applies the L0 pattern at the new scope.

The interaction of the two axes does the work. The horizontal axis decides what the agent loads, when, and what kind of information it is. The vertical axis decides where the agent operates and what scope the loading rules apply to. Together they decompose the full context-selection question ("for an agent operating at this scope with this task, what should it see?") into two cleaner sub-questions. The architecture handles each with a different mechanism: folders and load patterns on the horizontal, nested `CLAUDE.md` files on the vertical.

This decomposition is the paper's central conceptual claim. Most working architectures collapse context selection into a single flat dimension and rely on the model to disambiguate. The two-axes framing makes explicit what those architectures leave implicit. This is the synthesis we propose: Van Clief's five-layer ICM model is more clearly understood as two independent axes — routing hierarchy combined with load pattern and content type. That decomposition is what makes the architecture infinitely composable while keeping the operating logic constant.

The architecture as designed is now closed. The remaining sections look at how it operates at scale, where it fails, and what surrounding ideas inform it.

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