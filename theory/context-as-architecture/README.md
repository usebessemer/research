# Context as architecture: an overview

Folder structure is not file organization; it is workflow architecture. Where information lives determines when and how an agent uses it.

## What this is

This is an accessible overview of the paper "Context as architecture." It is a deliberately lighter front door. It conveys the central claim and the shape of the argument without the full build. The complete paper assembles the architecture in five stages, walks through the loading sequences in detail, and validates the numbers in a measurement appendix; it is linked at the bottom. This overview is reductive on purpose, and it tries hard not to overclaim relative to what the full paper actually demonstrates.

## The central claim

When you give an agent a workspace, the natural instinct is to think of folders as tidy storage: a place to put things so they are not all in one pile. The paper argues that this undersells what folders are doing. In an agentic workflow, the place a file lives is not just where it is kept; it is a statement about when the agent should read it and what role it plays once read. Put the same sentence in three different files and you have made three different decisions about how often it loads, in what company, and for what kind of task. Structure is therefore not cosmetic. It is the thing that decides what the agent sees at the moment it acts, and what it sees is what shapes the work.

That decision matters because context windows are finite and uneven. Models do not use a long context evenly; they tend to rely on what sits at the start and the end and to under-weight the middle, a pattern the literature calls "lost in the middle." Bigger windows do not remove the problem; they move where the middle is. So loading everything is not a safe default. Choosing what to load, and what to leave out, is not an optimization you add later. It is architecture, and the folder structure is where that architecture lives.

## Two independent axes

The core of the paper is a claim about shape. The architecture has two axes, and they are independent of each other.

The first is vertical: a routing hierarchy with three levels. L0 is the root, the global identity and the routing for everything beneath it. L1 is a workspace, a folder the agent operates inside that has its own identity and its own routing. L2 is task-level routing, the rule that says which specific files to load for a given task and which to skip. Moving down the hierarchy narrows the scope. Each level decides where the agent is and which loading rules apply there.

The second axis is horizontal, and it combines two things the paper is careful to keep distinct: a load pattern and a content type. The load pattern is about timing, about when a file enters the context window. Some files are always loaded. Some load on demand when the task calls for them. Some load only for the specific work item in front of the agent. The content type is about kind: whether a file holds identity (who the agent is here, its conventions and voice and behavior), situational context (always-relevant facts that are not identity, like a recent change or a current priority), reference material (durable cross-cutting knowledge such as a style guide or a methodology), or working files (the actual output being produced).

The reason these are two axes and not one is that timing and kind do not determine each other. Identity is always loaded, and so is situational context, but they are different kinds of content; one is durable and one changes. Knowing when a file loads does not tell you what kind of information it is, and knowing the kind does not tell you when it loads. The paper calls this dimensional independence: you need both values to locate a file in the architecture. The horizontal axis answers what loads, when, and what kind it is. The vertical axis answers where the agent is operating and which rules apply. Splitting the question this way is the move; most working setups collapse both into a single flat dimension and lean on the model to sort it out, and the two-axes framing makes explicit what those setups leave implicit.

## Why it is fractal

Here is the property that makes the whole thing hold together. The horizontal pattern does not change as you move down the vertical hierarchy. The same four content types and the same load patterns appear at L0, and again inside an L1 workspace, and again at the task level. A workspace nested inside another workspace is not a new system to learn; it is the same pattern instantiated at a new depth. The paper calls this structural invariance, and it is what makes the architecture fractal.

The practical payoff is that complexity does not require new machinery. When a new kind of work appears, you do not invent a new structure for it. You reuse the existing pattern at a new scope. A project that has outgrown being a single file becomes a workspace with its own identity, its own context, its own references, and its own work files, which is precisely the global structure expressed locally. New task types are new rows in a routing table, not new folders. Because the pattern repeats without special cases, the paper argues that no level of complexity is unreachable, provided the ordering is kept strict. That is a strong-sounding claim, so it is worth being precise about its scope: it is a claim about composability within this class of workflow, not a claim that the architecture suits every workflow. The paper is explicit that it does not.

## What it builds on

The paper does not present this as invented from nothing. It positions itself as an extension of ICM, the Interpretable Context Methodology of Van Clief and McDermott, which framed folder structure as agentic architecture through a five-layer model with numbered stages and Input to Process to Output contracts. The contribution here is a reframing: those five layers are more clearly understood as the two independent axes described above, a routing hierarchy combined with a load pattern and content type. The methodology stays ICM-shaped; what changes is the structural lens through which the layers are read.

The paper also adds one concrete piece to ICM's stage contract. Where ICM's contract runs Input to Process to Output, this paper adds a fourth field, Completion, which makes explicit the acceptance criteria under which a stage is genuinely finished rather than merely stopped. The distinction it draws is between a stage that terminates, where the agent simply halts, and one that completes, where the work is actually done and verifiable. The paper argues this implicit gap is where agentic workflows tend to break, an agent treating output-written as work-finished when the next stage or the human disagrees.

## Demonstrated at scale

The toy example in the full paper is the architecture at minimum size. To show it holds up under weight, the paper points to a production system the author runs, called AIOS (an AI Operating System, a framing drawn from Nate Herk's work), which spans consulting, software, training, and daily life. AIOS instantiates the same patterns at scale: global identity and always-loaded context at L0, many active workspaces at L1, and separated per-stage contracts at L2. The paper's claim about scale is specific and worth keeping intact. What changes between the toy workspace and the production system is content density, not architecture. AIOS holds hundreds of files where the toy example holds a handful, but the categories, the routing, and the loading rules are the same. The full paper backs the token-cost discussion with measurements from real AIOS files; that detail lives in the appendix and is not reproduced here.

## Where it does not apply

A faithful overview has to carry the paper's hedges, not just its claims. ICM, and this architecture with it, is built for sequential, human-reviewed workflows with periodic agent handoff. The paper states plainly that it does not fit real-time multi-agent collaboration with tight message loops, high-concurrency multi-user systems, or pipelines that branch automatically on AI decisions without a human review gate. It also names an open problem it does not solve: traceability. When an output reflects one rule rather than another, the architecture gives no easy way to trace which scope's rule drove it. The scoping is clear; the debuggability is not, and the paper flags this rather than papering over it.

## Read the full paper

This overview omits the stage-by-stage build, the worked loading sequences, the full treatment of identity versus operations, the failure modes, and the measurement methodology. For all of that, including the five-stage construction, the loading-sequence walkthroughs, and the token-cost appendix, read [main.md](main.md).
