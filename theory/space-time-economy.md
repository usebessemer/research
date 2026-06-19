# The space-by-time economy of the context window

## TL;DR

A model has a bounded context window: a finite set of slots, filled for one call, attended all at once. Everything you place there, files, agents, tools, memories, connections, skills, competes for that finite attention.

One universal claim, one precise condition.

**Universal:** anything placed before a finite, all-at-once, per-call window is economized by deciding **where** it is present (scope) and **when**, and for how long, it is present (time). Space and time, every time.

**Condition:** those two levers split into **two independent peer axes**, a clean grid with structural theorems like the empty cell, only when the element carries an intrinsic second dimension for the time-lever to bind to. Files carry **content-type**; agents carry **lifecycle**. With one, time varies freely while scope holds fixed, so the two are genuine peers. Without one, the time-lever does not vanish but collapses into scope: deciding when becomes the same act as deciding where, and no grid forms.

Three populations show this. Files factor cleanly because they carry content-type (this corrects v1, which wrongly waved content-type away). Agents factor cleanly because they carry lifecycle, with one honest asymmetry: the routing hierarchy is genuinely shared, but the agent's first axis, Position, is scope-*like*, a functional role, not a literal partition of situation-space. Tools are the worked **bounding case**: they obey scope cleanly but carry no second dimension, so their time-lever collapses into scope and no empty-cell theorem reproduces. They sit at "one-and-a-half axes," and they prove the rule the way the topology's empty cell does.

Honest status, up front: the universal economy and its exhaustiveness are **argued** (a framework), not proven, with the soft joint in the exhaustiveness argument named openly. The generalization to all future populations is a **conjecture**, offered for refutation, with falsifiers sharp enough that they cannot be redescribed away.

## Why a root

Two papers in this repository reach for the same shape from different directions. *Context as Architecture* resolves "put files where they belong" into independent axes with a classification function, which is what makes a file tree machine-checkable. *The agent-role topology* resolves "one lead plus leaves" into orthogonal axes with a grid, which is what makes a role structure machine-checkable. Both notice scope and time as the organizing pair. The topology paper names the coincidence and flags it as an open conjecture: a "space-by-time economy" forced by the bounded window, predicted to recur in any layer built against a finite window, with "a fuller treatment left to a future foundational piece."

This is that piece. It sits underneath both companions, states the principle they each instance, and argues it rather than asserting it.

But it also does something the conjecture did not anticipate, and that is the more interesting result. An adversarial pass on the strong form, that *every* population factors cleanly into two peer axes, broke it. Tools do not factor that way; looking harder at files and agents showed why. The clean two-axis form is not guaranteed by the window; it is purchased by a property of the population. So the refined thesis is sharper than the conjecture, not weaker. The two existing papers become the first two corollaries; tools become the boundary that locates the condition.

The discipline is the companions': take a thing everyone half-knows and resolve it until a checker, or a critic, could disagree precisely. The half-known thing is "context is scarce, so be selective."

## The principle

Start with what a context window *is*. Two properties matter, and only these two.

1. **It is bounded.** The window holds a finite number of tokens. Past the bound, content is not admitted, and even admitted content is attended to unevenly (the "lost in the middle" finding), so the *effective* budget is tighter than the nominal one. There is a ceiling on what can be present at once, and a softer ceiling below it on what can be present *and well-attended*.

2. **It is filled at a moment and attended all at once.** The window is a set of slots assembled for one call, all attended simultaneously. There is no "later in the call" where something unloaded becomes available; the window for this call is fixed when the call is made, and the next call is a fresh fill.

Together these give a finite resource re-provisioned per call: at each cycle you have fixed capacity and must decide what occupies it *this* time. Cache lines, a working set of memory pages, a register file, a single screen of a progressively-disclosed UI, each is a fixed-capacity slot-set refilled per cycle, where the only question is what is present now. The window is one more member of that family, whose economics are well understood.

The economic claim, plainly: **for a finite resource provisioned at a moment and attended all at once, economizing it means deciding, for each candidate element, whether it occupies a slot in this provisioning.** That decision has two dimensions.

- **Where (scope).** Of all the contexts the system can be in, in which is this element relevant enough to occupy a slot? Scope *partitions* the space of situations and admits the element only in its partition.

