# Context as architecture

## TL;DR

Folder structure is not file organization; it is agentic workflow architecture. Where information lives determines when and how an agent uses it. This paper builds the architecture across five stages, from a single identity file to a fully nested workspace with task-level routing, and proposes that the architecture has two independent axes: a routing hierarchy (L0 root, L1 workspace, L2 task) and a horizontal axis combining load pattern with content type (identity, situational context, references, working files). The horizontal pattern repeats at every routing level, making the architecture compositional: new work scopes use the same pattern at new depths. The paper extends ICM (Van Clief and McDermott) by reframing its five layers as two independent axes, and demonstrates the architecture at scale through a production system the author runs, referred to throughout as AIOS.

## The architecture in five stages

A small toy example, progressively complicated, surfaces the two axes that make folder structure architectural: the **routing hierarchy** (vertical: global → workspace → task) and the **load pattern** (horizontal: always loaded / on-demand / per-work-item).

### Stage 0 — Identity only
```
my-workspace/
├── CLAUDE.md
```

Before the workspace, there's the chatbot. A user opens a chatbot connected to an LLM, has a conversation, closes the tab. The next session starts from zero. No memory of what was discussed, no continuity of preferences, no identity.

A workspace with a single `CLAUDE.md` file changes that. `CLAUDE.md` carries identity: who the user is, how the agent should behave, what conventions and voice rules apply. What this delivers, that a chatbot connected to an LLM cannot, is persistent state across instances of a chat. Each new session begins by reading `CLAUDE.md`, so the agent picks up where the last session left off. A chatbot, by contrast, begins stateless every time. 

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

Identity is in place. Work has nowhere to live. The fix is a `projects/` folder that holds work files separately from identity. In this example three projects are currently underway. Now there are two distinct load patterns: `CLAUDE.md` is always loaded (identity), and `projects/*` files are loaded per task. This creates separation between identity (always loaded) and per-task work (loaded only when relevant). Identity remains clean; work has a stable home; the agent has a target location for output.

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

Identity has a home. Projects have a home. The conventions that apply across all projects do not. The fix is a `references/` folder for durable, cross-project material. In this example a style guide captures the conventions the user wants applied to writing across all projects, such as formatting standards, citation discipline, and argument structure. The style guide is neither identity nor work-in-progress. `references/style-guide.md` is loaded on demand when the task type calls for it (writing). Three load patterns now distinguishable:

- Always loaded → `CLAUDE.md`
- On-demand by task type → `references/`
- Per specific work item → `projects/`

Identity, per-task work, and on-demand reference handle most context needs. What's still missing is always-relevant facts about the user that aren't identity statements. Things like the recent move or a standing appointment shape how the agent should respond, but they aren't durable identity (too situational) and they aren't reference (relevant everywhere, not just for some tasks). They need their own always-loaded home.


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

Identity is durable. Projects come and go. Reference material is timeless. But what about the user's life right now: the recent move or a standing appointment? Facts that are always relevant but never identity. This is the first time two locations share a load pattern but hold different content types. When something gets loaded and what kind of information it is are independent questions; the architecture answers both. `context/about-me.md` holds facts about the user that always inform decisions but aren't identity statements. These shape every interaction but aren't durable identity. Identity should be stable; these change.

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


```markdown
## What to load per task

| Task | Load | Skip |
|---|---|---|
| Drafting chapter X | `chapters/X.md`, `plot-bible.md`, `style-guide.md` | other chapters |
| Editing chapter X | `chapters/X.md`, `feedback/X.md` | `plot-bible.md` |
| Continuity check | `plot-bible.md`, all chapters | `style-guide.md` |
```

That is L2: task-level routing. Notice what is not added: no new folder, no new file, not even a new structural pattern. The addition is content inside an existing file. The workspace's `CLAUDE.md` was already L1 (workspace identity); now it grows a section that handles L2 routing too. The table is a living document, populated when the workspace is first set up and refined as new task types emerge.

With L2 in place, the agent's loading sequence gains a branching step:

