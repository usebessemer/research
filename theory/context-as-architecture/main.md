# Context as architecture

*An accessible overview is in [the README](README.md). This is the full paper.*

## TL;DR

Folder structure is not file organization; it is workflow architecture. Where information lives determines when and how an agent uses it. This paper builds that architecture in five stages, from a single identity file to a fully nested workspace with task-level routing, and shows it has two independent axes: a **routing hierarchy** (L0 root, L1 workspace, L2 task) and a **horizontal axis** combining load pattern with content type (identity, situational context, references, working files). The horizontal pattern repeats at every routing level, which makes the architecture fractal: new work scopes reuse the same pattern at new depths. The paper extends ICM (Van Clief and McDermott) by reframing its five layers as these two axes, and demonstrates the result at scale through a production system the author runs, called AIOS.

## The architecture in five stages

A small toy example, progressively complicated, surfaces the two axes that make folder structure architectural: the **routing hierarchy** (vertical: global → workspace → task) and the **load pattern** (horizontal: always loaded / on-demand / per-work-item).

### Stage 0: Identity only
```
my-workspace/
└── CLAUDE.md
```

Before the workspace there is the chatbot. You open a tab, have a conversation, close it; the next session starts from zero. No memory, no continuity, no identity.

A single `CLAUDE.md` changes that. It carries identity: who the user is, how the agent should behave, what conventions and voice rules apply. Each session begins by reading it, so the agent picks up where the last one left off. That is persistent state across sessions, which a stateless chatbot cannot offer.

What it lacks is a place for work. The product of each session must either be forced into `CLAUDE.md`, polluting identity, or pile up as ad-hoc files with no convention.

### Stage 1: Work files
```
my-workspace/
├── CLAUDE.md
└── projects/
    ├── project-1.md
    └── project-2.md
```

The fix is a `projects/` folder that holds work separately from identity. Now there are two load patterns: `CLAUDE.md` is **always loaded** (identity); `projects/*` files load **per task**. Identity stays clean, work has a stable home, and the agent has a target for output.

What is still missing: durable knowledge that spans projects, such as style guides, methodologies, and conventions. None of it is identity or work-in-progress.

### Stage 2: References
```
my-workspace/
├── CLAUDE.md
├── references/
│   └── style-guide.md
└── projects/
    └── project-1.md
```

A `references/` folder holds durable, cross-project material; here a style guide for formatting, citation, and argument structure. It loads **on demand** when the task type calls for it (writing). Three load patterns are now distinct:

- Always loaded → `CLAUDE.md`
- On-demand by task type → `references/`
- Per specific work item → `projects/`

Still missing: always-relevant facts about the user that are not identity. A recent move, a standing appointment. Relevant everywhere, but too situational to be durable identity and too specific-to-the-user to be reference.

### Stage 3: Background context
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

`context/about-me.md` holds facts that always inform decisions but are not identity statements. This is the first time **two locations share a load pattern but hold different content types**: `context/` and `CLAUDE.md` are both always-loaded, yet one is situational and changeable while the other is durable identity. *When* something loads and *what kind* of information it is are independent questions, and the architecture answers both.

Everything so far lives at one routing level. There is no notion yet of a workspace inside a workspace. Real workflows nest: a novel has chapters, novel-specific references, and a `CLAUDE.md` governing how the novel work gets done. The flat architecture cannot hold that.

### Stage 4: Nested workspaces
```
my-workspace/
├── CLAUDE.md
├── context/about-me.md
├── references/style-guide.md
└── projects/
    └── novel/
        ├── CLAUDE.md
        ├── context/plot-bible.md
        └── chapters/ch-1.md
```

Some projects outgrow being a file. Once a project's internal structure starts to matter, it becomes a **workspace**: a folder with its own `CLAUDE.md`, its own `context/` (a plot bible), its own `references/`, and its own work files (`chapters/`). A workspace's internal architecture is the global architecture, locally instantiated. Each workspace takes what it needs from the global pattern without polluting global context with workspace-specific concerns.

When an agent works inside the novel it does not replace global context with workspace context; it **adds**. The loading sequence stacks:

1. **L0 (global):** read root `CLAUDE.md` (identity and routing), global `context/*`, and any global `references/*` the task calls for.
2. **L1 (workspace):** read `novel/CLAUDE.md`, `novel/context/*` (the plot bible), and applicable `novel/references/*`.
3. **Task:** read the work file (the chapter) and execute with the full stacked context.

