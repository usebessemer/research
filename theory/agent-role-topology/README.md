# The agent-role topology: an overview

When you build a system of AI agents that work together, you quickly need to answer a deceptively simple question: what kinds of agents are there, and how do they relate? "One lead plus a bunch of workers" is a fine sketch for a human to read, but it is too vague for a tool to check. This overview walks through the answer the paper gives, in plain language.

## What this is

This is a front door, not the building. It is a lighter, intuition-first tour of the paper "The agent-role topology." The full paper carries the parts this overview deliberately leaves out: the precise type system, the diagram that shows how the pieces fit, the step-by-step derivation, and a glossary of load-bearing terms. Everything here is meant to be true to that paper and never to claim more than it does. Where the paper hedges, this overview keeps the hedge. The full paper is linked at the bottom.

## The core idea: two axes, not one ladder

The natural way to organize a set of agents is a single ladder of "kinds," where each kind is a more specific version of the one above it. The paper argues that this is the wrong shape. Instead, an agent's role is fixed by answering two separate questions, and neither question answers the other.

The first question is about **Position**. Does this agent hold a *board* and hand work out, or does it produce a work product itself? The paper calls the first kind a **Lead** and the second an **Executor**. A board, in this setting, is the agent's running view of its area of responsibility: the set of files it reads to keep track of what is going on. A Lead holds that view, delegates the actual work downward, reviews what comes back, and passes important decisions up to the human. An Executor does none of that. It takes a spec, builds the thing the spec describes, and opens it up to be reviewed. It holds no board and has nobody working under it.

The second question is about **Lifecycle**. Is this agent **Standing** or **Ephemeral**? A Standing agent is set up once against a durable charter and then sticks around. It wakes on a schedule, does its job against state that survives from one waking to the next, and goes back to sleep rather than shutting down for good. An Ephemeral agent is the opposite. It is spun up for a single task, does that task, opens its result for review, and then terminates. The next task gets a fresh instance.

The important move is that these two questions are *independent*. Position does not tell you Lifecycle, and Lifecycle does not tell you Position. You can ask either one of any agent. That independence is what makes them two axes rather than one ladder, and it is the correction at the heart of the paper: an earlier version of this work treated these distinctions as a single inheritance chain, and that chain could not express a property that applies across the whole structure but is fixed on one side of it. Two axes can.

## The 2x2 grid

Two yes-or-no axes give you a two-by-two grid with four cells:

- **(Lead, Standing)** holds every Lead in the system.
- **(Lead, Ephemeral)** is empty, and the paper argues it must be.
- **(Executor, Ephemeral)** holds the build-it-once-and-finish agents.
- **(Executor, Standing)** holds the keep-it-current-on-a-schedule agents.

Three of the four cells are inhabited. The fourth is where the structure gets interesting.

## Why one cell is empty

The paper claims the **(Lead, Ephemeral)** cell cannot be filled, and it presents this as an argument from the meaning of the terms rather than as an accident of the current design. The intuition is short. A Lead's whole job is to hold a board, and a board is a view that persists. Subordinates write to it, and the Lead reads across it over many wakings. That requires the Lead to stick around. An Ephemeral agent, by definition, does not stick around; it does its one task and terminates, taking its state with it. So an Ephemeral Lead would be an agent asked to hold a board it cannot keep. The moment it terminated, the board would have no holder and the agents reporting to it would have nowhere to report. There is no coherent role to put in that cell.

The paper frames this as "empty by argument," or in its own stronger wording, by theorem, and it is worth keeping that hedge in view. The claim is that the emptiness follows from what "board" means, not that someone simply chose not to build such a role. A reader who wants to push on the argument should read the full derivation rather than take the word "theorem" as the end of the discussion.

The consequence is tidy. Because the Lead side can only ever be Standing, Lifecycle is described as a "whole-tree" axis that is *pinned* on the Lead side and runs free only among Executors. You can still ask a Lead whether it is Standing or Ephemeral; it is just that the answer is forced. Among Executors, both answers are genuinely live: an Executor that builds one thing and stops, and an Executor that runs a standing routine on a schedule, are both perfectly coherent.

## The Principal is the root, and is not a Role