1. **At the global level (L0):**
    - Read root `CLAUDE.md`, global `context/*`, and any global `references/*` the task calls for
2. **At the workspace level (L1):**
    - Read `novel/CLAUDE.md` (workspace identity, routing, and the load/skip table)
    - Read `novel/context/*`
    - Match the requested task against the load/skip table. This is L2.
3. **At the task level (now informed by L2):**
    - Load exactly the files the matched row specifies; skip the rest
    - Execute the task

The new step is the table consultation. The agent reads the workspace's `CLAUDE.md`, finds the row that matches the user's request, and now knows precisely which files to load. The table replaces "load everything in the workspace" or "guess what's relevant" with a deterministic rule.

The architecture is now complete. Both axes are fully populated. The horizontal axis (load pattern combined with content type) operates at each routing level; the vertical axis (L0, L1, L2) determines where the agent operates. Every kind of extension uses the same pattern. Add new workspaces, new task types, nested workspaces, new content types. The architecture absorbs all of them by applying its existing patterns at new scopes. No level of complexity is unreachable while still maintaining strict order. The fractal property does the work.

With operations now part of the architecture alongside identity, a dichotomy emerges that is worth articulating directly before the synthesis. That is the next section.

## Identity vs Operations

Throughout the build-up, `CLAUDE.md` files have been described as "identity." That is incomplete. `CLAUDE.md` actually carries two distinct kinds of content, and the distinction matters.

Identity content is who the agent is at this scope. Conventions, voice rules, behavioral patterns, mode declarations. It is declarative and durable. It answers: what kind of agent am I in this context?

Operations content is what the agent does given the scope. Load/skip tables (introduced in Stage 5), task-specific routing, action sequences. It is imperative and situational. It answers: given a task, what should I do?

Both kinds are present in `CLAUDE.md`, especially at L1. The root L0 leans more toward identity (the user's identity is the user's identity, full stop). The workspace L1 balances both: identity for the workspace plus operations for tasks within it.

The split matters for three reasons. **Identity is stable, operations change.** Identity rules rarely shift; operations grow with new task types. Recognizing them as distinct lets you extend operations without touching identity. **Operations are derived from identity.** You can write operations clearly only when identity is clear first. **The architecture's robustness comes from this separation.** Adding a new task type means adding a row to the load/skip table. Identity stays untouched. If they were entangled, every new task would require revisiting identity.

In Stage 5's toy example, identity and operations are co-located in the workspace's `CLAUDE.md`. The same file carries the identity section and the load/skip table. That is the simplest form. More mature workflows separate them further: each stage gets its own `CONTEXT.md` file with explicit Input → Process → Output contracts, and the workspace's `CLAUDE.md` stays identity-only. This is the form Van Clief's methodology specifies for multi-stage pipelines. In practice this paper extends Van Clief's three-part contracts with a Completion field that defines when a stage is done, giving Input → Process → Output → Completion. Co-location is fine for simple workspaces with few task types; separation pays off when task types multiply and per-task instructions grow beyond a row in a table.

Operations are a living layer of the architecture. The load/skip table is populated when the workspace is first set up and refined over time as new task types emerge, existing rules turn out to be incomplete, or task types fall out of use. This refinement happens deliberately, with human review. The agent can propose changes when it notices recurring patterns or repeated insufficiencies, but the table itself stays under human control. Identity, by contrast, stabilizes early and changes rarely.

## The two axes, and why they recur

Step back from the stages and look at the architecture as a whole.

What started as a single file (`CLAUDE.md`) has grown into a structure with two organizing dimensions.

Vertical: routing hierarchy. L0 (root), L1 (workspace), L2 (task-specific operations). An agent's scope deepens as it moves down. Each level scopes what is relevant at that depth.