Each layer assumes the one above is present, so `novel/CLAUDE.md` never restates global voice rules; it carries only what is specific to the novel.

This is the first time two axes are visible. The **horizontal axis** (load pattern plus content type) operates at each routing level; the **vertical axis** (L0, L1, eventually L2) determines where the agent operates. They are **dimensionally independent**: knowing the routing level does not fix the content type, or vice versa. They are also **structurally invariant**: the horizontal pattern repeats at each level with no special cases. That is the architecture's **fractal property**.

What L1 still lacks is task-specific routing. The workspace knows its identity but not which files to load when drafting chapter 3 versus editing chapter 1 versus checking continuity. That is the Stage 5 gap.

### Stage 5: Task-level routing

The fix is mechanically simple. The workspace's `CLAUDE.md` grows a section mapping tasks to file loads:

```markdown
## What to load per task

| Task                | Load                                          | Skip            |
|---------------------|-----------------------------------------------|-----------------|
| Drafting chapter X  | `chapters/X.md`, `plot-bible.md`, `style-guide.md` | other chapters  |
| Editing chapter X   | `chapters/X.md`, `feedback/X.md`              | `plot-bible.md` |
| Continuity check    | `plot-bible.md`, all chapters                 | `style-guide.md`|
```

That is **L2: task-level routing**. Notice what was not added: no new folder, no new file, no new structural pattern. The addition is content inside an existing file; the L1 `CLAUDE.md` now also handles L2. The table is a living document, populated at setup and refined as task types emerge.

The loading sequence gains a branching step: at L1 the agent matches the request against the load/skip table, then at the task level loads exactly the files that row specifies and skips the rest. The table replaces "load everything" or "guess what's relevant" with a deterministic rule.

The architecture is now complete; both axes are fully populated. Every extension, new workspaces, new task types, nested workspaces, new content types, reuses the same pattern at a new scope. No level of complexity is unreachable while strict order holds. The fractal property does the work.

## Identity vs operations

`CLAUDE.md` has been called "identity" throughout. That is incomplete: it carries two distinct kinds of content.

**Identity** is who the agent is at this scope: conventions, voice rules, behavioral patterns, mode declarations. Declarative and durable. It answers *what kind of agent am I here?*

**Operations** is what the agent does given the scope: load/skip tables, task routing, action sequences. Imperative and situational. It answers *given a task, what do I do?*

Both live in `CLAUDE.md`, especially at L1. The root L0 leans toward identity (the user's identity is just the user's identity). L1 balances both.

The split matters for three reasons. **Identity is stable; operations change** with new task types, so separating them lets you extend operations without touching identity. **Operations derive from identity**; you can write them clearly only once identity is clear. **The architecture's robustness comes from this separation**: adding a task type means adding a table row, leaving identity untouched. Entangled, every new task would force revisiting identity.

In the toy example identity and operations are co-located in one `CLAUDE.md`, the simplest form. Mature workflows separate them: each stage gets its own `CONTEXT.md` with an explicit Input → Process → Output contract, and the workspace `CLAUDE.md` stays identity-only. This is Van Clief's form for multi-stage pipelines.

This paper extends that three-part contract with a fourth field, **Completion**. Input → Process → Output defines what a stage consumes, does, and produces, but leaves *when it is finished* implicit, and that implicit field is where agentic workflows break: an agent thinks the work is done because Output was written, while the human or the next stage disagrees. Completion makes the acceptance criteria explicit and verifiable: named output files exist, a schema validates, a reviewer tags the stage complete, or all open questions are resolved or logged. The distinction is between a stage that **terminates** (the agent stops) and one that **completes** (the work is genuinely finished). Without it, the gap stays invisible until it bites.

Co-location is fine for simple workspaces; separation pays off when task types multiply beyond a table row. Operations are a **living layer**: refined over time under deliberate human review as task types emerge, prove incomplete, or fall out of use. The agent can propose changes when it notices patterns, but the table stays under human control. Identity, by contrast, stabilizes early and changes rarely.

## The two axes, and why they recur

A single `CLAUDE.md` has grown into a structure with two organizing dimensions.

**Vertical: routing hierarchy.** L0 (root), L1 (workspace), L2 (task-specific operations). Scope deepens as the agent moves down; each level scopes what is relevant at that depth.