Above the grid sits the human. The paper calls this the **Principal**, and it makes a careful point: the Principal is the root that every Lead ultimately reports to, but the Principal is *not itself a Role*. It does not sit in any cell of the grid. It holds no board and authors no work product; it is the authority that all the Roles answer to, not one of the things being managed.

The reasoning is that the entire structure exists to serve the Principal. Decisions travel up the chain of Leads, getting distilled at each step, until they reach the human as a small set of clean calls. Direction then flows back down. Treating the Principal as just another Role would be a category error, because you cannot make the thing the whole machine feeds into one of the items on the menu. The Principal is where decision authority lives; every Role, by contrast, has only bounded authority to *execute*, never to *decide*. That split is one of the load-bearing ideas in the full paper.

## Concrete agents are domain bindings of the cells

The four cells are abstract. They describe shapes, not running agents. A real agent comes from taking a cell and binding it to a domain: a specific job, with specific inputs, skills, and a specific review surface. The paper names a handful of these concrete classes as examples, and it is careful about which are shipping and which are planned.

- **CodeExecutor** is an (Executor, Ephemeral) agent. It takes a coding task with acceptance criteria, writes and tests the code, and opens a pull request for review, then terminates when the work is merged.
- **ContentExecutor** is a planned (Executor, Ephemeral) agent. It does the same shape of work for writing rather than code: it drafts a document or memo against a brief and opens the draft for sign-off.
- **Bookkeeper** is an (Executor, Standing) agent. It keeps a small business's books current and produces a periodic package for an accountant. It wakes on a schedule, handles the routine cases on its own, and pushes anything uncertain into a review queue rather than guessing.
- **LegalAdmin** is a planned (Executor, Standing) agent. It tracks legal-administrative obligations and deadlines for a law firm, surfacing what is coming due.

The pattern to notice is that these are the *only* true classes in the model. Position, Lifecycle, the level a Lead operates at, and a Lead's working mode are all described as axes, properties, or traits rather than as separate classes. The paper's stated bias is to prefer a property or a trait over a new subclass unless the interface genuinely changes, because a subclass is a permanent commitment that every tool downstream has to carry. So the Lead cell does not split into separate "L0" and "stream lead" classes; those are just the same Lead at different scopes. The genuine classes live in the two Executor cells, and even there the paper leaves explicit room for more to be added later as new domains come up.

## What the structure is for

The reason any of this is spelled out so precisely is that it feeds a tool. The paper presents this grid as the type system that a forthcoming tooling layer will instantiate when it scaffolds a new agent workspace and validate when it audits one. A loose taxonomy is fine for a human reading a manual; a tool that has to *check* the structure needs to know exactly what a role is, which members every role must have, and which apparent kinds are really just one axis set to a different value.

The paper is also honest about the limits of that checking, and the honesty is worth carrying into this overview. A file-and-folder linter can confirm that a workspace is *shaped* correctly: that a Lead has a board, that an Executor has the right kind of review surface stubbed for its lifecycle, that a role sits in an inhabited cell and binds exactly one value on each axis. What such a linter cannot do is confirm what the running agent actually *did*: whether it really escalated an ambiguous case instead of quietly guessing, or whether a surfaced decision was framed well. Those are runtime facts, enforced by the review surface and by a safety default that keeps an agent inert until it is properly configured, not by the linter. The paper draws that line deliberately so the tool is not oversold, and this overview keeps the line where the paper put it.

## A note on the bigger claim

The paper closes with a forward-looking conjecture, and it is explicitly flagged as a conjecture rather than a result. The idea is that this two-axis shape, a "where" axis and a "when" axis, may not be specific to agent roles at all. A companion paper organizes *files* with the same two axes, and the suggestion is that any layer built against a model's finite attention may be forced into the same two levers: route a thing by scope so it is present only where it is relevant, and schedule a thing by time so it is present only when it is relevant. The paper offers this as a direction for future work, not a proven claim, and that framing should travel with the idea wherever it goes.

## Read the full paper

This has been the intuition only. For the precise type system, the shared interface every Role implements, the diagram, the full argument for the empty cell, the concrete class bindings, and the glossary, read [main.md](main.md).