Horizontal: load pattern combined with content type. At each routing level, the architecture distinguishes always-loaded identity, always-loaded situational context, on-demand reference material (the "factory" in Van Clief's framing: stable, used to make things), and per-task working files (the "product": created and consumed during execution). The categories repeat at every depth.

These two axes are dimensionally independent: knowing the routing level does not fix the content type, and knowing the content type does not fix the routing level. Both axis values are needed to locate the agent's current context. They are also structurally invariant: the horizontal pattern repeats at each routing level. The same categories appear at L0, at L1, and at any nested workspace. This is the fractal property. The agent does not learn a new system at L1; it applies the L0 pattern at the new scope.

The interaction of the two axes does the work. The horizontal axis decides what the agent loads, when, and what kind of information it is. The vertical axis decides where the agent operates and what scope the loading rules apply to. Together they decompose the full context-selection question ("for an agent operating at this scope with this task, what should it see?") into two cleaner sub-questions. The architecture handles each with a different mechanism: folders and load patterns on the horizontal, nested `CLAUDE.md` files on the vertical.

This decomposition is the paper's central conceptual claim. Most working architectures collapse context selection into a single flat dimension and rely on the model to disambiguate. The two-axes framing makes explicit what those architectures leave implicit. This is the synthesis we propose: Van Clief's five-layer ICM model is more clearly understood as two independent axes: routing hierarchy combined with load pattern and content type. That decomposition is what makes the architecture infinitely composable while keeping the operating logic constant.

The architecture as designed is now closed. The remaining sections look at how it operates at scale, where it fails, and what surrounding ideas inform it.

## AIOS as a worked example at scale

The toy workspace built across Stages 0-5 is the architecture in its minimum viable form. The author runs a production instance at much larger scale: a structured environment of folders spanning consulting work, software projects, training, and daily life. The author refers to this instance as AIOS, short for AI Operating System, a framing drawn from Nate Herk's work. It instantiates the same patterns described above, scaled up.

At L0, AIOS's root `CLAUDE.md` carries global identity: who the user is, voice rules, the Three Ms framework that informs his AI work, mode declarations, and routing instructions for everything below. The L0 always-loaded context lives in `context/`: separate files for training, nutrition, household, inbox, and active content planning. Each updates at its own cadence as life updates. On-demand references in `references/` include the consulting SOP, the lead-gen methodology, voice samples, and operating manuals for channels like iMessage. The horizontal pattern at L0 is exactly the pattern Stage 3 introduced.

At L1, AIOS has multiple active workspaces: client engagements under `clients/consulting/<name>/`, coaching workspaces under `clients/coaching/<name>/`, the author's training workspace under `training/`, and others. Each workspace is a fractal of the global architecture, with its own `CLAUDE.md` (workspace identity), its own `context/` for workspace-specific situational facts, and its own structured work folders. Each consulting engagement follows the same form: seven numbered stage folders (`00-context/` through `06-writeup/`), each with files specific to that stage of the engagement.

At L2, AIOS extends beyond the toy example's co-located load/skip table. Each engagement stage has its own `CONTEXT.md` file with explicit Input → Process → Output → Completion contracts, extending Van Clief's three-part form with a Completion field that defines when a stage is done. The workspace's `CLAUDE.md` carries identity plus pointers to those per-stage contracts. File sizes broadly track Van Clief's typical ranges (workspace routing ~300 tokens, per-stage contracts 200-500, reference material 500-2k), with comprehensive reference documents running larger; see the appendix on token-cost methodology. This is the separated form discussed in the Identity vs Operations section, applied where it pays off most: multi-stage workflows where per-task operations exceed what a single load/skip table can capture.

The architecture also demonstrates its compositional payoff at AIOS scale. New workspaces (a new client engagement, a new training block) get spun up by applying the existing patterns at the new scope. The decisions log (`decisions/log.md`) operates as an appendable record across all workspaces. The architecture absorbs work as varied as software development, consulting engagements, training programming, and coaching, without requiring a fundamentally different system for each. The same routing hierarchy and the same load patterns serve all of them.

What changes at scale is content density, not architecture. AIOS has hundreds of files where the toy workspace has a handful. But the categorization, the routing, the loading rules: none of these change. That stability is the architecture's payoff.

## Failure modes

The architecture's clarity makes its failure modes specific and identifiable. Five recurring patterns where the methodology gets misapplied, followed by the scenarios where it does not apply at all.

**Monolithic context.** Everything written into a single `CLAUDE.md` (or single file at any routing level). No load patterns, no scoping. Every session loads the entire file regardless of relevance. Context windows degrade; the "lost in the middle" problem dominates. The fix is recovering the categories: identity, situational context, references, working files. Each goes where its load pattern requires.

**Hidden context.** Information that should inform decisions is in the workspace but not surfaced through any routing layer. The agent never reads it. Common cause: the file exists but no `CLAUDE.md` mentions it, and it lives in a location no load/skip rule routes to. The fix is making routing explicit. If a file matters, the agent must have a path to discover it.

**Stale content.** Loaded but no longer accurate. Worse than missing context because it overrides truth. Common sources: out-of-date situational facts in `context/`, retired conventions still in `references/`, load/skip tables that have not kept pace with how the work has evolved. The fix is treating context and operations as living documents with a deliberate review cadence. Identity stabilizes early; everything else gets revisited.

**Over-routing.** Too many nested workspaces or routing layers, each adding indirection the agent must navigate. The methodology becomes its own obstacle. Three layers (L0, L1, L2) is usually sufficient; going deeper rarely justifies the cognitive overhead, for either the agent or the human reading the structure.

**Layer bloat.** Content at the wrong routing level. Operations content (load/skip tables, task-specific instructions) accumulating in the root `CLAUDE.md` instead of staying in workspaces. Situational facts that should live in `context/` getting baked into identity in `CLAUDE.md`. The fix is recognizing each piece of content's natural scope and putting it where it belongs. The Identity vs Operations section frames the most common variant: operations creeping into identity.

### When the architecture does not apply

ICM is designed for sequential, human-reviewed workflows with periodic agent handoff. It does not fit:

- Real-time multi-agent collaboration with tight message loops
- High-concurrency systems with many simultaneous users
- Mid-pipeline automated branching based on AI decisions, without human review gates

A separate, currently open, problem is traceability. When an output reflects one rule rather than another, the architecture does not provide an easy way to trace back which scope's rule drove the specific output. Van Clief flags this in the source paper. The scoping is clear; the debuggability is not.

## Adjacent concepts

This methodology sits in a broader intellectual neighborhood. The most relevant adjacencies are documented here to position the work and acknowledge what it builds on.

**ICM (Interpretable Context Methodology).** Van Clief and McDermott, Interpretable Context Methodology: Folder Structure as Agentic Architecture, arXiv 2603.16021. The direct architectural lineage. ICM specifies a five-layer model with numbered stages and explicit Input → Process → Output contracts per stage. This paper proposes that ICM's five layers are more clearly understood as two independent axes: routing hierarchy and load pattern combined with content type. The methodology presented here is ICM-shaped, with that structural reinterpretation.

**Unix pipeline design.** "Programs that do one thing. Output of one becomes input of another. Plain text as universal interface." ICM invokes this lineage directly. The same principles operate in this paper's stage contracts: scoped responsibility, plain markdown as the medium, output-becomes-input across folder boundaries. The filesystem-as-architecture move has Unix as its deepest conceptual ancestor.

**Multi-pass compiler design.** Sequential transformation across well-defined intermediate forms. Each pass scopes its concerns; outputs flow forward. Van Clief invokes this alongside Unix in the source paper. The stage pattern in workspace pipelines is the same shape applied to agent workflows.

**Literate programming.** Knuth's idea that documentation and code interleave so the program reads as prose. The `CLAUDE.md` / `CONTEXT.md` convention here is a descendant: structured prose carrying both intent and instruction. Where literate programming wove documentation into code, this architecture weaves instruction into folder structure.

**Progressive disclosure.** Norman, Tognazzini, and the UI design tradition. The underlying principle of the load-pattern axis: surface what is needed for the current task, hide what is not. Originally a principle for designing for human attention; here, a principle for designing against finite context windows.

**AGENTS.md / CLAUDE.md conventions.** Different vendors, same convention: a designated identity file at the root of a workspace, read first when an agent enters. Anthropic's `CLAUDE.md` and others' `AGENTS.md` are local variants of the same architectural pattern. This paper uses `CLAUDE.md` throughout because the author's working environment is Anthropic's, but the methodology generalizes.

**Adjacent but distinct: memory systems.** Memory is agent-side state that persists across conversations. Context-as-architecture is project-side structure that the agent reads each session. They cooperate but solve different problems. A memory system records what the agent has learned; the architecture defines what the agent should read.

**Adjacent but distinct: prompt engineering.** A prompt operates per call: it shapes a single inference's input. The architecture is structural and persistent: it shapes which prompts the agent assembles for itself based on where it is operating. Prompt engineering optimizes within a single call; the architecture optimizes which calls happen and what they carry.

## References

Anthropic. Claude Code documentation. https://docs.anthropic.com/en/docs/claude-code

Knuth, D. E. (1984). Literate Programming. The Computer Journal, 27(2), 97-111.

Norman, D. A. (1988). The Design of Everyday Things. Basic Books.

Pocock, M. agent-skills (https://github.com/mattpocock/skills). Implementation reference for skill-level progressive disclosure.

Ritchie, D. M., and Thompson, K. (1974). The UNIX Time-Sharing System. Communications of the ACM, 17(7), 365-375.

Van Clief, J., and McDermott, D. (2026). Interpretable Context Methodology: Folder Structure as Agentic Architecture. arXiv:2603.16021. https://arxiv.org/abs/2603.16021

## Appendix: Token-cost methodology

The main text cites approximate per-layer token ranges (workspace routing ~300 tokens, stage contracts 200-500, reference material 500-2,000). Those ranges originate in Van Clief's ICM paper. This appendix documents their source, validates them against a production system, and gives a reproducible measurement method.

### What a token is

A token is the unit of text an LLM processes, roughly a word-fragment. Token counts matter here because the context window is measured in tokens, and the per-layer budget determines how much of that window each part of the architecture consumes on a given task.

### Source ranges

The ranges in the main text are reported in Van Clief and McDermott's ICM paper: approximately 300 tokens for workspace-level routing (L1), 200 to 500 tokens for per-stage contracts (L2), and 500 to 2,000 tokens for reference material (L3).

### Measurement against a production system

To check whether these ranges hold in practice, representative files from AIOS were tokenized. Counts were produced with tiktoken using the cl100k_base encoding. tiktoken is OpenAI's tokenizer, used here as an accessible proxy for Claude's tokenizer; the two differ in detail but produce counts in the same range. The method is reproducible: install tiktoken, encode the file's text, count the tokens.

| AIOS file | Layer | Measured tokens | Van Clief range |
|---|---|---|---|
| Workspace router (a consulting engagement `CLAUDE.md`) | L1 | 431 | ~300 |
| Stage contract (a discovery-stage `CONTEXT.md`) | L2 | 331 | 200-500 |
| Focused reference (`voice.md`) | L3 | 734 | 500-2,000 |
| Comprehensive reference (the consulting SOP) | L3 | 3,822 | 500-2,000 |

### Interpretation

Stage contracts and focused references track Van Clief's ranges directly. The workspace router runs somewhat heavier than the reported ~300 tokens (431 measured) because AIOS routers carry both a load/skip table and a stage-status table, more than a minimal routing file.

The clearest divergence is the comprehensive reference. A full methodology document like the consulting SOP runs roughly 3,800 tokens, nearly double Van Clief's upper bound for L3. The difference is one of kind: Van Clief's L3 examples are focused references (design systems, voice rules, conventions), whereas a complete SOP is comprehensive. The implication is that the upper bound on reference material is not fixed. It scales with how comprehensive the reference is, and comprehensive references are candidates for splitting, or for loading only the relevant section on demand.

### Why it matters

Per-layer token counts are not trivia. They determine how much of a finite context window each part of the architecture consumes on a given task. A workspace that loads a 3,800-token SOP on every task, when only a 300-token section is relevant, spends its context budget poorly and is more exposed to the lost-in-the-middle problem. Measuring the actual token cost of each file is how you know whether the architecture loads efficiently or quietly wastes the window.