**Horizontal: load pattern plus content type.** At each level the architecture distinguishes always-loaded identity, always-loaded situational context, on-demand reference material (Van Clief's **"factory"**: stable, used to make things), and per-task working files (the **"product"**: created and consumed during execution).

The two axes are **dimensionally independent** (both values are needed to locate the agent's context) and **structurally invariant** (the horizontal pattern repeats at every level; the agent applies the L0 pattern at new scopes rather than learning a new system).

Their interaction does the work. The horizontal axis decides *what* loads, *when*, and *what kind* of information it is; the vertical axis decides *where* the agent operates and which loading rules apply. Together they split the full context-selection question, "for an agent at this scope with this task, what should it see?", into two cleaner sub-questions, each handled by a different mechanism: folders and load patterns on the horizontal, nested `CLAUDE.md` files on the vertical.

This decomposition is the paper's **central conceptual claim**. Most working architectures collapse context selection into one flat dimension and rely on the model to disambiguate; the two-axes framing makes explicit what they leave implicit. This is the synthesis: Van Clief's five-layer ICM model is more clearly understood as two independent axes, routing hierarchy combined with load pattern and content type. That is what makes the architecture infinitely composable while keeping the operating logic constant.

## AIOS as a worked example at scale

The toy workspace is the architecture at minimum viable size. The author runs a production instance, **AIOS** (AI Operating System, a framing drawn from Nate Herk's work), spanning consulting, software, training, and daily life. It instantiates the same patterns, scaled up.

**At L0**, AIOS's root `CLAUDE.md` carries global identity: who the user is, voice rules, the Three Ms framework, mode declarations, and routing for everything below. Always-loaded `context/` holds separate files for training, nutrition, household, inbox, and content planning, each updating at its own cadence. On-demand `references/` holds the consulting SOP, lead-gen methodology, voice samples, and channel manuals. This is exactly the Stage 3 pattern.

**At L1**, AIOS has many active workspaces: client engagements under `clients/consulting/<name>/`, coaching under `clients/coaching/<name>/`, a `training/` workspace, and others. Each is a fractal of the global architecture, with its own `CLAUDE.md`, `context/`, and work folders. Every consulting engagement uses the same form: seven numbered stage folders (`00-context/` through `06-writeup/`).

**At L2**, AIOS uses the separated form rather than a co-located table. Each engagement stage has its own `CONTEXT.md` with an explicit Input → Process → Output → Completion contract; the workspace `CLAUDE.md` carries identity plus pointers to those contracts. File sizes broadly track Van Clief's ranges (workspace routing ~300 tokens, per-stage contracts 200-500, references 500-2k), with comprehensive references running larger (see appendix).

The **compositional payoff** shows at scale: new workspaces spin up by reusing existing patterns; `decisions/log.md` is an appendable record across all of them. The architecture absorbs software work, consulting, training, and coaching without a different system for each. What changes at scale is **content density, not architecture**: AIOS has hundreds of files where the toy workspace has a handful, but the categorization, routing, and loading rules do not change. That stability is the payoff.

## Failure modes

The architecture's clarity makes its failure modes specific.

**Monolithic context.** Everything in one file at some level. No load patterns, no scoping; every session loads it all, context degrades, and "lost in the middle" dominates. *Fix:* recover the categories (identity, situational context, references, working files) and place each by its load pattern.

**Hidden context.** Information that should inform decisions exists in the workspace but is surfaced through no routing layer, so the agent never reads it. *Fix:* make routing explicit; if a file matters, give the agent a path to discover it.

**Stale content.** Loaded but no longer accurate, which is worse than missing because it overrides truth. *Fix:* treat context and operations as living documents with a deliberate review cadence.

**Over-routing.** Too many nested workspaces or routing layers, each adding indirection. *Fix:* three layers (L0, L1, L2) is usually enough; deeper rarely justifies the overhead.

**Layer bloat.** Content at the wrong level: operations creeping into the root `CLAUDE.md`, situational facts baked into identity. *Fix:* put each piece of content at its natural scope. Operations-creeping-into-identity is the most common variant.

### When the architecture does not apply

ICM is built for sequential, human-reviewed workflows with periodic agent handoff. It does not fit real-time multi-agent collaboration with tight message loops, high-concurrency multi-user systems, or mid-pipeline automated branching on AI decisions without human review gates.

A separate open problem is **traceability**: when an output reflects one rule rather than another, the architecture gives no easy way to trace which scope's rule drove it. Van Clief flags this too. The scoping is clear; the debuggability is not.

## Adjacent concepts

**ICM (Interpretable Context Methodology).** Van Clief and McDermott (arXiv 2603.16021). The direct lineage: a five-layer model with numbered stages and Input → Process → Output contracts. This paper reframes those five layers as two independent axes. The methodology here is ICM-shaped with that structural reinterpretation.

**Unix pipeline design.** Programs that do one thing; output of one becomes input of another; plain text as universal interface. ICM invokes this; the same principles run through the stage contracts (scoped responsibility, plain markdown, output-becomes-input across folders). Unix is the deepest ancestor of the filesystem-as-architecture move.

**Multi-pass compiler design.** Sequential transformation across well-defined intermediate forms, each pass scoping its concerns. The same shape as the workspace stage pattern.

**Literate programming.** Knuth's idea that documentation and code interleave so the program reads as prose. The `CLAUDE.md` / `CONTEXT.md` convention is a descendant: structured prose carrying both intent and instruction, woven into folder structure rather than code.

**Progressive disclosure.** Norman, Tognazzini, and the UI tradition: surface what the current task needs, hide the rest. The principle behind the load-pattern axis, repurposed from designing for human attention to designing against finite context windows.

**AGENTS.md / CLAUDE.md conventions.** Different vendors, same convention: a designated identity file at the workspace root, read first on entry. This paper uses `CLAUDE.md` because the author's environment is Anthropic's, but the methodology generalizes.

**Adjacent but distinct: memory systems.** Memory is agent-side state persisting across conversations; context-as-architecture is project-side structure the agent reads each session. Memory records what the agent learned; the architecture defines what it should read.

**Adjacent but distinct: prompt engineering.** A prompt shapes a single inference's input. The architecture is structural and persistent: it shapes which prompts the agent assembles for itself based on where it operates. Prompt engineering optimizes within a call; the architecture optimizes which calls happen and what they carry.

## Glossary

Paper-specific and foundational terms, alphabetical.

**AIOS.** The production architecture the author runs (AI Operating System, after Nate Herk). The worked example at scale.

**`CLAUDE.md`.** The identity file at a workspace root, read first on entry. Carries identity content and, at the workspace level, operations content (routing, load/skip tables).

**Completion field.** The fourth element this paper adds to Van Clief's contract. Defines the acceptance criteria under which a stage is *finished*, not merely *terminated*. Gives Input → Process → Output → Completion.

**`CONTEXT.md`.** A per-stage file in the separated form, carrying one stage's Input → Process → Output → Completion contract; keeps operations out of the identity file.

**Content type.** What kind of information a file holds: identity, situational context, reference, or working file. One half of the horizontal axis, independent of when it loads.

**Dimensional independence.** The two axes do not determine each other; both values are needed to locate the agent's context.

**Factory and product.** Van Clief's framing: reference material is the "factory" (stable, used to make things); working files are the "product" (created and consumed during a task).

**Fractal property (structural invariance).** The horizontal pattern repeats unchanged at every routing level, with no special cases. The agent applies the L0 pattern at new scopes rather than learning a new system.

**Horizontal axis.** Load pattern combined with content type. Decides what loads, when, and what kind of information it is. Operates at every level.

**ICM (Interpretable Context Methodology).** Van Clief and McDermott (arXiv 2603.16021): folder structure as agentic architecture, a five-layer model with numbered stages and Input → Process → Output contracts. The lineage this paper reframes.

**Identity (content).** Who the agent is at a scope: conventions, voice rules, behavioral patterns, mode declarations. Declarative and durable. Contrast operations.

**L0 / L1 / L2.** The three routing levels. L0 root (global identity), L1 workspace (workspace identity and routing), L2 task-level routing (which files to load for a task).

**Load pattern.** When a file enters the context window: always loaded (identity, situational context), on demand by task type (references), or per work item (working files). One half of the horizontal axis.

**Load/skip table.** A table in a workspace's `CLAUDE.md` mapping task types to the exact files to load and skip. The mechanism implementing L2, refined under human review.

**"Lost in the middle."** LLMs use information at the start and end of context more reliably than the middle. Larger windows do not solve it; they shift where the middle is. The constraint that makes selective loading architectural rather than optional.

**Operations (content).** What the agent does given the scope: load/skip tables, task routing, action sequences. Imperative and situational. Derived from identity; grows as task types multiply. Contrast identity.

**Reference material.** Durable cross-cutting knowledge, neither identity nor work-in-progress (style guides, methodologies, conventions). Loaded on demand. Van Clief's "factory."

**Routing hierarchy.** The vertical axis: nesting of scopes from root (L0) through workspace (L1) to task routing (L2). Determines where the agent operates and which loading rules apply.

**Situational context.** Always-relevant facts about the user or project that are not identity (a recent move, active priorities). Always loaded like identity, but changeable. Lives in `context/`.

**Stage contract.** The explicit interface for one stage of a multi-stage workflow: Input → Process → Output (Van Clief), extended here with Completion.

**Two axes.** The paper's central reframing: context selection decomposes into a vertical axis (routing hierarchy) and a horizontal axis (load pattern plus content type), dimensionally independent and structurally invariant.

**Vertical axis.** The routing hierarchy. Decides where the agent operates and what scope the loading rules apply to.

**Workspace.** A folder an agent operates in, with its own identity file, context, references, and work files. Can nest; each nested workspace is the global architecture instantiated locally.

**Working files.** The per-task output: the document, chapter, deliverable being produced. Created and consumed during execution. Van Clief's "product."

## References

Anthropic. Claude Code documentation. https://docs.anthropic.com/en/docs/claude-code

Knuth, D. E. (1984). Literate Programming. The Computer Journal, 27(2), 97-111.

Norman, D. A. (1988). The Design of Everyday Things. Basic Books.

Pocock, M. agent-skills (https://github.com/mattpocock/skills). Implementation reference for skill-level progressive disclosure.

Ritchie, D. M., and Thompson, K. (1974). The UNIX Time-Sharing System. Communications of the ACM, 17(7), 365-375.

Van Clief, J., and McDermott, D. (2026). Interpretable Context Methodology: Folder Structure as Agentic Architecture. arXiv:2603.16021. https://arxiv.org/abs/2603.16021

## Appendix: Token-cost methodology

The main text cites approximate per-layer token ranges (workspace routing ~300, stage contracts 200-500, reference material 500-2,000). These originate in Van Clief's ICM paper. This appendix documents their source, validates them against a production system, and gives a reproducible method. A token is the unit of text an LLM processes, roughly a word-fragment; counts matter because the context window is measured in tokens, and the per-layer budget determines how much of the window each part of the architecture consumes.

**Source ranges.** Reported in Van Clief and McDermott: ~300 tokens for workspace routing (L1), 200-500 for per-stage contracts (L2), 500-2,000 for reference material (L3).

**Measurement.** Representative AIOS files were tokenized with tiktoken (cl100k_base encoding). tiktoken is OpenAI's tokenizer, used as an accessible proxy for Claude's; the two differ in detail but land in the same range. The method is reproducible: install tiktoken, encode the file's text, count tokens.

| AIOS file | Layer | Measured | Van Clief range |
|---|---|---|---|
| Workspace router (consulting `CLAUDE.md`) | L1 | 431 | ~300 |
| Stage contract (discovery `CONTEXT.md`) | L2 | 331 | 200-500 |
| Focused reference (`voice.md`) | L3 | 734 | 500-2,000 |
| Comprehensive reference (consulting SOP) | L3 | 3,822 | 500-2,000 |

**Interpretation.** Stage contracts and focused references track Van Clief's ranges directly. The workspace router runs heavier than ~300 (431 measured) because AIOS routers carry both a load/skip table and a stage-status table. The clearest divergence is the comprehensive reference: a full SOP runs ~3,800 tokens, nearly double the L3 upper bound. The difference is one of kind; Van Clief's L3 examples are focused references, whereas a complete SOP is comprehensive. So the upper bound on reference material is not fixed; it scales with comprehensiveness, and comprehensive references are candidates for splitting or for loading only the relevant section on demand.

**Why it matters.** Per-layer token counts determine how much of a finite window each part of the architecture consumes on a given task. A workspace that loads a 3,800-token SOP on every task, when only a 300-token section is relevant, spends its budget poorly and is more exposed to lost-in-the-middle. Measuring actual token cost is how you know whether the architecture loads efficiently or quietly wastes the window.