- **When (time).** Given the situations where the element is in-scope, at which moments, and for how long, should it actually occupy a slot? Time schedules presence: *always* (a slot in every provisioning while in scope), *on demand* (only when a trigger fires), or *per item* (one work-item's duration, then yielded).

Scope decides the set of situations an element may appear in; time decides when, and for how long, within those situations, it actually does. The first is a partition of space, the second a schedule over time. Every act of economizing the window is some setting of these two: present this element *here* and *then, for that long*. **That is the universal part, and it holds for every population this paper examines, including the bounding one.** What does not hold universally is that the two are always cleanly separable into peer axes. That is the refined thesis, and the next section is its spine.

## The two levers, and when they are two peer axes

The principle would be empty if "where and when" were just a capacious phrase. The objection is fair: *of course* any organizing decision can be described as being about where and when, the way any event has a location and a time. That framing absorbs everything and explains nothing. If that were all, it would be a tautology in costume.

The claim is stronger, and the strength lives in two properties the levers must *earn*: **independence** and **exhaustiveness**. A tautology has neither. And the refined thesis is that independence is *not automatic*. The economy gives you two levers; whether they are two *peer axes* you can set separately depends on whether the element has an intrinsic second dimension for the time-lever to bind to.

### Independence is earned, and an enabling second dimension is how

Two axes are independent if fixing one leaves the other free. For scope and time, you must be able to vary an element's *when* while holding its *where* fixed, and vice versa.

Files demonstrate both directions. Hold scope fixed: take everything in-scope for one client engagement. Within that fixed scope they still differ in time. The engagement's identity file is *always* present; its reference SOP is present *on demand* when a writing task calls; the specific stage file is present *per item* for one stage, then yields. Same scope, three time settings. Now hold time fixed: take every file *always loaded* while in scope. A global identity statement is always-loaded *everywhere*; a workspace-specific fact is always-loaded *only within that workspace*. Same time setting, two scopes.

Now notice *what made that possible*, because it is the load-bearing observation of the paper. A file's time setting could vary independently of scope because a file has a property *other than* its scope and its load-timing that the load-timing tracks: **content-type**. An identity file is always-loaded because it is *identity*; a reference is on-demand because it is *reference*; a working file is per-item because it is a *work product*. The load pattern (time) is keyed to content-type, and content-type is orthogonal to scope. So time runs free of scope because there is a third intrinsic property for the time-lever to ride. Remove it, force the time setting to be readable only off "where the element is and whether this is the call that needs it," and the time decision and the scope decision become one. That is exactly what happens to tools.

A sharper way to see independence when it holds: scope is a question about the *partition of situations*; time is a question about the *schedule within a situation*. These are different mathematical objects, over different domains (space of situations versus line of moments). You cannot in general derive one from the other. When a population carries a second dimension keying the schedule, the two are genuinely separable and independence is structural. When it does not, the schedule can key only on the situation itself, and degenerates into a re-description of the partition. **Independence is structural when earned and absent when not; the enabling dimension is what earns it.**

### Exhaustiveness, argued not proven, with the soft joint named

Exhaustiveness is the other half: any economization is a setting of scope and time, with no residue. There is no third *kind of thing* you can do to a finite, all-at-once, per-call resource that is not reducible to where it is present and when, for how long.

The argument is by elimination. What can you actually do to economize a slot-set filled at a moment and attended all at once? Only control which elements occupy slots in a given provisioning, because that is the only thing the window exposes. A provisioning is identified by the **situation** the system is in (which fixes the *set* of candidate elements) and the **moment** the call happens (which fixes *which* in-scope candidates are triggered, and for how long). "Which provisionings does this element occupy?" therefore decomposes into "which situations?" (scope) and "at which moments within them, for how long?" (time). Priority, relevance score, recency, frequency, cost, everything one might reach for as a third axis, are not coordinates; they are *inputs* to the scope and time decisions. A relevance score is how you decide a thing's scope or trigger its time, not a place it can be. Compression and summarization change *how much* of an element occupies its slot, not whether-and-when; they operate *within* a time setting, not beside it.

Here is where v1 overclaimed and this version is honest. v1 stated this elimination with theorem-like confidence. The elimination's strength is exactly the strength of one premise: **a provisioning is a (situation, moment) pair and nothing else.** That premise has a sink in it. "Situation" is elastic. Almost any candidate third coordinate, the user's role this call, the channel it arrived on, the trust level of the requester, the budget remaining, can be absorbed into "the situation the system is in" and re-described as scope. So the elimination does not so much *prove* there is no third axis as *define* the situation broadly enough that nothing escapes. A good argument, not a theorem, because its key term is load-bearing by being elastic.

The honest move is to name the joint and offer a criterion, imperfect, to tell a genuine third coordinate from a re-description. **Criterion (offered, not proven decisive): a candidate property is a coordinate of a provisioning if slot-occupancy must condition on it *and* it cannot be folded into the partition of situations without making the partition track the element's own time-varying state.** Plainly: a stable feature of the world the call happens in is part of the situation, so it is scope; a feature of the element's *own changing condition* that the schedule reads is time; something genuinely neither would be a real third axis, and the framework predicts you will not find one for a bounded, per-call, all-at-once window. The criterion has a fuzzy edge precisely where "situation" is elastic, so we flag it as a soft joint. **Exhaustiveness is therefore argued, not proven, and wears that status as visibly as the generalization conjecture does.** A reader who exhibits a property the criterion cannot file as scope, time, or input has found the joint's give. That is a falsifier, not a footnote.

What survives strongly is the *minimum*. A single axis cannot economize the window for any population whose where and when come apart: a one-axis scheme must either fix scope and vary only time (and so cannot express "this element belongs only in some situations") or fix time and vary only scope (and so cannot express "this in-scope element should be present only on demand"). Both failures have names in the file paper: *monolithic-context* (everything always-present, time collapsed) and *hidden-context* (a notional scope that no time setting ever loads). So **two is the minimum that economizes any population whose levers separate, and, by the argued elimination, the maximum that has a coordinate to economize.** The clean *count of two peer axes* is conditional, which is the next section.

## The condition for the clean two-axis form

The where/when economy is **universal**: every element before a finite, all-at-once, per-call window is economized by where and when. That never fails. What is **conditional** is whether the economy *factors* into two independent peer axes, a clean grid read one coordinate at a time, with structural theorems (the empty cell) only a genuine grid can host.

**The condition: the economy factors into two independent peer axes exactly when the element carries an intrinsic second dimension, orthogonal to scope, for the time-lever to bind to.** Files carry content-type; agents carry lifecycle. With one, time is keyed to a property other than scope, so it varies freely at fixed scope and the two are peers. Without one, the time-lever keys only on the situation, so "when is this present" collapses into "in which situations," and you have one axis (scope) with a time-flavored gloss. That is "one-and-a-half axes," and tools are the worked instance.

Two definitions must be kept rigorously apart, because conflating them is exactly v1's error.

- An **enabling second dimension** is an intrinsic property of the element, orthogonal to scope, that the *time*-lever reads in order to vary independently of scope. Content-type for files; lifecycle for agents. It is *not* an economizing lever; it does not decide slot-occupancy. It is the thing the time-lever attaches to so the time decision is not forced to be the scope decision. You can have it and still have exactly two economizing axes.

- A **third economizing lever** would be a genuine third coordinate of the provisioning, a further independent decision about slot-occupancy beyond where and when. The framework predicts there is none for the window-as-characterized (argued, soft joint above). An enabling dimension is emphatically *not* one of these; it is a feature of the element, not a coordinate of the call.

An enabling second dimension is what *lets the two axes be two*; a third economizing lever would be what *makes the two axes three*. Files have the former (content-type) and not the latter, which is why they get a clean two-axis grid and not a three-axis one. This distinction is the keystone.

## Files: they factor cleanly *because* they carry content-type

*Context as Architecture* economizes the population of **files**, resolving file-positioning into two independent axes that together decompose the full "what should this agent see?" question.

- **Scope axis = the routing hierarchy (L0 / L1 / L2).** The *where*. An L0 root file is in-scope everywhere (global identity); an L1 workspace file only within that workspace; an L2 task rule only for a matched task. The companion's "vertical axis: determines where the agent operates and what scope the loading rules apply to," named for files.

- **Time axis = the load pattern (always / on-demand / per-item).** The *when*. Identity and situational context are always loaded; references load on demand by task type; working files load per work-item. The companion's "horizontal axis: decides what the agent loads, when," named for files.

Now the correction, the keystone reframe of the paper, where v1 got it backwards.

The companion does not call its horizontal axis "load pattern." It calls it **"load pattern combined with content type,"** and is emphatic that content-type is its own independent thing: *"When something gets loaded and what kind of information it is are independent questions; the architecture answers both."* The four content-types, identity, situational context, reference material, working files, are a real classification orthogonal to scope and to load-timing. Downstream, the ICM SPEC's file linter runs `classify(file_path, tree, claude_md) -> {routing_level, content_type, load_pattern}`, **three** independent, checkable, violable attributes. Content-type is not a stray label; it is a first-class attribute a tool *checks* and a file can *violate*.

**v1 waved this away as "a descriptive tag riding the time axis,"** and treated the file population's third checkable attribute as an embarrassment, a "smoking gun" against "exactly two economizing axes." That was wrong, and wrong against the paper's own source. The refined thesis flips it: **content-type is not a problem for the two-axis claim; it is the *reason* the claim holds for files.** Content-type is the file population's **enabling second dimension**. The load pattern is keyed to it (always because *identity*; on-demand because *reference*; per-item because *work product*), and content-type is not scope. That is exactly the independence the previous section demanded: a file's *when* can move while its *where* stays pinned, because the *when* tracks content-type. The fractal property the companion observes, the load pattern repeating unchanged at every routing level, is just the time axis being free at every value of scope, possible only because the property keying it is orthogonal to scope.

So the rigorous statement: **files have *three* independent checkable attributes (routing-level, content-type, load-pattern) but *two* economizing axes (scope = routing-level; time = load-pattern), and the third attribute is the enabling dimension that holds the two apart.** Content-type does not decide slot-occupancy; routing-level and load-pattern do. It is checkable and violable (a file mis-typed as identity when it is really a work product is a real, lintable error) because it is a real attribute, and it is *not* a third economizing lever because slot-occupancy still factors into where and when. v1's "smoking gun" is, read correctly, the confirming evidence: files get a clean grid *because* they carry content-type, exactly parallel to agents carrying lifecycle.

## Agents: they factor cleanly, with an honest asymmetry

*The agent-role topology* economizes the population of **agents**, resolving role-positioning into two orthogonal axes and a grid. The mapping is real and the time axis is clean, but v1 oversold it ("literally identical / shared"). The honest account has an asymmetry, and the asymmetry is itself evidence for the thesis.

- **Time axis = lifecycle (standing vs ephemeral).** The *when*, and the **cleaner cross-population invariant**, the part that ports with no fudging. A *standing* role persists, wakes on a schedule, survives between runs (the role analogue of always/on-cadence loading); an *ephemeral* role is invoked per task, authors its product, terminates (the analogue of per-item loading). Lifecycle is the agent population's **enabling second dimension**: a property orthogonal to position-in-the-structure that the time-lever reads so *when* a role is present varies independently of *where* in the authority structure it sits. That is content-type's job, played by lifecycle, and it is *why* agents, like files, get a clean grid.

- **First axis = Position (Lead vs Executor), which is scope-*like*, not literal scope.** Here is the asymmetry v1 hid. A role's Position, hold a board and delegate or author a product, is a *functional role*: it says *where in the authority structure* the role operates. That is scope-*like*. But it is **not a literal partition of situation-space** the way a file's routing level is. L0/L1/L2 literally partitions the *situations the model can be in*; Position partitions *authority roles*, not situations. It is analogous to scope and functions as the grid's first axis, but "the same axis as file scope" overstates, and v1 did.

What *is* genuinely shared is **the routing hierarchy itself, as the level property.** A Lead carries a `level` (L0 / stream / engagement) running along the *same routing hierarchy* the file scope axis uses; the topology paper says so: *"Context as Architecture made L0/L1/L2 a property of the vertical routing axis, not three kinds of file, and the role layer mirrors that exactly. One routing hierarchy, read once for files and once for roles."* So the shared object is the routing hierarchy read as a *level property*, carried by Position, not the whole first axis.

The downgrade, cleanly: the agent population is **analogous** to the file population, not identical. The routing hierarchy is shared as a *property* (level). The time axis (lifecycle) ports onto the file load pattern with no asymmetry. The first axis is scope-*like* rather than literal scope. This honesty is not a retreat; it is evidence. Both populations get a clean grid, and in both the *reason* is an intrinsic second dimension keying the time-lever. The time axis ports perfectly; the scope axis ports as analogy. That the enabling-dimension story holds identically while the scope story is only analogous is exactly what the thesis predicts: the clean form is purchased by the second dimension, and that purchase is the invariant.

### The empty cell, predicted

The mapping predicts a feature the topology paper had to discover separately: its **empty cell**. The (Lead, Ephemeral) cell is provably empty because "holding a board requires persistence." In this root's terms: a Lead's board is a *standing* element by construction, so its time setting is pinned. A Lead's role forces its lifecycle (standing), leaving the ephemeral lifecycle uninhabitable for that role. This is a *constraint between two genuinely independent axes*, not a collapse: lifecycle still runs free among Executors, so the axis is real, merely pinned at one first-axis value. **An empty-cell theorem is a thing only a real two-axis grid can have**: you need two independent coordinates for one combination to be provably uninhabitable. That is exactly why tools, lacking two independent axes, cannot reproduce it. The root predicts that wherever a population has an enabling second dimension *and* one first-axis value structurally demands a particular time setting, the opposite time-value is an empty cell. The topology's theorem is a corollary.

### Two populations, side by side

| | First axis (where / where-like) | Time axis (when) | Enabling 2nd dimension (keys the time axis) |
|---|---|---|---|
| **Files** (*Context as Architecture*) | routing hierarchy: L0 / L1 / L2 (literal scope) | load pattern: always / on-demand / per-item | **content-type**: identity / situational / reference / working |
| **Agents** (*agent-role topology*) | Position: Lead / Executor (scope-*like*; carries the shared level property) | lifecycle: standing / ephemeral | **lifecycle** *is* the dimension that keys the time axis |

For agents, lifecycle plays *both* the time-axis role and the enabling-dimension role, because the property the time-lever reads (standing-vs-ephemeral) is the very property that *is* the schedule. For files the two are distinct (content-type keys the load pattern but is not it). A small structural difference, not a problem: in both, the time axis is free of the first axis *because* an intrinsic orthogonal property is available to key it. One economy, one condition met twice.

## Tools: the worked bounding case (one-and-a-half axes)

The strongest test of the thesis is not a third clean confirmation; it is a population that meets the *universal* economy but *fails the condition*. Tools are that population, and working them through shows the condition is real, not decorative. This stays general and illustrative.

A tool, a function-definition placed before the model (name, description, parameter schema, examples), occupies window slots and competes for attention exactly as files and agents do. So it is economized, and the universal economy applies in full.

**Tools obey the SCOPE economy cleanly.** Some tools are always-present with their full schema (the handful needed in nearly every situation). Some are present *by name only*, schema deferred until a fetch-or-search step pulls the full definition (the long tail: present enough that the agent knows the tool exists, absent enough that schemas do not flood the window). Some are workspace-scoped: present only in the repos, projects, or channels where the tool is wired. That is scope exactly, and the dominant economization for tools: with hundreds of tools and a finite window, which schemas are present is the whole game.

**But the TIME-lever collapses into scope, because a tool carries no intrinsic second dimension to hold them apart.** Consider deferring a tool's schema until a search step surfaces it. Is that a *when* decision or a *where* decision? Both, inseparably. Deferring the schema *is* the act of scoping it out of the current provisioning into a later, narrower one. "Load this schema only when a step calls for it" and "this schema is in-scope only in the situations where a step calls for it" are *the same decision stated twice*. There is no property of the tool, analogous to content-type or lifecycle, that the time-lever could read to make "when is this schema present" vary independently of "in which situations." A file's load pattern keys on content-type; an agent's presence keys on lifecycle; a tool's schema-presence keys on nothing but the situation and whether this is the call that needs it. So the *when* has nothing to bind to except the *where*, and the two levers, genuinely two for files and agents, are one-and-a-half for tools: a full scope axis, plus a time-lever that exists (you really can defer a schema) but cannot stand free of scope.

**No empty-cell theorem reproduces, and that is the diagnostic.** Try to build the grid: one axis is scope (workspace-scoped / global), the other would be time (always-present / deferred), but the second is not independent of the first, deferring *is* a scoping, so the "grid" is one coordinate drawn twice. With no second free coordinate, no combination of two coordinates can be proved uninhabitable, so no empty-cell theorem exists for tools. The *absence* of the theorem is not a gap in our analysis; it is the positive signature of a population failing the condition. A clean grid hosts theorems; a one-and-a-half-axis economy cannot.

Like the topology's empty cell, the boundary case proves the rule. The empty cell proved lifecycle is genuinely independent of position (only independent axes have provably-empty combinations); tools prove the converse, that *without* a second dimension the time-lever collapses into scope and no grid forms. Files and agents *confirm* the condition by meeting it; tools *bound* it by failing it. The thesis needed exactly this third population to be more than an observation about two papers that happened to rhyme. And a tool *could* be given an enabling second dimension by a future scheme: if tools acquired an intrinsic "kind" keying a genuine schedule independent of scope, the way content-type keys load pattern, the thesis predicts they would *then* factor into two peer axes and could host an empty-cell-style theorem. That conditional is itself a falsifiable prediction.

## The prediction, and what would falsify it

A framework that only re-describes the known earns little. The payoff of stating the principle *with a condition* is that it predicts, and a prediction is worth only the conditions that would kill it. The refined thesis predicts not just *that* a population is economized by where and when (the universal part, hard to falsify because hard to escape) but *whether* it factors into two clean peer axes (the conditional part, sharply falsifiable).

### The prediction

**Universal:** any population before a finite, all-at-once, per-call window is economized by scope and time. **Conditional and sharper:** a population factors into two independent peer axes, with a clean grid and possible empty-cell theorems, **if and only if** it carries an intrinsic second dimension, orthogonal to scope, for the time-lever to bind to. Files and agents have one and factor; tools lack one and do not. Forward predictions for the next populations come in two parts each, a where/when claim and a *does-it-have-an-enabling-dimension* claim.

- **Memories.** A memory entry occupies a slot when recalled. Universal: scope (which situations is it relevant to) by time (always-surfaced identity-like memory vs on-demand recall vs single-task scratch). Conditional: memories *do* appear to carry a second dimension, a memory-*kind* (semantic / episodic / procedural, or identity-like vs scratch) orthogonal to which situation recalls it, so they should factor cleanly like files and could host an empty-cell theorem (a kind that must be always-surfaced pins its time value).

- **Connections.** An integration's surface (actions, auth state, capabilities) occupies slots when present. Universal: scope (which workspaces is it wired into) by time (persistently mounted vs activated-on-demand). Conditional: the population to watch, because a connection may be *tool-like*, its "activate on demand" collapsing into "scope it to the situation that needs it," in which case connections join tools at one-and-a-half axes. Genuinely uncertain, which is what makes it worth testing.

- **Skills.** A skill's instructions occupy slots when loaded. Universal: scope (which situations it applies to) by time (the progressive-disclosure load pattern, present only when triggered). Conditional: skill-level progressive disclosure is *already* a time-axis mechanism, so the test is whether a skill carries a second dimension keying that disclosure independently of scope, or whether, like a tool, "disclose on demand" is just "scope to the triggering situation." Skills may land on either side, and saying so in advance is the prediction.

The strong, falsifiable form is never merely "you can describe it with where and when" (the universal part, cheap by the tautology objection). It is the *conditional*: **does this population have an intrinsic second dimension, and does its factoring track exactly whether it does?** That is the claim with teeth, and the one tools already illustrate by failing in the predicted way.

### What would falsify it

The strengthened falsifiers turn on the *condition*, a structural fact about a population that cannot be redescribed away by stretching "where and when." Any *one* of the following, demonstrated for a real population before a real window, falsifies the thesis.

1. **A clean two-axis factoring with NO enabling second dimension.** A population lacking any intrinsic property orthogonal to scope for the time-lever to bind to, yet factoring into two genuinely independent peer axes (time free at fixed scope, with at least one combination provably uninhabitable). It cannot be redescribed away, because "is there an intrinsic property orthogonal to scope keying the schedule" is a yes/no question about the population's own structure, not about how roomy "where and when" are. (Tools are the worked case and behave exactly as predicted: no second dimension, no clean factoring, no theorem. That makes them an internal consistency check, not independent confirmation.)

2. **An enabling second dimension that does NOT yield a clean factoring.** A population that *does* carry an intrinsic, scope-orthogonal property keying its time-lever, yet fails to factor into two independent peer axes (time stays pinned to scope despite the dimension). This breaks the "if and only if" from the other side, showing the condition necessary but not sufficient.

3. **A genuine third economizing lever.** An examined population whose downstream-checkable scheme has a *third* independently-checkable, violable attribute that genuinely *decides slot-occupancy*, a real third coordinate, not merely a *label*. (Content-type is checkable and violable but does not decide occupancy, so it is the enabling dimension, not a third lever, and does not falsify.) If organizing a population demonstrably needs a third orthogonal *occupancy*-deciding lever, "two economizing axes" is dead. This is the exhaustiveness criterion turned into a falsifier.

4. **The situation sink overflowing.** A demonstrated coordinate of a provisioning that the criterion cannot file as scope (a stable feature of the world), time (a feature of the element's schedule), or input (a heuristic feeding the two), and that must be conditioned on for the occupancy decision. Because exhaustiveness is *argued, not proven*, with "situation" an admitted elastic sink, this falsifier is reachable: it is the joint giving way. It would not show the universal economy wrong so much as show its *exhaustiveness* over-argued, a third coordinate "situation" was quietly absorbing.

5. **A window without the two structural properties.** A future architecture that relaxes boundedness (an unbounded window where nothing competes, the principle vacuous rather than false), or relaxes all-at-once provisioning, or exposes a *third addressing coordinate* beyond situation and moment (which would admit a real third axis per the elimination). This bounds the claim's reach rather than refuting it, but honesty requires flagging the condition under which the principle stops applying rather than being wrong.

What does *not* falsify it: a population you *can* describe with scope and time (the universal part succeeding, *provided* its factoring claim is also borne out); heuristics like priority or recency (inputs, predicted); compression or summarization (within the time setting, not beside it); a *checkable label* that does not decide occupancy (an enabling dimension or descriptive tag, content-type being the worked example, not a third lever).

## Epistemic status

The parts of this paper have different standing, and conflating them would oversell the work. v1's failure was an altitude failure: it stated an argued framework with theorem-like confidence. This version keeps the two halves at different altitudes.

**What is argued (framework).** That the window is a bounded, per-call, all-at-once slot-set; that economizing it is universally scope by time; that the economy factors into two peer axes exactly when the population carries an intrinsic second dimension keying the time-lever; and that files and agents meet that condition while tools fail it. Within the framework the parts differ in firmness. The *minimum* (one axis cannot economize a population whose levers separate) is firmest. The *condition* (enabling dimension iff clean factoring) is well-supported: two populations confirm by meeting it, one bounds by failing it. **Exhaustiveness, that there is no *third* economizing lever, is the softest part, argued not proven**, because its (situation, moment) premise leans on an elastic "situation." The softest framework sub-claim therefore carries its conjectural status as openly as the generalization does. That is the explicit correction of v1's overclaim.

**What is conjectured (generalization).** That the same conditional structure recurs for populations not yet examined: memories, connections, skills, any future element. Three instances are consistent with it and stronger than v1's two, because the third tests the *condition* rather than re-confirming the pattern. But three instances do not establish a universal, and the paper does not pretend they do.

The line between the halves is the line between "this is how the three populations fit under one economy with one condition, argued" and "this is what I bet the next population will show, conjectured." The first is defended, its softest joint flagged; the second is a wager with its losing conditions written down. That conditional claim is more interesting and more falsifiable than the flat "exactly two, everywhere" v1 overreached for, and naming its altitude honestly is part of the house discipline both companions keep.

## Adjacent concepts

**Context as Architecture (companion, this repository).** The first corollary: this principle applied to files, which factor cleanly *because* they carry content-type. Its axes are this root's scope (routing hierarchy) and time (load pattern); its third checkable attribute (content-type) is this root's enabling second dimension, not a third lever and (contra v1) not a tag to wave away. Its fractal property is the independence of the two axes made visible.

**The agent-role topology (companion, this repository).** The second corollary: this principle applied to agents, which factor cleanly *because* they carry lifecycle. Its time axis (lifecycle) is the cleaner cross-population invariant; its first axis (Position) is scope-*like*, with the routing hierarchy genuinely shared as the level property. Its empty-cell theorem is a corollary of a first-axis value (Lead) pinning a time value (standing). The topology's own closing conjecture is the seed this paper grows into a root, and sharpens with a condition.

**ICM (Van Clief and McDermott).** The grandparent methodology, reached through *Context as Architecture*. ICM's five layers are, per the companion, two axes; the ICM SPEC's file linter checks three independent attributes (routing-level, content-type, load-pattern), the concrete source of this paper's content-type-as-enabling-dimension argument. This paper sits above ICM by abstracting the *resource* (the window) rather than the *population* (files).

**Caching and the working set.** The deepest technical analogy, and the one that motivates the "finite, all-at-once, re-provisioned per call" framing. A cache is a fixed-capacity slot-set refilled per access cycle; the working-set model (Denning) characterizes which pages must be resident for a process to run without thrashing. The window is a working set for an inference call. Caching's two classic levers, *what to admit* (scope) and *what to evict, how long to keep* (time), are the two axes in systems-programming form. The "lost in the middle" effect is the window's version of cache pressure. The analogy is load-bearing, not decorative: it is *why* the two-lever economics is expected. It reaches the refined thesis too: even in caching, separating admission from retention relies on lines carrying tags and reference-bits, a second dimension; a cache with no per-line metadata would, like tools, collapse retention into admission.

**Rate-distortion and information theory.** With a finite channel (the token budget) you cannot transmit everything, so you choose what to send to minimize distortion (lost task-relevant information) under a rate constraint. Scope and time are the two degrees of freedom for spending a rate budget across situations and moments. Offered as a lens, not a derivation: the *shape* of the problem (finite channel, choose what to send) is the same, consistent with the two-lever conclusion.

**Progressive disclosure.** Norman, Tognazzini, and the UI tradition: surface what is needed now, hide what is not. This is the time axis for the human-attention resource; a single screen is a finite all-at-once slot-set for a person, refilled per interaction. The companion notes progressive disclosure as its load-pattern axis "applied against finite context windows." This root makes the connection structural: human attention and the window are *both* finite all-at-once slot-sets, both economized scope-by-time. (UI elements typically carry a content-kind, a second dimension, which is why disclosure runs free of placement; the enabling-dimension story ports here too.)

**Adjacent but distinct: prompt engineering and compression.** Both operate *within* the framework. Prompt engineering optimizes the content of slots already allocated for one call, downstream of the decision about *which* elements get slots. Compression and summarization change *how much* of an element occupies its slot, an implementation within a time setting ("present a smaller version, now"), not a third axis. Neither is a counterexample; both are inside.

## Glossary

The load-bearing terms; fuller treatment in the body.

**Provisioning.** One filling of the window for one inference call, identified by the *situation* (fixes scope) and the *moment* (fixes time). That it has only these two coordinates is the argued premise of exhaustiveness; "situation" is the elastic term flagged as the soft joint.

**Scope ("where").** Route an element so it occupies slots only in the situations where it is relevant. A *partition* of situation-space. A genuine axis for every population examined; the dominant economization for tools.

**Time ("when").** Schedule an element so it occupies slots only when relevant, and only for as long as: always / on-demand / per-item. A *schedule* over moments. A genuine peer axis only when an enabling second dimension keys it; collapses into scope without one.

**Enabling second dimension.** An intrinsic property of an element, orthogonal to scope, that the time-lever reads so the element's *when* varies independently of its *where*. Content-type for files; lifecycle for agents. The condition for the clean form: a population factors into two peer axes exactly when it has one. *Not* an economizing lever (it does not decide slot-occupancy), and sharply distinct from a hypothetical *third economizing lever*, which would be a real third coordinate of the provisioning and which the framework predicts does not exist.

**Independence.** Fixing one axis leaves the other free. *Earned, not automatic*: it needs an enabling second dimension. A pinned value at one axis-value (Lead forces standing) is a *constraint between independent axes*, not a loss of independence.

**Exhaustiveness.** That scope and time together fully economize the window, no third lever. **Argued, not proven**: the elimination rests on the (situation, moment) premise, whose "situation" can absorb candidate third coordinates. The softest part of the framework half.

**Bounding case / one-and-a-half axes.** A population that meets the universal economy but *fails* the condition: it obeys scope cleanly, but its time-lever collapses into scope for lack of a second dimension, so no empty-cell theorem reproduces. Tools are the worked instance. The boundary proves the rule.

**Population.** A class of elements of one kind before the window: files, agents, tools, memories, connections, skills.

**Content-type / lifecycle / Position.** The population-specific terms. *Content-type*: the file population's enabling dimension (identity / situational / reference / working), keying the load pattern. *Lifecycle*: the agent population's time axis *and* enabling dimension (standing / ephemeral). *Position*: the agent population's first axis (Lead / Executor), a functional role that is *scope-like* but not a literal partition of situation-space, carrying the shared level property.

**Working set.** Denning's model of the pages a process must keep resident to avoid thrashing. The window is a working set for an inference call; its levers (what to admit, how long to retain) are scope and time in systems-programming form.

## References

Denning, P. J. (1968). The Working Set Model for Program Behavior. Communications of the ACM, 11(5), 323-333.

Liu, N. F., Lin, K., Hewitt, J., Paranjape, A., Bevilacqua, M., Petroni, F., and Liang, P. (2024). Lost in the Middle: How Language Models Use Long Contexts. Transactions of the Association for Computational Linguistics, 12, 157-173.

Norman, D. A. (1988). The Design of Everyday Things. Basic Books.

Shannon, C. E. (1948). A Mathematical Theory of Communication. Bell System Technical Journal, 27(3), 379-423.

Shannon, C. E. (1959). Coding Theorems for a Discrete Source with a Fidelity Criterion. IRE National Convention Record, Part 4, 142-163.

Van Clief, J., and McDermott, D. (2026). Interpretable Context Methodology: Folder Structure as Agentic Architecture. arXiv:2603.16021. https://arxiv.org/abs/2603.16021

usebessemer/research. Context as Architecture. (companion paper, this repository).

usebessemer/research. The agent-role topology. (companion paper, this repository).

